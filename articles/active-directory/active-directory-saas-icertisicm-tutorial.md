---
title: "Oktatóanyag: Azure Active Directoryval integrált Icertis szerződés felügyeleti Platform |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Icertis szerződés felügyeleti Platform között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 6627e6dd-f559-4cd4-a509-f6d9a4961b49
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 9dd002f71b7a960338071db869f7c8cf88071342
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-icertis-contract-management-platform"></a><span data-ttu-id="e53d6-103">Oktatóanyag: Azure Active Directoryval integrált Icertis szerződés felügyeleti Platform</span><span class="sxs-lookup"><span data-stu-id="e53d6-103">Tutorial: Azure Active Directory integration with Icertis Contract Management Platform</span></span>

<span data-ttu-id="e53d6-104">Ebben az oktatóanyagban elsajátíthatja Icertis szerződés felügyeleti Platform integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e53d6-104">In this tutorial, you learn how to integrate Icertis Contract Management Platform with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e53d6-105">Icertis szerződés felügyeleti Platform integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e53d6-105">Integrating Icertis Contract Management Platform with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e53d6-106">Szabályozhatja az Azure AD, aki hozzáfér Icertis szerződés felügyeleti Platform</span><span class="sxs-lookup"><span data-stu-id="e53d6-106">You can control in Azure AD who has access to Icertis Contract Management Platform</span></span>
- <span data-ttu-id="e53d6-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Icertis szerződés felügyeleti platform (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="e53d6-107">You can enable your users to automatically get signed-on to Icertis Contract Management Platform (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e53d6-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e53d6-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e53d6-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e53d6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e53d6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e53d6-110">Prerequisites</span></span>

<span data-ttu-id="e53d6-111">Konfigurálása az Azure AD-integrációs Icertis szerződés felügyeleti Platform, az alábbi elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="e53d6-111">To configure Azure AD integration with Icertis Contract Management Platform, you need the following items:</span></span>

- <span data-ttu-id="e53d6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e53d6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e53d6-113">Egy Icertis szerződés felügyeleti Platform egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e53d6-113">An Icertis Contract Management Platform single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e53d6-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e53d6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e53d6-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e53d6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e53d6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e53d6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e53d6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e53d6-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e53d6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e53d6-118">Scenario description</span></span>
<span data-ttu-id="e53d6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e53d6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e53d6-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e53d6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e53d6-121">A gyűjteményből Icertis szerződés felügyeleti Platform hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e53d6-121">Adding Icertis Contract Management Platform from the gallery</span></span>
2. <span data-ttu-id="e53d6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e53d6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-icertis-contract-management-platform-from-the-gallery"></a><span data-ttu-id="e53d6-123">A gyűjteményből Icertis szerződés felügyeleti Platform hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e53d6-123">Adding Icertis Contract Management Platform from the gallery</span></span>
<span data-ttu-id="e53d6-124">Az Azure AD integrálása a Icertis szerződés felügyeleti Platform konfigurálásához kell hozzáadnia Icertis szerződés kezelési platformot a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e53d6-124">To configure the integration of Icertis Contract Management Platform into Azure AD, you need to add Icertis Contract Management Platform from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e53d6-125">**A gyűjteményből Icertis szerződés felügyeleti Platform hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e53d6-125">**To add Icertis Contract Management Platform from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e53d6-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e53d6-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e53d6-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e53d6-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e53d6-133">Írja be a keresőmezőbe, **Icertis szerződés felügyeleti Platform**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-133">In the search box, type **Icertis Contract Management Platform**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_search.png)

5. <span data-ttu-id="e53d6-135">Az eredmények panelen válassza ki a **Icertis szerződés felügyeleti Platform**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e53d6-135">In the results panel, select **Icertis Contract Management Platform**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e53d6-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e53d6-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e53d6-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló Icertis szerződés felügyeleti Platform.</span><span class="sxs-lookup"><span data-stu-id="e53d6-138">In this section, you configure and test Azure AD single sign-on with Icertis Contract Management Platform based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e53d6-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Icertis szerződés felügyeleti platform Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="e53d6-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Icertis Contract Management Platform is to a user in Azure AD.</span></span> <span data-ttu-id="e53d6-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Icertis szerződés felügyeleti platform közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e53d6-140">In other words, a link relationship between an Azure AD user and the related user in Icertis Contract Management Platform needs to be established.</span></span>

<span data-ttu-id="e53d6-141">Icertis szerződés felügyeleti platform, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e53d6-141">In Icertis Contract Management Platform, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e53d6-142">Az Azure AD az egyszeri bejelentkezés Icertis szerződés felügyeleti platform tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e53d6-142">To configure and test Azure AD single sign-on with Icertis Contract Management Platform, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e53d6-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e53d6-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e53d6-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e53d6-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e53d6-145">**[Egy Icertis szerződés felügyeleti Platform tesztfelhasználó létrehozása](#creating-an-icertis-contract-management-platform-test-user)**  - való egy megfelelője a Britta Simon Icertis szerződés felügyeleti Platform, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e53d6-145">**[Creating an Icertis Contract Management Platform test user](#creating-an-icertis-contract-management-platform-test-user)** - to have a counterpart of Britta Simon in Icertis Contract Management Platform that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e53d6-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e53d6-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e53d6-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e53d6-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e53d6-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e53d6-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e53d6-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Icertis szerződés felügyeleti Platform alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e53d6-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Icertis Contract Management Platform application.</span></span>

<span data-ttu-id="e53d6-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Icertis szerződés felügyeleti Platform, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e53d6-150">**To configure Azure AD single sign-on with Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="e53d6-151">Az Azure portálon a a **Icertis szerződés felügyeleti Platform** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-151">In the Azure portal, on the **Icertis Contract Management Platform** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e53d6-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e53d6-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_samlbase.png)

3. <span data-ttu-id="e53d6-155">Az a **Icertis szerződés felügyeleti Platform tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e53d6-155">On the **Icertis Contract Management Platform Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_url.png)

    <span data-ttu-id="e53d6-157">a.</span><span class="sxs-lookup"><span data-stu-id="e53d6-157">a.</span></span> <span data-ttu-id="e53d6-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="e53d6-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    <span data-ttu-id="e53d6-159">b.</span><span class="sxs-lookup"><span data-stu-id="e53d6-159">b.</span></span> <span data-ttu-id="e53d6-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.icertis.com`</span><span class="sxs-lookup"><span data-stu-id="e53d6-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.icertis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e53d6-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="e53d6-161">These values are not real.</span></span> <span data-ttu-id="e53d6-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="e53d6-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="e53d6-163">Ügyfél [Icertis szerződés felügyeleti Platform ügyfél-támogatási csoport](https://www.icertis.com/company/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="e53d6-163">Contact [Icertis Contract Management Platform Client support team](https://www.icertis.com/company/contact/) to get these values.</span></span> 

4. <span data-ttu-id="e53d6-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e53d6-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_certificate.png) 

5. <span data-ttu-id="e53d6-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e53d6-168">A a **Icertis szerződés felügyeleti Platform konfigurációs** kattintson **konfigurálása Icertis szerződés felügyeleti Platform** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e53d6-168">On the **Icertis Contract Management Platform Configuration** section, click **Configure Icertis Contract Management Platform** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e53d6-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e53d6-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_configure.png) 

7. <span data-ttu-id="e53d6-171">Egyszeri bejelentkezés konfigurálása **Icertis szerződés felügyeleti Platform** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Icertis szerződés felügyeleti Platform támogatási csoport](https://www.icertis.com/company/contact/).</span><span class="sxs-lookup"><span data-stu-id="e53d6-171">To configure single sign-on on **Icertis Contract Management Platform** side, you need to send the downloaded **Metadata XML** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="e53d6-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e53d6-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e53d6-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e53d6-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e53d6-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e53d6-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e53d6-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e53d6-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="e53d6-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e53d6-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e53d6-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e53d6-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e53d6-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e53d6-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e53d6-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e53d6-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e53d6-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e53d6-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-icertisicm-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e53d6-187">a.</span><span class="sxs-lookup"><span data-stu-id="e53d6-187">a.</span></span> <span data-ttu-id="e53d6-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e53d6-189">b.</span><span class="sxs-lookup"><span data-stu-id="e53d6-189">b.</span></span> <span data-ttu-id="e53d6-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e53d6-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e53d6-191">c.</span><span class="sxs-lookup"><span data-stu-id="e53d6-191">c.</span></span> <span data-ttu-id="e53d6-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e53d6-193">d.</span><span class="sxs-lookup"><span data-stu-id="e53d6-193">d.</span></span> <span data-ttu-id="e53d6-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-194">Click **Create**.</span></span>
 
### <a name="creating-an-icertis-contract-management-platform-test-user"></a><span data-ttu-id="e53d6-195">Egy Icertis szerződés felügyeleti Platform tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e53d6-195">Creating an Icertis Contract Management Platform test user</span></span>

<span data-ttu-id="e53d6-196">Ebben a szakaszban egy felhasználó Britta Simon meghívta Icertis szerződés felügyeleti Platform hoz létre.</span><span class="sxs-lookup"><span data-stu-id="e53d6-196">In this section, you create a user called Britta Simon in Icertis Contract Management Platform.</span></span> <span data-ttu-id="e53d6-197">Adjon együttműködve [Icertis szerződés felügyeleti Platform támogatási csoport](https://www.icertis.com/company/contact/) a felhasználók hozzáadása a Icertis szerződés felügyeleti platform.</span><span class="sxs-lookup"><span data-stu-id="e53d6-197">Please work with [Icertis Contract Management Platform support team](https://www.icertis.com/company/contact/) to add the users in the Icertis Contract Management Platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e53d6-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e53d6-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e53d6-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Icertis szerződés felügyeleti Platform Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="e53d6-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Icertis Contract Management Platform.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e53d6-201">**Britta Simon hozzárendelése Icertis szerződés felügyeleti Platform, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e53d6-201">**To assign Britta Simon to Icertis Contract Management Platform, perform the following steps:**</span></span>

1. <span data-ttu-id="e53d6-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e53d6-204">Az alkalmazások listában válassza ki a **Icertis szerződés felügyeleti Platform**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-204">In the applications list, select **Icertis Contract Management Platform**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-icertisicm-tutorial/tutorial_icertisicm_app.png) 

3. <span data-ttu-id="e53d6-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e53d6-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e53d6-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e53d6-208">Click **Add** button.</span></span> <span data-ttu-id="e53d6-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e53d6-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e53d6-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e53d6-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e53d6-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e53d6-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e53d6-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e53d6-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e53d6-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e53d6-214">Testing single sign-on</span></span>

<span data-ttu-id="e53d6-215">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="e53d6-215">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="e53d6-216">Ha a hozzáférési panelen Icertis szerződés felügyeleti Platform csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Icertis szerződés felügyeleti Platform alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="e53d6-216">When you click the Icertis Contract Management Platform tile in the Access Panel, you should get automatically signed-on to your Icertis Contract Management Platform application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e53d6-217">További források</span><span class="sxs-lookup"><span data-stu-id="e53d6-217">Additional resources</span></span>

* [<span data-ttu-id="e53d6-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e53d6-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e53d6-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e53d6-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-icertisicm-tutorial/tutorial_general_203.png

