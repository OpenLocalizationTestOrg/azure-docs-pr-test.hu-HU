---
title: "Oktatóanyag: Azure Active Directoryval integrált Concur |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Concur között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1eee0a5d-24fa-4986-9aef-3c543cfe3296
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 0b44437b3dcf69dae3587529da7d12e7809b9f55
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-concur"></a><span data-ttu-id="d0d02-103">Oktatóanyag: Azure Active Directoryval integrált Concur</span><span class="sxs-lookup"><span data-stu-id="d0d02-103">Tutorial: Azure Active Directory integration with Concur</span></span>

<span data-ttu-id="d0d02-104">Ebben az oktatóanyagban elsajátíthatja Concur integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d0d02-104">In this tutorial, you learn how to integrate Concur with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d0d02-105">Concur integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d0d02-105">Integrating Concur with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d0d02-106">Megadhatja a Concur hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d0d02-106">You can control in Azure AD who has access to Concur</span></span>
- <span data-ttu-id="d0d02-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Concur (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d0d02-107">You can enable your users to automatically get signed-on to Concur (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d0d02-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d0d02-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d0d02-109">Ha szeretné tudni, hogy az Azure AD SaaS integrálásáról további információt, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d0d02-109">If you want to know more information about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0d02-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d0d02-110">Prerequisites</span></span>

<span data-ttu-id="d0d02-111">Konfigurálása az Azure AD-integrációs Concur, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d0d02-111">To configure Azure AD integration with Concur, you need the following items:</span></span>

- <span data-ttu-id="d0d02-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d0d02-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d0d02-113">Egy Concur egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d0d02-113">A Concur single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d0d02-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d0d02-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d0d02-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d0d02-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d0d02-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d0d02-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d0d02-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d0d02-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d0d02-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d0d02-118">Scenario description</span></span>
<span data-ttu-id="d0d02-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d0d02-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d0d02-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d0d02-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d0d02-121">A gyűjteményből Concur hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0d02-121">Adding Concur from the gallery</span></span>
2. <span data-ttu-id="d0d02-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0d02-122">Configuring and testing Azure AD single sign-on</span></span>

>[!NOTE]
><span data-ttu-id="d0d02-123">Az összevont egyszeri bejelentkezéshez a SAML keresztül Concur előfizetés konfigurálása egy külön feladat, és vegye fel a kapcsolatot nem [ügyfél egyetért támogatási csoport](https://www.concur.co.in/contact) végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d0d02-123">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 

## <a name="adding-concur-from-the-gallery"></a><span data-ttu-id="d0d02-124">A gyűjteményből Concur hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d0d02-124">Adding Concur from the gallery</span></span>
<span data-ttu-id="d0d02-125">Az Azure AD integrálása a Concur konfigurálásához kell hozzáadnia Concur a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d0d02-125">To configure the integration of Concur into Azure AD, you need to add Concur from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d0d02-126">**A gyűjteményből Concur hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0d02-126">**To add Concur from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d0d02-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d0d02-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d0d02-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d0d02-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d0d02-134">Írja be a keresőmezőbe, **Concur**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-134">In the search box, type **Concur**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/tutorial_concur_search.png)

5. <span data-ttu-id="d0d02-136">Az eredmények panelen válassza ki a **Concur**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d0d02-136">In the results panel, select **Concur**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/tutorial_concur_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d0d02-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d0d02-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d0d02-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Concur</span><span class="sxs-lookup"><span data-stu-id="d0d02-139">In this section, you configure and test Azure AD single sign-on with Concur based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="d0d02-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Concur a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d0d02-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Concur is to a user in Azure AD.</span></span> <span data-ttu-id="d0d02-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Concur közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d0d02-141">In other words, a link relationship between an Azure AD user and the related user in Concur needs to be established.</span></span>

<span data-ttu-id="d0d02-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Concur a.</span><span class="sxs-lookup"><span data-stu-id="d0d02-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Concur.</span></span>

<span data-ttu-id="d0d02-143">Az Azure AD egyszeri bejelentkezést a Concur tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d0d02-143">To configure and test Azure AD single sign-on with Concur, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d0d02-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d0d02-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d0d02-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d0d02-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d0d02-146">**[Concur tesztfelhasználó létrehozása](#creating-a-concur-test-user)**  - való Britta Simon valami Concur, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d0d02-146">**[Creating a Concur test user](#creating-a-concur-test-user)** - to have a counterpart of Britta Simon in Concur that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d0d02-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d0d02-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d0d02-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d0d02-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d0d02-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0d02-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d0d02-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Concur alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d0d02-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Concur application.</span></span>

<span data-ttu-id="d0d02-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Concur, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0d02-151">**To configure Azure AD single sign-on with Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="d0d02-152">Az Azure portálon a a **Concur** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-152">In the Azure portal, on the **Concur** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d0d02-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d0d02-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-concur-tutorial/tutorial_concur_samlbase.png)

3. <span data-ttu-id="d0d02-156">Az a **egyetért tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d0d02-156">On the **Concur Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-concur-tutorial/tutorial_concur_url.png)

    <span data-ttu-id="d0d02-158">a.</span><span class="sxs-lookup"><span data-stu-id="d0d02-158">a.</span></span> <span data-ttu-id="d0d02-159">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket a következő minta használatával:`https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span><span class="sxs-lookup"><span data-stu-id="d0d02-159">In the **Sign on URL** textbox, type the value using the following pattern: `https://www.concursolutions.com/UI/SSO/<OrganizationId>`</span></span>

    <span data-ttu-id="d0d02-160">b.</span><span class="sxs-lookup"><span data-stu-id="d0d02-160">b.</span></span> <span data-ttu-id="d0d02-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<customer-domain>.concursolutions.com`</span><span class="sxs-lookup"><span data-stu-id="d0d02-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<customer-domain>.concursolutions.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d0d02-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="d0d02-162">These values are not the real.</span></span> <span data-ttu-id="d0d02-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d0d02-163">Update these values with the actual Sign on URL and Identifier.</span></span> <span data-ttu-id="d0d02-164">Ügyfél [ügyfél egyetért támogatási csoport](https://www.concur.co.in/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d0d02-164">Contact [Concur Client support team](https://www.concur.co.in/contact) to get these values.</span></span> 

4. <span data-ttu-id="d0d02-165">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d0d02-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-concur-tutorial/tutorial_concur_certificate.png) 

5. <span data-ttu-id="d0d02-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-167">Click **Save** button.</span></span>

    <span data-ttu-id="d0d02-168">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span><span class="sxs-lookup"><span data-stu-id="d0d02-168">![Configure Single Sign-On](./media/active-directory-saas-concur-tutorial/tutorial_general_400.png)
<CS></span></span>

6. <span data-ttu-id="d0d02-169">Egyszeri bejelentkezés konfigurálása **Concur** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** Concur támogatásához.</span><span class="sxs-lookup"><span data-stu-id="d0d02-169">To configure single sign-on on **Concur** side, you need to send the downloaded **Metadata XML** to Concur support.</span></span> <span data-ttu-id="d0d02-170">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d0d02-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

  >[!NOTE]
  ><span data-ttu-id="d0d02-171">Az összevont egyszeri bejelentkezéshez a SAML keresztül Concur előfizetés konfigurálása egy külön feladat, és vegye fel a kapcsolatot nem [ügyfél egyetért támogatási csoport](https://www.concur.co.in/contact) végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="d0d02-171">The configuration of your Concur subscription for federated SSO via SAML is a separate task, which you must contact [Concur Client support team](https://www.concur.co.in/contact) to perform.</span></span> 
  
<CE>

> [!TIP]
> <span data-ttu-id="d0d02-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d0d02-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d0d02-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d0d02-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d0d02-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d0d02-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d0d02-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0d02-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="d0d02-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d0d02-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d0d02-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0d02-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d0d02-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d0d02-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d0d02-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d0d02-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d0d02-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d0d02-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-concur-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d0d02-187">a.</span><span class="sxs-lookup"><span data-stu-id="d0d02-187">a.</span></span> <span data-ttu-id="d0d02-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d0d02-189">b.</span><span class="sxs-lookup"><span data-stu-id="d0d02-189">b.</span></span> <span data-ttu-id="d0d02-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d0d02-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d0d02-191">c.</span><span class="sxs-lookup"><span data-stu-id="d0d02-191">c.</span></span> <span data-ttu-id="d0d02-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d0d02-193">d.</span><span class="sxs-lookup"><span data-stu-id="d0d02-193">d.</span></span> <span data-ttu-id="d0d02-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-194">Click **Create**.</span></span>
 
### <a name="creating-a-concur-test-user"></a><span data-ttu-id="d0d02-195">Concur tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d0d02-195">Creating a Concur test user</span></span>

<span data-ttu-id="d0d02-196">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="d0d02-196">Application supports the Just in time user provisioning and after authentication users are created in the application automatically.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d0d02-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d0d02-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d0d02-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Concur Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d0d02-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Concur.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d0d02-200">**Britta Simon hozzárendelése Concur, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d0d02-200">**To assign Britta Simon to Concur, perform the following steps:**</span></span>

1. <span data-ttu-id="d0d02-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d0d02-203">Az alkalmazások listában válassza ki a **Concur**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-203">In the applications list, select **Concur**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-concur-tutorial/tutorial_concur_app.png) 

3. <span data-ttu-id="d0d02-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d0d02-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d0d02-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d0d02-207">Click **Add** button.</span></span> <span data-ttu-id="d0d02-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0d02-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d0d02-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d0d02-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d0d02-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0d02-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d0d02-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d0d02-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d0d02-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d0d02-213">Testing single sign-on</span></span>

<span data-ttu-id="d0d02-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d0d02-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="d0d02-215">Ha a hozzáférési panelen Concur csempére kattint, Concur alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d0d02-215">When you click the Concur tile in the Access Panel, you should get login page of Concur application.</span></span>
<span data-ttu-id="d0d02-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d0d02-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d0d02-217">További források</span><span class="sxs-lookup"><span data-stu-id="d0d02-217">Additional resources</span></span>

* [<span data-ttu-id="d0d02-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d0d02-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d0d02-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d0d02-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="d0d02-220">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d0d02-220">Configure User Provisioning</span></span>](active-directory-saas-concur-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-concur-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-concur-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-concur-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-concur-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-concur-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-concur-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-concur-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-concur-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-concur-tutorial/tutorial_general_203.png

