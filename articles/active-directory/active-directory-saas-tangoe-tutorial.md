---
title: "Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Tangoe parancs prémium Mobile között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2b0b544c-9c2c-49cd-862b-ec2ee9330126
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 595541e7248a7486e58123927389c552af0e50f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-tangoe-command-premium-mobile"></a><span data-ttu-id="69e8b-103">Oktatóanyag: Azure Active Directoryval integrált Tangoe parancs prémium Mobile</span><span class="sxs-lookup"><span data-stu-id="69e8b-103">Tutorial: Azure Active Directory integration with Tangoe Command Premium Mobile</span></span>

<span data-ttu-id="69e8b-104">Ebben az oktatóanyagban elsajátíthatja Tangoe parancs prémium Mobile integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="69e8b-104">In this tutorial, you learn how to integrate Tangoe Command Premium Mobile with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="69e8b-105">Tangoe parancs prémium Mobile integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="69e8b-105">Integrating Tangoe Command Premium Mobile with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="69e8b-106">Megadhatja a Tangoe parancs prémium Mobile hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="69e8b-106">You can control in Azure AD who has access to Tangoe Command Premium Mobile</span></span>
- <span data-ttu-id="69e8b-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Tangoe parancs prémium Mobile (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="69e8b-107">You can enable your users to automatically get signed-on to Tangoe Command Premium Mobile (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="69e8b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="69e8b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="69e8b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="69e8b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69e8b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="69e8b-110">Prerequisites</span></span>

<span data-ttu-id="69e8b-111">Konfigurálása az Azure AD-integrációs Tangoe parancs prémium Mobile, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="69e8b-111">To configure Azure AD integration with Tangoe Command Premium Mobile, you need the following items:</span></span>

- <span data-ttu-id="69e8b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="69e8b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="69e8b-113">Egy Tangoe parancs prémium Mobile egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="69e8b-113">A Tangoe Command Premium Mobile single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="69e8b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="69e8b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="69e8b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="69e8b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="69e8b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="69e8b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="69e8b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="69e8b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="69e8b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="69e8b-118">Scenario description</span></span>
<span data-ttu-id="69e8b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="69e8b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="69e8b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="69e8b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="69e8b-121">Adja hozzá a Tangoe parancs prémium Mobile a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="69e8b-121">Add Tangoe Command Premium Mobile from the gallery</span></span>
2. <span data-ttu-id="69e8b-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69e8b-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-tangoe-command-premium-mobile-from-the-gallery"></a><span data-ttu-id="69e8b-123">Adja hozzá a Tangoe parancs prémium Mobile a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="69e8b-123">Add Tangoe Command Premium Mobile from the gallery</span></span>
<span data-ttu-id="69e8b-124">Az Azure AD integrálása a Tangoe parancs prémium Mobile konfigurálásához kell hozzáadnia Tangoe parancs prémium Mobile a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="69e8b-124">To configure the integration of Tangoe Command Premium Mobile into Azure AD, you need to add Tangoe Command Premium Mobile from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="69e8b-125">**A gyűjteményből Tangoe parancs prémium Mobile hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="69e8b-125">**To add Tangoe Command Premium Mobile from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="69e8b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="69e8b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="69e8b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="69e8b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="69e8b-133">Írja be a keresőmezőbe, **Tangoe parancs prémium Mobile**, jelölje be **Tangoe parancs prémium Mobile** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="69e8b-133">In the search box, type **Tangoe Command Premium Mobile**, select **Tangoe Command Premium Mobile** from result panel then click **Add** button to add the application.</span></span>

    ![<span data-ttu-id="69e8b-134">Adja hozzá a Tangoe parancs prémium mobil gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="69e8b-134">Add Tangoe Command Premium Mobile from gallery</span></span> ](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="69e8b-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69e8b-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="69e8b-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Tangoe parancs prémium Mobile "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="69e8b-136">In this section, you configure and test Azure AD single sign-on with Tangoe Command Premium Mobile based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="69e8b-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Tangoe parancs prémium Mobile tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="69e8b-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Tangoe Command Premium Mobile is to a user in Azure AD.</span></span> <span data-ttu-id="69e8b-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Tangoe parancs prémium Mobile közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="69e8b-138">In other words, a link relationship between an Azure AD user and the related user in Tangoe Command Premium Mobile needs to be established.</span></span>

<span data-ttu-id="69e8b-139">A Tangoe parancs prémium Mobile, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="69e8b-139">In Tangoe Command Premium Mobile, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="69e8b-140">Az Azure AD egyszeri bejelentkezést és a Tangoe parancs prémium Mobile tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="69e8b-140">To configure and test Azure AD single sign-on with Tangoe Command Premium Mobile, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="69e8b-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="69e8b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="69e8b-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="69e8b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="69e8b-143">**[Tangoe parancs prémium Mobile tesztfelhasználó létrehozása](#create-a-tangoe-command-premium-mobile-test-user)**  - való egy megfelelője a Britta Simon Tangoe parancs prémium mobileszköz, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="69e8b-143">**[Create a Tangoe Command Premium Mobile test user](#create-a-tangoe-command-premium-mobile-test-user)** - to have a counterpart of Britta Simon in Tangoe Command Premium Mobile that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="69e8b-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="69e8b-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="69e8b-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="69e8b-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="69e8b-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="69e8b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="69e8b-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az Tangoe parancs prémium Mobile alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="69e8b-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Tangoe Command Premium Mobile application.</span></span>

<span data-ttu-id="69e8b-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása Tangoe parancs prémium Mobile, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="69e8b-148">**To configure Azure AD single sign-on with Tangoe Command Premium Mobile, perform the following steps:**</span></span>

1. <span data-ttu-id="69e8b-149">Az Azure portálon a a **Tangoe parancs prémium Mobile** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-149">In the Azure portal, on the **Tangoe Command Premium Mobile** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="69e8b-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="69e8b-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_samlbase.png)

3. <span data-ttu-id="69e8b-153">Az a **Tangoe parancs prémium Mobile tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="69e8b-153">On the **Tangoe Command Premium Mobile Domain and URLs** section, perform the following steps:</span></span>

    ![Tangoe parancs prémium mobil tartomány és az URL-címek](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_url.png)

    <span data-ttu-id="69e8b-155">a.</span><span class="sxs-lookup"><span data-stu-id="69e8b-155">a.</span></span> <span data-ttu-id="69e8b-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span><span class="sxs-lookup"><span data-stu-id="69e8b-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.tangoe.com/sp/startSSO.ping?PartnerIdpId=<tenant issuer>&TARGET=<target page url>`</span></span>

    <span data-ttu-id="69e8b-157">b.</span><span class="sxs-lookup"><span data-stu-id="69e8b-157">b.</span></span> <span data-ttu-id="69e8b-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.tangoe.com/sp/ACS.saml2`</span><span class="sxs-lookup"><span data-stu-id="69e8b-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://sso.tangoe.com/sp/ACS.saml2`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="69e8b-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="69e8b-159">These values are not real.</span></span> <span data-ttu-id="69e8b-160">Frissítheti ezeket az értékeket a tényleges válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="69e8b-160">Update these values with the actual  Reply URL and Sign-On URL.</span></span> <span data-ttu-id="69e8b-161">Ügyfél [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="69e8b-161">Contact [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) to get these values.</span></span> 

4. <span data-ttu-id="69e8b-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="69e8b-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_certificate.png) 

5. <span data-ttu-id="69e8b-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-164">Click **Save** button.</span></span>

    ![Mentés gombja](./media/active-directory-saas-tangoe-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="69e8b-166">A a **Tangoe prémium mobilalkalmazás konfigurálása** kattintson **Tangoe parancsot konfigurálása prémium szintű Mobile** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="69e8b-166">On the **Tangoe Command Premium Mobile Configuration** section, click **Configure Tangoe Command Premium Mobile** to open **Configure sign-on** window.</span></span> <span data-ttu-id="69e8b-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="69e8b-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Tangoe parancs prémium Mobile konfigurációs szakasz](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_configure.png) 

7. <span data-ttu-id="69e8b-169">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba a [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) , és adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="69e8b-169">To get SSO configured for your application, contact your [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) and provide the following:</span></span>

   - <span data-ttu-id="69e8b-170">A letöltött metaadatait tartalmazó fájl</span><span class="sxs-lookup"><span data-stu-id="69e8b-170">The downloaded metadata file</span></span>
   - <span data-ttu-id="69e8b-171">A **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="69e8b-171">The **SAML Entity ID**</span></span>
   - <span data-ttu-id="69e8b-172">A **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-címe**</span><span class="sxs-lookup"><span data-stu-id="69e8b-172">The **SAML Single Sign-On Service URL**</span></span>
   - <span data-ttu-id="69e8b-173">A **kijelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="69e8b-173">The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="69e8b-174">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="69e8b-174">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="69e8b-175">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="69e8b-175">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="69e8b-176">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="69e8b-176">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="69e8b-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="69e8b-177">Create an Azure AD test user</span></span>
<span data-ttu-id="69e8b-178">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="69e8b-178">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="69e8b-180">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="69e8b-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="69e8b-181">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-181">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="69e8b-183">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-183">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-tangoe-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="69e8b-185">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="69e8b-185">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Felhasználó hozzáadása](./media/active-directory-saas-tangoe-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="69e8b-187">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="69e8b-187">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-tangoe-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="69e8b-189">a.</span><span class="sxs-lookup"><span data-stu-id="69e8b-189">a.</span></span> <span data-ttu-id="69e8b-190">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-190">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="69e8b-191">b.</span><span class="sxs-lookup"><span data-stu-id="69e8b-191">b.</span></span> <span data-ttu-id="69e8b-192">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="69e8b-192">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="69e8b-193">c.</span><span class="sxs-lookup"><span data-stu-id="69e8b-193">c.</span></span> <span data-ttu-id="69e8b-194">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-194">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="69e8b-195">d.</span><span class="sxs-lookup"><span data-stu-id="69e8b-195">d.</span></span> <span data-ttu-id="69e8b-196">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-196">Click **Create**.</span></span>
 
### <a name="create-a-tangoe-command-premium-mobile-test-user"></a><span data-ttu-id="69e8b-197">Tangoe parancs prémium Mobile tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="69e8b-197">Create a Tangoe Command Premium Mobile test user</span></span>

<span data-ttu-id="69e8b-198">Ebben a szakaszban Tangoe parancs prémium Mobile Britta Simon nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="69e8b-198">In this section, you create a user called Britta Simon in Tangoe Command Premium Mobile.</span></span> 

<span data-ttu-id="69e8b-199">Tangoe parancs prémium Mobile alkalmazást úgy kell létrehozni, az alkalmazásban, az egyszeri bejelentkezés végrehajtása előtt minden felhasználó kell.</span><span class="sxs-lookup"><span data-stu-id="69e8b-199">Tangoe Command Premium Mobile application needs all the users to be provisioned in the application before doing Single Sign On.</span></span> <span data-ttu-id="69e8b-200">Ezért kérjük, munkahelyi és a [Tangoe parancs prémium mobileszköz ügyfél-támogatási csoport](https://www.tangoe.com/contact-2/) ezek a felhasználók az alkalmazás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="69e8b-200">So please work with the [Tangoe Command Premium Mobile Client support team](https://www.tangoe.com/contact-2/) to provision all these users into the application.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="69e8b-201">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="69e8b-201">Assign the Azure AD test user</span></span>

<span data-ttu-id="69e8b-202">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Tangoe parancs prémium Mobile Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="69e8b-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Tangoe Command Premium Mobile.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="69e8b-204">**Britta Simon hozzárendelése Tangoe parancs prémium Mobile, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="69e8b-204">**To assign Britta Simon to Tangoe Command Premium Mobile, perform the following steps:**</span></span>

1. <span data-ttu-id="69e8b-205">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="69e8b-207">Az alkalmazások listában válassza ki a **Tangoe parancs prémium Mobile**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-207">In the applications list, select **Tangoe Command Premium Mobile**.</span></span>

    ![Tangoe parancs prémium Mobile alkalmazáslistában](./media/active-directory-saas-tangoe-tutorial/tutorial_tangoe_app.png) 

3. <span data-ttu-id="69e8b-209">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="69e8b-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="69e8b-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="69e8b-211">Click **Add** button.</span></span> <span data-ttu-id="69e8b-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="69e8b-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="69e8b-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="69e8b-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="69e8b-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="69e8b-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="69e8b-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="69e8b-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="69e8b-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="69e8b-217">Test single sign-on</span></span>

<span data-ttu-id="69e8b-218">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="69e8b-218">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="69e8b-219">Ha a hozzáférési panelen Tangoe parancs prémium Mobile csempére kattint, meg kell beolvasása automatikusan bejelentkezett az Tangoe parancs prémium Mobile-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="69e8b-219">When you click the Tangoe Command Premium Mobile tile in the Access Panel, you should get automatically signed-on to your Tangoe Command Premium Mobile application.</span></span> <span data-ttu-id="69e8b-220">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="69e8b-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="69e8b-221">További források</span><span class="sxs-lookup"><span data-stu-id="69e8b-221">Additional resources</span></span>

* [<span data-ttu-id="69e8b-222">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="69e8b-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="69e8b-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="69e8b-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-tangoe-tutorial/tutorial_general_203.png

