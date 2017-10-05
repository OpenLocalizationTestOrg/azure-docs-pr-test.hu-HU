---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TOPdesk - nyilvános között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: f21fe0b363776974108ff460060e4c15a51a58a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a><span data-ttu-id="fdf72-103">Oktatóanyag: Azure Active Directoryval integrált TOPdesk - nyilvános</span><span class="sxs-lookup"><span data-stu-id="fdf72-103">Tutorial: Azure Active Directory integration with TOPdesk - Public</span></span>

<span data-ttu-id="fdf72-104">Ebben az oktatóanyagban elsajátíthatja TOPdesk - nyilvános Azure Active Directory (Azure AD) integrálása.</span><span class="sxs-lookup"><span data-stu-id="fdf72-104">In this tutorial, you learn how to integrate TOPdesk - Public with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fdf72-105">TOPdesk - nyilvános az Azure AD integrálása lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fdf72-105">Integrating TOPdesk - Public with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fdf72-106">Szabályozhatja, aki hozzáfér TOPdesk - nyilvános Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fdf72-106">You can control in Azure AD who has access to TOPdesk - Public.</span></span>
- <span data-ttu-id="fdf72-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett TOPdesk - nyilvános (egyszeri bejelentkezés) a saját Azure AD-fiókok számára.</span><span class="sxs-lookup"><span data-stu-id="fdf72-107">You can enable your users to automatically get signed-on to TOPdesk - Public (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="fdf72-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="fdf72-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="fdf72-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fdf72-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fdf72-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fdf72-110">Prerequisites</span></span>

<span data-ttu-id="fdf72-111">Az Azure AD-integrációs konfigurálása TOPdesk - nyilvános, a következőkre van szüksége:</span><span class="sxs-lookup"><span data-stu-id="fdf72-111">To configure Azure AD integration with TOPdesk - Public, you need the following items:</span></span>

- <span data-ttu-id="fdf72-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fdf72-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fdf72-113">Egy TOPdesk - nyilvános egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="fdf72-113">A TOPdesk - Public single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fdf72-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fdf72-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fdf72-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fdf72-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fdf72-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fdf72-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fdf72-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fdf72-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fdf72-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fdf72-118">Scenario description</span></span>
<span data-ttu-id="fdf72-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fdf72-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fdf72-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fdf72-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fdf72-121">Hozzáadás TOPdesk - nyilvános a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fdf72-121">Adding TOPdesk - Public from the gallery</span></span>
2. <span data-ttu-id="fdf72-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fdf72-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-topdesk---public-from-the-gallery"></a><span data-ttu-id="fdf72-123">Hozzáadás TOPdesk - nyilvános a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fdf72-123">Adding TOPdesk - Public from the gallery</span></span>
<span data-ttu-id="fdf72-124">A TOPdesk - nyilvános az Azure AD-integráció konfigurálása kell hozzáadnia a TOPdesk - nyilvános a gyűjteményből, a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fdf72-124">To configure the integration of TOPdesk - Public into Azure AD, you need to add TOPdesk - Public from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fdf72-125">**Adja hozzá a TOPdesk - nyilvános a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fdf72-125">**To add TOPdesk - Public from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fdf72-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="fdf72-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fdf72-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="fdf72-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="fdf72-133">Írja be a keresőmezőbe, **TOPdesk - nyilvános**, jelölje be **TOPdesk - nyilvános** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fdf72-133">In the search box, type **TOPdesk - Public**, select **TOPdesk - Public** from result panel then click **Add** button to add the application.</span></span>

    ![TOPdesk - nyilvános az eredménylistában](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fdf72-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdf72-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="fdf72-136">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a TOPdesk tesztelése és konfigurálása – nyilvános "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="fdf72-136">In this section, you configure and test Azure AD single sign-on with TOPdesk - Public based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fdf72-137">Az egyszeri bejelentkezés használatához az Azure AD meg kell tudni, hogy milyen a párjukhoz felhasználó TOPdesk - nyilvános egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="fdf72-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TOPdesk - Public is to a user in Azure AD.</span></span> <span data-ttu-id="fdf72-138">Más szóval egy Azure AD-felhasználó és a kapcsolódó felhasználó a TOPdesk - hivatkozás kapcsolatának nyilvános kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fdf72-138">In other words, a link relationship between an Azure AD user and the related user in TOPdesk - Public needs to be established.</span></span>

<span data-ttu-id="fdf72-139">A TOPdesk - nyilvános, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fdf72-139">In TOPdesk - Public, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fdf72-140">Az Azure AD egyszeri bejelentkezést a TOPdesk - tesztelése és konfigurálása a nyilvános, végre kell hajtania a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-140">To configure and test Azure AD single sign-on with TOPdesk - Public, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fdf72-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fdf72-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fdf72-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fdf72-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fdf72-143">**[Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó](#create-a-topdesk---public-test-user)**  - való egy megfelelője a Britta Simon TOPdesk - nyilvános, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fdf72-143">**[Create a TOPdesk - Public test user](#create-a-topdesk---public-test-user)** - to have a counterpart of Britta Simon in TOPdesk - Public that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fdf72-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fdf72-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fdf72-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fdf72-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fdf72-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fdf72-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fdf72-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a TOPdesk - nyilvános alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fdf72-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TOPdesk - Public application.</span></span>

<span data-ttu-id="fdf72-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása TOPdesk - nyilvános, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fdf72-148">**To configure Azure AD single sign-on with TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="fdf72-149">Az Azure portálon a a **TOPdesk - nyilvános** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-149">In the Azure portal, on the **TOPdesk - Public** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="fdf72-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fdf72-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_samlbase.png)

3. <span data-ttu-id="fdf72-153">Az a **TOPdesk - nyilvános tartományi és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-153">On the **TOPdesk - Public Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk TOPdesk - nyilvános tartományi és URL-címek](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_url.png)

    <span data-ttu-id="fdf72-155">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-155">a.</span></span> <span data-ttu-id="fdf72-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.topdesk.net`</span><span class="sxs-lookup"><span data-stu-id="fdf72-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net`</span></span>
    
    <span data-ttu-id="fdf72-157">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-157">b.</span></span> <span data-ttu-id="fdf72-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.topdesk.net/tas/public/login/verify`</span><span class="sxs-lookup"><span data-stu-id="fdf72-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/verify`</span></span>

    <span data-ttu-id="fdf72-159">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-159">c.</span></span> <span data-ttu-id="fdf72-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.topdesk.net/tas/public/login/saml`</span><span class="sxs-lookup"><span data-stu-id="fdf72-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<companyname>.topdesk.net/tas/public/login/saml`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="fdf72-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="fdf72-161">These values are not real.</span></span> <span data-ttu-id="fdf72-162">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="fdf72-162">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="fdf72-163">Válasz URL-CÍMEN explaned az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="fdf72-163">Reply URL is explaned later in tutorial.</span></span> <span data-ttu-id="fdf72-164">Ügyfél [TOPdesk - nyilvános ügyfél-támogatási csoport](https://help.topdesk.com/saas/enterprise/user/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="fdf72-164">Contact [TOPdesk - Public Client support team](https://help.topdesk.com/saas/enterprise/user/) to get these values.</span></span>  

4. <span data-ttu-id="fdf72-165">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fdf72-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_certificate.png) 

5. <span data-ttu-id="fdf72-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="fdf72-169">A a **TOPdesk - nyilvános konfigurációs** kattintson **konfigurálása TOPdesk - nyilvános** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fdf72-169">On the **TOPdesk - Public Configuration** section, click **Configure TOPdesk - Public** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fdf72-170">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fdf72-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TOPdesk - nyilvános konfigurációja](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_configure.png) 

7. <span data-ttu-id="fdf72-172">Jelentkezzen be a **TOPdesk - nyilvános** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fdf72-172">Sign on to your **TOPdesk - Public** company site as an administrator.</span></span>

8. <span data-ttu-id="fdf72-173">Az a **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-173">In the **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="fdf72-174">![Beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="fdf72-174">![Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790598.png "Settings")</span></span>

9. <span data-ttu-id="fdf72-175">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-175">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="fdf72-176">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="fdf72-176">![Login Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790599.png "Login Settings")</span></span>

10. <span data-ttu-id="fdf72-177">Bontsa ki a **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-177">Expand the **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="fdf72-178">![Általános](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="fdf72-178">![General](./media/active-directory-saas-topdesk-public-tutorial/ic790600.png "General")</span></span>

11. <span data-ttu-id="fdf72-179">Az a **nyilvános** szakasza a **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-179">In the **Public** section of the **SAML login** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="fdf72-180">![Műszaki beállítások](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "műszaki beállításai")</span><span class="sxs-lookup"><span data-stu-id="fdf72-180">![Technical Settings](./media/active-directory-saas-topdesk-public-tutorial/ic790601.png "Technical Settings")</span></span>
   
    <span data-ttu-id="fdf72-181">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-181">a.</span></span> <span data-ttu-id="fdf72-182">Kattintson a **letöltése** a nyilvános metaadatait tartalmazó fájl letöltéséhez, és mentse helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fdf72-182">Click **Download** to download the public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="fdf72-183">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-183">b.</span></span> <span data-ttu-id="fdf72-184">Nyissa meg a letöltött metaadat-fájlt, és keresse meg a **AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="fdf72-184">Open the downloaded metadata file, and then locate the **AssertionConsumerService** node.</span></span>

    <span data-ttu-id="fdf72-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span><span class="sxs-lookup"><span data-stu-id="fdf72-185">![AssertionConsumerService](./media/active-directory-saas-topdesk-public-tutorial/ic790619.png "AssertionConsumerService")</span></span>
   
    <span data-ttu-id="fdf72-186">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-186">c.</span></span> <span data-ttu-id="fdf72-187">Másolás a **AssertionConsumerService** értékét, illessze be ezt az értéket a **válasz URL-CÍMEN** textbox **TOPdesk - nyilvános tartományi és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="fdf72-187">Copy the **AssertionConsumerService** value, paste this value in the **Reply URL** textbox in **TOPdesk - Public Domain and URLs** section.</span></span>      
   
12. <span data-ttu-id="fdf72-188">A tanúsítványfájl létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-188">To create a certificate file, perform the following steps:</span></span>
    
    <span data-ttu-id="fdf72-189">![Tanúsítvány](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "tanúsítvány")</span><span class="sxs-lookup"><span data-stu-id="fdf72-189">![Certificate](./media/active-directory-saas-topdesk-public-tutorial/ic790606.png "Certificate")</span></span>
    
    <span data-ttu-id="fdf72-190">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-190">a.</span></span> <span data-ttu-id="fdf72-191">Nyissa meg a letöltött metaadatfájl Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="fdf72-191">Open the downloaded metadata file from Azure portal.</span></span>
    
    <span data-ttu-id="fdf72-192">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-192">b.</span></span> <span data-ttu-id="fdf72-193">Bontsa ki a **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-193">Expand the **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    
    <span data-ttu-id="fdf72-194">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-194">c.</span></span> <span data-ttu-id="fdf72-195">Másolja a értékének a **x.509** csomópont.</span><span class="sxs-lookup"><span data-stu-id="fdf72-195">Copy the value of the **X509Certificate** node.</span></span>
    
    <span data-ttu-id="fdf72-196">d.</span><span class="sxs-lookup"><span data-stu-id="fdf72-196">d.</span></span> <span data-ttu-id="fdf72-197">Mentse a másolt **x.509** érték helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fdf72-197">Save the copied **X509Certificate** value locally on your computer in a file.</span></span>

13. <span data-ttu-id="fdf72-198">Az a **nyilvános** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-198">In the **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="fdf72-199">![SAML bejelentkezési](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML-bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="fdf72-199">![SAML Login](./media/active-directory-saas-topdesk-public-tutorial/ic790625.png "SAML Login")</span></span>

14. <span data-ttu-id="fdf72-200">Az a **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fdf72-200">On the **SAML configuration assistant** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="fdf72-201">![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML-alapú konfigurációs Segéd")</span><span class="sxs-lookup"><span data-stu-id="fdf72-201">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-public-tutorial/ic790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="fdf72-202">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-202">a.</span></span> <span data-ttu-id="fdf72-203">Töltse fel az Azure-portálon, a letöltött metaadatfájl **összevonási metaadatok**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-203">To upload your downloaded metadata file from Azure portal, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="fdf72-204">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-204">b.</span></span> <span data-ttu-id="fdf72-205">Töltse fel a tanúsítványfájlt, a **tanúsítvány (RSA)**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-205">To upload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="fdf72-206">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-206">c.</span></span> <span data-ttu-id="fdf72-207">Fel kell töltenie az embléma fájlt során kapott azonosítóértékeket TOPdesk terméktámogatással a **embléma ikon**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-207">To upload the logo file you got from the TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="fdf72-208">d.</span><span class="sxs-lookup"><span data-stu-id="fdf72-208">d.</span></span> <span data-ttu-id="fdf72-209">Az a **felhasználói név attribútum** szövegmezőhöz típus `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span><span class="sxs-lookup"><span data-stu-id="fdf72-209">In the **User name attribute** textbox, type `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.</span></span>

    <span data-ttu-id="fdf72-210">e.</span><span class="sxs-lookup"><span data-stu-id="fdf72-210">e.</span></span> <span data-ttu-id="fdf72-211">Az a **megjelenített név** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="fdf72-211">In the **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="fdf72-212">f.</span><span class="sxs-lookup"><span data-stu-id="fdf72-212">f.</span></span> <span data-ttu-id="fdf72-213">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-213">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="fdf72-214">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fdf72-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fdf72-215">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fdf72-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fdf72-216">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fdf72-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fdf72-217">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fdf72-217">Create an Azure AD test user</span></span>

<span data-ttu-id="fdf72-218">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fdf72-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="fdf72-220">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fdf72-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fdf72-221">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-221">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="fdf72-223">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-223">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="fdf72-225">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="fdf72-225">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="fdf72-227">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-227">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-topdesk-public-tutorial/create_aaduser_04.png)

    <span data-ttu-id="fdf72-229">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-229">a.</span></span> <span data-ttu-id="fdf72-230">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-230">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fdf72-231">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-231">b.</span></span> <span data-ttu-id="fdf72-232">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="fdf72-232">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="fdf72-233">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-233">c.</span></span> <span data-ttu-id="fdf72-234">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="fdf72-234">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="fdf72-235">d.</span><span class="sxs-lookup"><span data-stu-id="fdf72-235">d.</span></span> <span data-ttu-id="fdf72-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-236">Click **Create**.</span></span>
 
### <a name="create-a-topdesk---public-test-user"></a><span data-ttu-id="fdf72-237">Hozzon létre egy TOPdesk - nyilvános tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="fdf72-237">Create a TOPdesk - Public test user</span></span>

<span data-ttu-id="fdf72-238">Ahhoz, hogy az Azure AD-felhasználók TOPdesk - be tudjon jelentkezni nyilvános, TOPdesk - nyilvános be kell kiépítve.</span><span class="sxs-lookup"><span data-stu-id="fdf72-238">In order to enable Azure AD users to log into TOPdesk - Public, they must be provisioned into TOPdesk - Public.</span></span>  
<span data-ttu-id="fdf72-239">TOPdesk - esetén nyilvános, kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fdf72-239">In the case of TOPdesk - Public, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="fdf72-240">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-240">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="fdf72-241">Jelentkezzen be a **TOPdesk - nyilvános** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fdf72-241">Sign on to your **TOPdesk - Public** company site as administrator.</span></span>

2. <span data-ttu-id="fdf72-242">Kattintson a felső menüben **TOPdesk \> új \> támogatófájljait \> személy**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-242">In the menu on the top, click **TOPdesk \> New \> Support Files \> Person**.</span></span>
   
    <span data-ttu-id="fdf72-243">![Személy](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "személy")</span><span class="sxs-lookup"><span data-stu-id="fdf72-243">![Person](./media/active-directory-saas-topdesk-public-tutorial/ic790628.png "Person")</span></span>

3. <span data-ttu-id="fdf72-244">Új személy párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fdf72-244">On the New Person dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="fdf72-245">![Új személy](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "új személy")</span><span class="sxs-lookup"><span data-stu-id="fdf72-245">![New Person](./media/active-directory-saas-topdesk-public-tutorial/ic790629.png "New Person")</span></span>
   
    <span data-ttu-id="fdf72-246">a.</span><span class="sxs-lookup"><span data-stu-id="fdf72-246">a.</span></span> <span data-ttu-id="fdf72-247">Kattintson az Általános fülre.</span><span class="sxs-lookup"><span data-stu-id="fdf72-247">Click the General tab.</span></span>

    <span data-ttu-id="fdf72-248">b.</span><span class="sxs-lookup"><span data-stu-id="fdf72-248">b.</span></span> <span data-ttu-id="fdf72-249">Az a **vezetékneve** szövegmező, írja be például a Simon a felhasználó vezetékneve</span><span class="sxs-lookup"><span data-stu-id="fdf72-249">In the **Surname** textbox, type Surname of the user like Simon</span></span>
 
    <span data-ttu-id="fdf72-250">c.</span><span class="sxs-lookup"><span data-stu-id="fdf72-250">c.</span></span> <span data-ttu-id="fdf72-251">Válassza ki a **hely** a fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="fdf72-251">Select a **Site** for the account.</span></span>
 
    <span data-ttu-id="fdf72-252">d.</span><span class="sxs-lookup"><span data-stu-id="fdf72-252">d.</span></span> <span data-ttu-id="fdf72-253">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-253">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="fdf72-254">Bármely más TOPdesk - nyilvános felhasználói fiók létrehozása eszközök is használhatja, vagy TOPdesk - nyilvános kiépítéséhez az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="fdf72-254">You can use any other TOPdesk - Public user account creation tools or APIs provided by TOPdesk - Public to provision Azure AD user accounts.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="fdf72-255">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fdf72-255">Assign the Azure AD test user</span></span>

<span data-ttu-id="fdf72-256">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TOPdesk - nyilvános Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="fdf72-256">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TOPdesk - Public.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="fdf72-258">**Britta Simon hozzárendelése TOPdesk - nyilvános, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fdf72-258">**To assign Britta Simon to TOPdesk - Public, perform the following steps:**</span></span>

1. <span data-ttu-id="fdf72-259">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-259">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fdf72-261">Az alkalmazások listában válassza ki a **TOPdesk - nyilvános**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-261">In the applications list, select **TOPdesk - Public**.</span></span>

    ![A TOPdesk - nyilvános hivatkozás az alkalmazások listáját a](./media/active-directory-saas-topdesk-public-tutorial/tutorial_topdesk-public_app.png)  

3. <span data-ttu-id="fdf72-263">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fdf72-263">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="fdf72-265">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fdf72-265">Click **Add** button.</span></span> <span data-ttu-id="fdf72-266">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fdf72-266">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="fdf72-268">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fdf72-268">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fdf72-269">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fdf72-269">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fdf72-270">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fdf72-270">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fdf72-271">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fdf72-271">Test single sign-on</span></span>

<span data-ttu-id="fdf72-272">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fdf72-272">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fdf72-273">Ha kattint a TOPdesk – nyilvános csempére a hozzáférési panelen, meg kell beolvasni automatikusan aláírt a a TOPdesk - nyilvános alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fdf72-273">When you click the TOPdesk - Public tile in the Access Panel, you should get automatically signed-on to your TOPdesk - Public application.</span></span>
<span data-ttu-id="fdf72-274">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fdf72-274">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="fdf72-275">További források</span><span class="sxs-lookup"><span data-stu-id="fdf72-275">Additional resources</span></span>

* [<span data-ttu-id="fdf72-276">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fdf72-276">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fdf72-277">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fdf72-277">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-topdesk-public-tutorial/tutorial_general_203.png

