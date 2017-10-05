---
title: "Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és MOVEit átadás - integráció az Azure AD között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: d35aceb9be2d0ff49f86a00cc84f5deb198d88f0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a><span data-ttu-id="ca7cb-103">Oktatóanyag: Azure Active Directoryval integrált MOVEit átadás - Azure AD-integrációs</span><span class="sxs-lookup"><span data-stu-id="ca7cb-103">Tutorial: Azure Active Directory integration with MOVEit Transfer - Azure AD integration</span></span>

<span data-ttu-id="ca7cb-104">Ebben az oktatóanyagban elsajátíthatja MOVEit átadás - Azure AD integrálása az Azure Active Directoryval (Azure AD) integrálása.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-104">In this tutorial, you learn how to integrate MOVEit Transfer - Azure AD integration with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ca7cb-105">MOVEit átadás - Azure AD integrálása az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-105">Integrating MOVEit Transfer - Azure AD integration with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ca7cb-106">Az Azure AD, aki hozzáfér MOVEit átadás - Azure AD-integrációs szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-106">You can control in Azure AD who has access to MOVEit Transfer - Azure AD integration.</span></span>
- <span data-ttu-id="ca7cb-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett MOVEit átviteléig – az Azure AD integrálása (egyszeri bejelentkezés) az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-107">You can enable your users to automatically get signed-on to MOVEit Transfer - Azure AD integration (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="ca7cb-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="ca7cb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ca7cb-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ca7cb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ca7cb-110">Prerequisites</span></span>

<span data-ttu-id="ca7cb-111">Konfigurálása az Azure AD-integrációs MOVEit átadás - Azure AD-integrációs, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-111">To configure Azure AD integration with MOVEit Transfer - Azure AD integration, you need the following items:</span></span>

- <span data-ttu-id="ca7cb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ca7cb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ca7cb-113">A MOVEit átvitel – az Azure AD integrálása egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ca7cb-113">A MOVEit Transfer - Azure AD integration single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ca7cb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ca7cb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ca7cb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="ca7cb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ca7cb-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ca7cb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ca7cb-118">Scenario description</span></span>
<span data-ttu-id="ca7cb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ca7cb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ca7cb-121">Hozzáadás MOVEit átadás - Azure AD integrálása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ca7cb-121">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
2. <span data-ttu-id="ca7cb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ca7cb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a><span data-ttu-id="ca7cb-123">Hozzáadás MOVEit átadás - Azure AD integrálása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="ca7cb-123">Adding MOVEit Transfer - Azure AD integration from the gallery</span></span>
<span data-ttu-id="ca7cb-124">A MOVEit átadás - az Azure AD integrálása az Azure AD-integráció konfigurálása kell hozzáadnia a MOVEit átadás - az Azure AD integrálása a gyűjteményből listájára felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-124">To configure the integration of MOVEit Transfer - Azure AD integration into Azure AD, you need to add MOVEit Transfer - Azure AD integration from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ca7cb-125">**Adja hozzá a MOVEit átadás - Azure AD integrálása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ca7cb-125">**To add MOVEit Transfer - Azure AD integration from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ca7cb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="ca7cb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ca7cb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="ca7cb-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="ca7cb-133">Írja be a keresőmezőbe, **MOVEit átadás - Azure AD-integrációs**, jelölje be **MOVEit átadás - Azure AD-integrációs** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel a az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-133">In the search box, type **MOVEit Transfer - Azure AD integration**, select **MOVEit Transfer - Azure AD integration** from result panel then click **Add** button to add the application.</span></span>

    ![MOVEit átadás - Azure AD integrálása az eredménylistában](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="ca7cb-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ca7cb-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="ca7cb-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a MOVEit átadás - Azure AD-integrációs "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-136">In this section, you configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ca7cb-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó MOVEit átadás - Azure AD-integrációs a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-137">For single sign-on to work, Azure AD needs to know what the counterpart user in MOVEit Transfer - Azure AD integration is to a user in Azure AD.</span></span> <span data-ttu-id="ca7cb-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a MOVEit átadás - hivatkozás kapcsolata az Azure AD-integrációs kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-138">In other words, a link relationship between an Azure AD user and the related user in MOVEit Transfer - Azure AD integration needs to be established.</span></span>

<span data-ttu-id="ca7cb-139">Az MOVEit átviteli - Azure AD-integrációs, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-139">In MOVEit Transfer - Azure AD integration, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="ca7cb-140">Az Azure AD egyszeri bejelentkezést a MOVEit átadás - tesztelése és konfigurálása az Azure AD-integrációs kell végrehajtani a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-140">To configure and test Azure AD single sign-on with MOVEit Transfer - Azure AD integration, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ca7cb-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ca7cb-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ca7cb-143">**[Hozzon létre egy MOVEit átadás - az Azure AD-integrációs tesztfelhasználó](#create-a-moveit-transfer---azure-ad-integration-test-user)**  - való egy megfelelője a Britta Simon MOVEit átadás - Azure AD-integrációs, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-143">**[Create a MOVEit Transfer - Azure AD integration test user](#create-a-moveit-transfer---azure-ad-integration-test-user)** - to have a counterpart of Britta Simon in MOVEit Transfer - Azure AD integration that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="ca7cb-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ca7cb-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="ca7cb-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ca7cb-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="ca7cb-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a MOVEit átadás - Azure AD-integrációs alkalmazást az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MOVEit Transfer - Azure AD integration application.</span></span>

<span data-ttu-id="ca7cb-148">**Az Azure AD konfigurálása egyszeri bejelentkezéshez az MOVEit átadás - Azure AD-integrációs, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ca7cb-148">**To configure Azure AD single sign-on with MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="ca7cb-149">Az Azure portálon a a **MOVEit átadás - Azure AD-integrációs** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-149">In the Azure portal, on the **MOVEit Transfer - Azure AD integration** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="ca7cb-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_samlbase.png)

3. <span data-ttu-id="ca7cb-153">Az a **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-153">On the **MOVEit Transfer - Azure AD integration Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_url.png)

    <span data-ttu-id="ca7cb-155">a.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-155">a.</span></span> <span data-ttu-id="ca7cb-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://contoso.com`</span><span class="sxs-lookup"><span data-stu-id="ca7cb-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://contoso.com`</span></span>

    <span data-ttu-id="ca7cb-157">b.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-157">b.</span></span> <span data-ttu-id="ca7cb-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://contoso.com/<tenatid>`</span><span class="sxs-lookup"><span data-stu-id="ca7cb-158">In the **Identifier** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>`</span></span>

    <span data-ttu-id="ca7cb-159">c.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-159">c.</span></span> <span data-ttu-id="ca7cb-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span><span class="sxs-lookup"><span data-stu-id="ca7cb-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://contoso.com/<tenatid>/SAML/SSO/HTTP-Post`</span></span>    
     
    > [!NOTE] 
    > <span data-ttu-id="ca7cb-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-161">These values are not real.</span></span> <span data-ttu-id="ca7cb-162">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="ca7cb-163">Olvassa el ezeket az értékeket később a **szolgáltató metaadatainak URL-címe** szakaszban, vagy forduljon a [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-163">You can refer these values later in **Service Provider Metadata URL** section or contact [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support) to get these values.</span></span>

4. <span data-ttu-id="ca7cb-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_certificate.png) 

5. <span data-ttu-id="ca7cb-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="ca7cb-168">Jelentkezzen be rendszergazdaként a MOVEit átviteli bérlő.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-168">Sign on to your MOVEit Transfer tenant as an administrator.</span></span>

7. <span data-ttu-id="ca7cb-169">A bal oldali navigációs ablaktábláján kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-169">On the left navigation pane, click **Settings**.</span></span>

    ![Beállítások szakaszban az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_000.png)

8. <span data-ttu-id="ca7cb-171">Kattintson a **egyszeri bejelentkezés** hivatkozás, amely alatt **biztonsági házirendek -> felhasználói hitelesítési**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-171">Click **Single Signon** link, which is under **Security Policies -> User Auth**.</span></span>

    ![Biztonsági házirendek az alkalmazás ügyféloldali](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_001.png)

9. <span data-ttu-id="ca7cb-173">Kattintson a metaadatok hivatkozásra kattintva töltse le a metaadat-dokumentum.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-173">Click the Metadata URL link to download the metadata document.</span></span>

    ![Szolgáltató metaadatainak URL-címe](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
    * <span data-ttu-id="ca7cb-175">Győződjön meg arról **entityid beállítást** megfelelő **azonosító** a a **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-175">Verify **entityID** matches **Identifier** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section .</span></span>
    * <span data-ttu-id="ca7cb-176">Győződjön meg arról **AssertionConsumerService** hely URL-címe megegyezik **válasz URL-CÍMEN** a a **MOVEit átviteli - tartomány az Azure AD-integrációs és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-176">Verify **AssertionConsumerService** Location URL matches **REPLY URL** in the **MOVEit Transfer - Azure AD integration Domain and URLs** section.</span></span>
    
    ![Egyszeri bejelentkezés az alkalmazás ügyféloldali konfigurálása](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_007.png)

10. <span data-ttu-id="ca7cb-178">Kattintson a **identitásszolgáltató hozzáadása** új összevont identitás szolgáltató hozzáadása gomb.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-178">Click **Add Identity Provider** button to add a new Federated Identity Provider.</span></span>

    ![Identitás-szolgáltató felvétele](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_003.png)

11. <span data-ttu-id="ca7cb-180">Kattintson a **Tallózás...**  válassza ki a metaadatok fájlt, amely az Azure-portálról letöltött, majd kattintson a **identitásszolgáltató hozzáadása** fel kell töltenie a letöltött fájlt.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-180">Click **Browse...** to select the metadata file which you downloaded from Azure portal, then click **Add Identity Provider** to upload the downloaded file.</span></span>

    ![SAML-Identitásszolgáltatóként](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_004.png)

12. <span data-ttu-id="ca7cb-182">Jelölje ki "**Igen**" mint **engedélyezett** a a **összevont identitás Szolgáltatóbeállítások szerkesztése...**  lapot, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-182">Select "**Yes**" as **Enabled** in the **Edit Federated Identity Provider Settings...** page and click **Save**.</span></span>

    ![Összevont identitás-szolgáltató beállításai](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_005.png)

13. <span data-ttu-id="ca7cb-184">Az a **összevont identitás szolgáltató felhasználói beállítások szerkesztése** lapon, a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-184">In the **Edit Federated Identity Provider User Settings** page, perform the following actions:</span></span>
    
    ![Összevont identitás Szolgáltatóbeállítások szerkesztése](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    <span data-ttu-id="ca7cb-186">a.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-186">a.</span></span> <span data-ttu-id="ca7cb-187">Válassza ki **SAML NameID** , **bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-187">Select **SAML NameID** as **Login name**.</span></span>
    
    <span data-ttu-id="ca7cb-188">b.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-188">b.</span></span> <span data-ttu-id="ca7cb-189">Válassza ki **más** , **teljes nevét** és a a **attribútumnév** szövegmezőbe írja be az értéket: `http://schemas.microsoft.com/identity/claims/displayname`.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-189">Select **Other** as **Full name** and in the **Attribute name** textbox put the value: `http://schemas.microsoft.com/identity/claims/displayname`.</span></span>
    
    <span data-ttu-id="ca7cb-190">c.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-190">c.</span></span> <span data-ttu-id="ca7cb-191">Válassza ki **más** , **E-mail** és a a **attribútumnév** szövegmezőbe írja be az értéket: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-191">Select **Other** as **Email** and in the **Attribute name** textbox put the value: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>
    
    <span data-ttu-id="ca7cb-192">d.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-192">d.</span></span> <span data-ttu-id="ca7cb-193">Válassza ki **Igen** , **fiók automatikus létrehozása frissítsen**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-193">Select **Yes** as **Auto-create account on signon**.</span></span>
    
    <span data-ttu-id="ca7cb-194">e.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-194">e.</span></span> <span data-ttu-id="ca7cb-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-195">Click **Save** button.</span></span>

> [!TIP]
> <span data-ttu-id="ca7cb-196">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ca7cb-196">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ca7cb-197">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-197">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ca7cb-198">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ca7cb-198">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="ca7cb-199">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="ca7cb-199">Create an Azure AD test user</span></span>

<span data-ttu-id="ca7cb-200">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="ca7cb-202">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ca7cb-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ca7cb-203">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-203">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="ca7cb-205">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-205">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="ca7cb-207">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-207">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="ca7cb-209">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ca7cb-209">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-moveittransfer-tutorial/create_aaduser_04.png)

    <span data-ttu-id="ca7cb-211">a.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-211">a.</span></span> <span data-ttu-id="ca7cb-212">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-212">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ca7cb-213">b.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-213">b.</span></span> <span data-ttu-id="ca7cb-214">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-214">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="ca7cb-215">c.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-215">c.</span></span> <span data-ttu-id="ca7cb-216">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-216">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="ca7cb-217">d.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-217">d.</span></span> <span data-ttu-id="ca7cb-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-218">Click **Create**.</span></span>
 
### <a name="create-a-moveit-transfer---azure-ad-integration-test-user"></a><span data-ttu-id="ca7cb-219">Hozzon létre egy MOVEit átadás - Azure AD-integrációs teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="ca7cb-219">Create a MOVEit Transfer - Azure AD integration test user</span></span>

<span data-ttu-id="ca7cb-220">Ez a szakasz célja Britta Simon meghívta MOVEit átadás - Azure AD-integrációs felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-220">The objective of this section is to create a user called Britta Simon in MOVEit Transfer - Azure AD integration.</span></span> <span data-ttu-id="ca7cb-221">MOVEit átadás - Azure AD-integrációs támogatja közvetlenül az időponthoz kötött kiosztást, amelyhez engedélyezte.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-221">MOVEit Transfer - Azure AD integration supports just-in-time provisioning, which you have enabled.</span></span> <span data-ttu-id="ca7cb-222">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-222">There is no action item for you in this section.</span></span> <span data-ttu-id="ca7cb-223">Új felhasználó jön létre az MOVEit átviteli – Ha még nem létezik az Azure AD-integrációs elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-223">A new user is created during an attempt to access MOVEit Transfer - Azure AD integration if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="ca7cb-224">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [MOVEit átadás - az Azure AD integrálása ügyfél támogatási csoport](https://community.ipswitch.com/s/support).</span><span class="sxs-lookup"><span data-stu-id="ca7cb-224">If you need to create a user manually, you need to contact the [MOVEit Transfer - Azure AD integration Client support team](https://community.ipswitch.com/s/support).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="ca7cb-225">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="ca7cb-225">Assign the Azure AD test user</span></span>

<span data-ttu-id="ca7cb-226">Ebben a szakaszban Britta Simon használandó Azure egyszeri bejelentkezés által biztosított hozzáférés MOVEit átadás - Azure AD-integrációs engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to MOVEit Transfer - Azure AD integration.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="ca7cb-228">**Britta Simon hozzárendelése MOVEit átadás - Azure AD-integrációs, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ca7cb-228">**To assign Britta Simon to MOVEit Transfer - Azure AD integration, perform the following steps:**</span></span>

1. <span data-ttu-id="ca7cb-229">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ca7cb-231">Az alkalmazások listában válassza ki a **MOVEit átadás - Azure AD-integrációs**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-231">In the applications list, select **MOVEit Transfer - Azure AD integration**.</span></span>

    ![A MOVEit átadás - Azure AD-integrációs hivatkozásra az alkalmazások listáját](./media/active-directory-saas-moveittransfer-tutorial/tutorial_moveittransfer_app.png)  

3. <span data-ttu-id="ca7cb-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-233">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="ca7cb-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-235">Click **Add** button.</span></span> <span data-ttu-id="ca7cb-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="ca7cb-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ca7cb-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ca7cb-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="ca7cb-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ca7cb-241">Test single sign-on</span></span>

<span data-ttu-id="ca7cb-242">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-242">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="ca7cb-243">A MOVEit átadás - kattintva az Azure AD-integrációs csempét a hozzáférési panelen, meg kell beolvasása automatikusan aláírt-a MOVEit átviteléig - Azure AD-integrációs alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ca7cb-243">When you click the MOVEit Transfer - Azure AD integration tile in the Access Panel, you should get automatically signed-on to your MOVEit Transfer - Azure AD integration application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="ca7cb-244">További források</span><span class="sxs-lookup"><span data-stu-id="ca7cb-244">Additional resources</span></span>

* [<span data-ttu-id="ca7cb-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ca7cb-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ca7cb-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ca7cb-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-moveittransfer-tutorial/tutorial_general_203.png

