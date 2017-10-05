---
title: "Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Splunk vállalati és Splunk felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/02/2017
ms.author: jeedes
ms.openlocfilehash: 181d0f33245f0811c15c1e7945c797502ef71eba
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="f8615-103">Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő</span><span class="sxs-lookup"><span data-stu-id="f8615-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="f8615-104">Ebben az oktatóanyagban elsajátíthatja Splunk vállalati és Splunk felhő integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f8615-104">In this tutorial, you learn how to integrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f8615-105">Splunk vállalati és Splunk felhő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f8615-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f8615-106">Szabályozhatja, aki hozzáfér Splunk vállalati és Splunk felhőalapú Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f8615-106">You can control in Azure AD who has access to Splunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="f8615-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Splunk vállalati és Splunk felhő (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="f8615-107">You can enable your users to automatically get signed-on to Splunk Enterprise and Splunk Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f8615-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f8615-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f8615-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f8615-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8615-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f8615-110">Prerequisites</span></span>

<span data-ttu-id="f8615-111">Konfigurálja az Azure AD-integrációs Splunk vállalati és Splunk felhő, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="f8615-111">To configure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need the following items:</span></span>

- <span data-ttu-id="f8615-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f8615-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f8615-113">Egy Splunk vállalati és Splunk felhő egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f8615-113">A Splunk Enterprise and Splunk Cloud single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f8615-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f8615-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f8615-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f8615-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f8615-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f8615-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f8615-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f8615-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f8615-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f8615-118">Scenario description</span></span>
<span data-ttu-id="f8615-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f8615-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f8615-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f8615-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f8615-121">Splunk vállalati és Splunk felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f8615-121">Adding Splunk Enterprise and Splunk Cloud from the gallery</span></span>
2. <span data-ttu-id="f8615-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f8615-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a><span data-ttu-id="f8615-123">Splunk vállalati és Splunk felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f8615-123">Adding Splunk Enterprise and Splunk Cloud from the gallery</span></span>
<span data-ttu-id="f8615-124">Az Azure AD integrálása a Splunk vállalati és Splunk felhő konfigurálásához kell hozzáadnia Splunk vállalati és a felhő Splunk a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f8615-124">To configure the integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need to add Splunk Enterprise and Splunk Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f8615-125">**A gyűjteményből Splunk vállalati és Splunk felhő hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8615-125">**To add Splunk Enterprise and Splunk Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f8615-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f8615-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f8615-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f8615-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f8615-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f8615-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f8615-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f8615-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f8615-133">Írja be a keresőmezőbe, **Splunk vállalati és Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="f8615-133">In the search box, type **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_search.png)

5. <span data-ttu-id="f8615-135">Az eredmények panelen válassza ki a **Splunk vállalati és Splunk felhő**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f8615-135">In the results panel, select **Splunk Enterprise and Splunk Cloud**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f8615-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f8615-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f8615-138">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a vállalati Splunk tesztelése és konfigurálása és Splunk felhő "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="f8615-138">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="f8615-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Splunk vállalati és Splunk felhő Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f8615-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Splunk Enterprise and Splunk Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="f8615-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Splunk Enterprise és Splunk felhő közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f8615-140">In other words, a link relationship between an Azure AD user and the related user in Splunk Enterprise and Splunk Cloud needs to be established.</span></span>

<span data-ttu-id="f8615-141">A Splunk Enterprise és Splunk felhő, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f8615-141">In Splunk Enterprise and Splunk Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f8615-142">Az Azure AD az egyszeri bejelentkezés Splunk vállalati és Splunk felhő tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f8615-142">To configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f8615-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f8615-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f8615-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f8615-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f8615-145">**[Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  – egy partner Britta Simon Splunk vállalati és felhasználói az Azure AD ábrázolását kapcsolódó Splunk felhő rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f8615-145">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - to have a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f8615-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f8615-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f8615-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f8615-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f8615-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f8615-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f8615-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Splunk vállalati és Splunk felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="f8615-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Splunk Enterprise and Splunk Cloud application.</span></span>

<span data-ttu-id="f8615-150">**Konfigurálja az Azure AD az egyszeri bejelentkezés Splunk vállalati és Splunk felhő, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8615-150">**To configure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="f8615-151">Az Azure portálon a a **Splunk vállalati és Splunk felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f8615-151">In the Azure portal, on the **Splunk Enterprise and Splunk Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f8615-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f8615-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_samlbase.png)

3. <span data-ttu-id="f8615-155">Az a **Splunk vállalati és Splunk felhőalapú tartományt és URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="f8615-155">On the **Splunk Enterprise and Splunk Cloud Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_url.png)

    <span data-ttu-id="f8615-157">a.</span><span class="sxs-lookup"><span data-stu-id="f8615-157">a.</span></span> <span data-ttu-id="f8615-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="f8615-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
    
    <span data-ttu-id="f8615-159">b.</span><span class="sxs-lookup"><span data-stu-id="f8615-159">b.</span></span> <span data-ttu-id="f8615-160">Az a **azonosító** szövegmezőhöz a Splunk kiszolgáló URL-címét.</span><span class="sxs-lookup"><span data-stu-id="f8615-160">In the **Identifier** textbox, type the URL of your Splunk Server.</span></span>

    <span data-ttu-id="f8615-161">c.</span><span class="sxs-lookup"><span data-stu-id="f8615-161">c.</span></span> <span data-ttu-id="f8615-162">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="f8615-162">In the **Reply URL** textbox, type a URL using the following pattern: `https://<splunkserver>/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f8615-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f8615-163">These values are not real.</span></span> <span data-ttu-id="f8615-164">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="f8615-164">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="f8615-165">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="f8615-165">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="f8615-166">Ügyfél [Splunk vállalati és Splunk felhő ügyfél támogatási csoport](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f8615-166">Contact [Splunk Enterprise and Splunk Cloud Client support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) to get these values.</span></span> 

4. <span data-ttu-id="f8615-167">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f8615-167">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_certificate.png) 

5. <span data-ttu-id="f8615-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f8615-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f8615-171">Egyszeri bejelentkezés konfigurálása **Splunk vállalati és Splunk felhő** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Splunk vállalati és Splunk felhő támogatási csoport](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).</span><span class="sxs-lookup"><span data-stu-id="f8615-171">To configure single sign-on on **Splunk Enterprise and Splunk Cloud** side, you need to send the downloaded **Metadata XML** to [Splunk Enterprise and Splunk Cloud support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support).</span></span>

> [!TIP]
> <span data-ttu-id="f8615-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f8615-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f8615-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f8615-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f8615-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f8615-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f8615-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8615-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="f8615-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f8615-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f8615-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8615-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f8615-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f8615-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f8615-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f8615-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f8615-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f8615-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f8615-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f8615-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f8615-187">a.</span><span class="sxs-lookup"><span data-stu-id="f8615-187">a.</span></span> <span data-ttu-id="f8615-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f8615-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f8615-189">b.</span><span class="sxs-lookup"><span data-stu-id="f8615-189">b.</span></span> <span data-ttu-id="f8615-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f8615-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f8615-191">c.</span><span class="sxs-lookup"><span data-stu-id="f8615-191">c.</span></span> <span data-ttu-id="f8615-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f8615-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f8615-193">d.</span><span class="sxs-lookup"><span data-stu-id="f8615-193">d.</span></span> <span data-ttu-id="f8615-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f8615-194">Click **Create**.</span></span>
 
### <a name="creating-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="f8615-195">Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f8615-195">Creating a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="f8615-196">Ebben a szakaszban Britta Simon Splunk vállalati és Splunk felhő nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8615-196">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="f8615-197">Együttműködve [Splunk vállalati és Splunk felhő támogatási csoport](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) a felhasználók hozzáadása a Splunk vállalati és Splunk felhő platform.</span><span class="sxs-lookup"><span data-stu-id="f8615-197">Work with  [Splunk Enterprise and Splunk Cloud support team](https://www.splunk.com/content/splunkcom/en_us/about-us/contact.html#tabs/customer-support) to add the users in the Splunk Enterprise and Splunk Cloud platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f8615-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f8615-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f8615-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Splunk vállalati és Splunk felhőalapú Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f8615-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Splunk Enterprise and Splunk Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f8615-201">**Britta Simon Splunk vállalati és Splunk felhő hozzárendelése, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f8615-201">**To assign Britta Simon to Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="f8615-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f8615-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f8615-204">Az alkalmazások listában válassza ki a **Splunk vállalati és Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="f8615-204">In the applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_splunkenterpriseandsplunkcloud_app.png) 

3. <span data-ttu-id="f8615-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f8615-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f8615-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f8615-208">Click **Add** button.</span></span> <span data-ttu-id="f8615-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8615-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f8615-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f8615-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f8615-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8615-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f8615-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f8615-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f8615-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f8615-214">Testing single sign-on</span></span>

<span data-ttu-id="f8615-215">Ebben a szakaszban az Azure AD SSOonfiguration a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f8615-215">In this section, you test your Azure AD SSOonfiguration using the Access Panel.</span></span>

<span data-ttu-id="f8615-216">A Splunk vállalati kattint, és a hozzáférési Panel Splunk felhő csempére, amikor meg kell beolvasása automatikusan bejelentkezett az Splunk vállalati és Splunk felhő alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f8615-216">When you click the Splunk Enterprise and Splunk Cloud tile in the Access Panel, you should get automatically signed-on to your Splunk Enterprise and Splunk Cloud application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8615-217">További források</span><span class="sxs-lookup"><span data-stu-id="f8615-217">Additional resources</span></span>

* [<span data-ttu-id="f8615-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f8615-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f8615-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f8615-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-splunkenterpriseandsplunkcloud-tutorial/tutorial_general_203.png

