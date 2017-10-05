---
title: "Oktatóanyag: Azure Active Directoryval integrált Datahug |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Datahug között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5c0dc1ea-7ff4-4554-b60b-0f2fa9f5abaa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: jeedes
ms.openlocfilehash: ec431dd5ccfa53e4b975e46da247704dd1e15c2c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-datahug"></a><span data-ttu-id="1d67d-103">Oktatóanyag: Azure Active Directoryval integrált Datahug</span><span class="sxs-lookup"><span data-stu-id="1d67d-103">Tutorial: Azure Active Directory integration with Datahug</span></span>

<span data-ttu-id="1d67d-104">Ebben az oktatóanyagban elsajátíthatja Datahug integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="1d67d-104">In this tutorial, you learn how to integrate Datahug with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="1d67d-105">Datahug integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="1d67d-105">Integrating Datahug with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="1d67d-106">Megadhatja a Datahug hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="1d67d-106">You can control in Azure AD who has access to Datahug</span></span>
- <span data-ttu-id="1d67d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Datahug (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="1d67d-107">You can enable your users to automatically get signed-on to Datahug (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="1d67d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="1d67d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="1d67d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="1d67d-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="1d67d-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="1d67d-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1d67d-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="1d67d-111">Prerequisites</span></span>

<span data-ttu-id="1d67d-112">Konfigurálása az Azure AD-integrációs Datahug, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="1d67d-112">To configure Azure AD integration with Datahug, you need the following items:</span></span>

- <span data-ttu-id="1d67d-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="1d67d-113">An Azure AD subscription</span></span>
- <span data-ttu-id="1d67d-114">Egy Datahug egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="1d67d-114">A Datahug single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="1d67d-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="1d67d-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="1d67d-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="1d67d-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="1d67d-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="1d67d-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="1d67d-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="1d67d-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="1d67d-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="1d67d-119">Scenario description</span></span>
<span data-ttu-id="1d67d-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="1d67d-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="1d67d-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="1d67d-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="1d67d-122">A gyűjteményből Datahug hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d67d-122">Adding Datahug from the gallery</span></span>
2. <span data-ttu-id="1d67d-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1d67d-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-datahug-from-the-gallery"></a><span data-ttu-id="1d67d-124">A gyűjteményből Datahug hozzáadása</span><span class="sxs-lookup"><span data-stu-id="1d67d-124">Adding Datahug from the gallery</span></span>
<span data-ttu-id="1d67d-125">Az Azure AD integrálása a Datahug konfigurálásához kell hozzáadnia Datahug a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="1d67d-125">To configure the integration of Datahug into Azure AD, you need to add Datahug from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="1d67d-126">**A gyűjteményből Datahug hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d67d-126">**To add Datahug from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="1d67d-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="1d67d-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="1d67d-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="1d67d-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="1d67d-134">Írja be a keresőmezőbe, **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-134">In the search box, type **Datahug**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_search.png)

5. <span data-ttu-id="1d67d-136">Az eredmények panelen válassza ki a **Datahug**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1d67d-136">In the results panel, select **Datahug**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="1d67d-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="1d67d-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="1d67d-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Datahug</span><span class="sxs-lookup"><span data-stu-id="1d67d-139">In this section, you configure and test Azure AD single sign-on with Datahug based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="1d67d-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Datahug a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="1d67d-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Datahug is to a user in Azure AD.</span></span> <span data-ttu-id="1d67d-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Datahug közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="1d67d-141">In other words, a link relationship between an Azure AD user and the related user in Datahug needs to be established.</span></span>

<span data-ttu-id="1d67d-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Datahug a.</span><span class="sxs-lookup"><span data-stu-id="1d67d-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Datahug.</span></span>

<span data-ttu-id="1d67d-143">Az Azure AD egyszeri bejelentkezést a Datahug tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="1d67d-143">To configure and test Azure AD single sign-on with Datahug, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="1d67d-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="1d67d-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="1d67d-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="1d67d-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="1d67d-146">**[Datahug tesztfelhasználó létrehozása](#creating-a-datahug-test-user)**  - való Britta Simon valami Datahug, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="1d67d-146">**[Creating a Datahug test user](#creating-a-datahug-test-user)** - to have a counterpart of Britta Simon in Datahug that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="1d67d-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1d67d-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="1d67d-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="1d67d-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="1d67d-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="1d67d-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="1d67d-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Datahug alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="1d67d-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Datahug application.</span></span>

<span data-ttu-id="1d67d-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Datahug, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d67d-151">**To configure Azure AD single sign-on with Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="1d67d-152">Az Azure portálon a a **Datahug** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-152">In the Azure portal, on the **Datahug** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="1d67d-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="1d67d-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_samlbase.png)

3. <span data-ttu-id="1d67d-156">Az a **Datahug tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="1d67d-156">On the **Datahug Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_ur1.png)

    <span data-ttu-id="1d67d-158">a.</span><span class="sxs-lookup"><span data-stu-id="1d67d-158">a.</span></span> <span data-ttu-id="1d67d-159">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://apps.datahug.com/identity/<uniqueID>`</span><span class="sxs-lookup"><span data-stu-id="1d67d-159">In the **Identifier** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>`</span></span>

    <span data-ttu-id="1d67d-160">b.</span><span class="sxs-lookup"><span data-stu-id="1d67d-160">b.</span></span> <span data-ttu-id="1d67d-161">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://apps.datahug.com/identity/<uniqueID>/acs`</span><span class="sxs-lookup"><span data-stu-id="1d67d-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://apps.datahug.com/identity/<uniqueID>/acs`</span></span>

4. <span data-ttu-id="1d67d-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="1d67d-163">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="1d67d-163">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_url2.png)

    <span data-ttu-id="1d67d-165">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://apps.datahug.com/`</span><span class="sxs-lookup"><span data-stu-id="1d67d-165">In the **Sign-on URL** textbox, type a URL as: `https://apps.datahug.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="1d67d-166">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="1d67d-166">These values are not the real.</span></span> <span data-ttu-id="1d67d-167">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="1d67d-167">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="1d67d-168">Itt javasoljuk, hogy az egyedi értéket a azonosítója és a válasz URL-CÍMEN karakterlánc.</span><span class="sxs-lookup"><span data-stu-id="1d67d-168">Here we suggest you to use the unique value of string in the Identifier and Reply URL.</span></span> <span data-ttu-id="1d67d-169">Ügyfél [Datahug ügyfél-támogatási csoport](http://datahug.com/about/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="1d67d-169">Contact [Datahug Client support team](http://datahug.com/about/contact-us/) to get these values.</span></span> 

5. <span data-ttu-id="1d67d-170">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="1d67d-170">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_certificate.png) 

6.  <span data-ttu-id="1d67d-172">Ellenőrizze **"Megjelenítése speciális tanúsítvány-aláírási beállításai"** és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="1d67d-172">Check **“Show advanced certificate signing settings”** and perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_cert.png)

    <span data-ttu-id="1d67d-174">a.</span><span class="sxs-lookup"><span data-stu-id="1d67d-174">a.</span></span> <span data-ttu-id="1d67d-175">A **aláíró beállítás**, jelölje be **bejelentkezési SAML-előfeltétel**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-175">In **Signing Option**, select **Sign SAML assertion**.</span></span>
    
    <span data-ttu-id="1d67d-176">b.</span><span class="sxs-lookup"><span data-stu-id="1d67d-176">b.</span></span> <span data-ttu-id="1d67d-177">A **aláíró algoritmus**, jelölje be **SHA1**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-177">In **Signing algorithm**, select **SHA1**.</span></span>
 
7. <span data-ttu-id="1d67d-178">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-178">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="1d67d-180">A a **Datahug konfigurációs** kattintson **konfigurálása Datahug** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="1d67d-180">On the **Datahug Configuration** section, click **Configure Datahug** to open **Configure sign-on** window.</span></span> <span data-ttu-id="1d67d-181">Másolás a **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="1d67d-181">Copy the **SAML Entity ID** and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_configure.png) 

9. <span data-ttu-id="1d67d-183">Egyszeri bejelentkezés konfigurálása **Datahug** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja**, **SAML Entitásazonosító** és **SAML-alapú egyszeri bejelentkezési URL-címe** való [Datahug támogatási](http://datahug.com/about/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="1d67d-183">To configure single sign-on on **Datahug** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID** and **SAML Single Sign-On Service URL** to [Datahug support](http://datahug.com/about/contact-us/).</span></span> <span data-ttu-id="1d67d-184">Ez állítva ez az alkalmazás a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="1d67d-184">They set this application up to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="1d67d-185">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="1d67d-185">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="1d67d-186">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="1d67d-186">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="1d67d-187">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="1d67d-187">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="1d67d-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d67d-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="1d67d-189">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="1d67d-189">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="1d67d-191">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d67d-191">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="1d67d-192">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-192">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="1d67d-194">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-194">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="1d67d-196">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="1d67d-196">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="1d67d-198">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="1d67d-198">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-datahug-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="1d67d-200">a.</span><span class="sxs-lookup"><span data-stu-id="1d67d-200">a.</span></span> <span data-ttu-id="1d67d-201">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-201">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="1d67d-202">b.</span><span class="sxs-lookup"><span data-stu-id="1d67d-202">b.</span></span> <span data-ttu-id="1d67d-203">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="1d67d-203">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="1d67d-204">c.</span><span class="sxs-lookup"><span data-stu-id="1d67d-204">c.</span></span> <span data-ttu-id="1d67d-205">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-205">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="1d67d-206">d.</span><span class="sxs-lookup"><span data-stu-id="1d67d-206">d.</span></span> <span data-ttu-id="1d67d-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-207">Click **Create**.</span></span>
 
### <a name="creating-a-datahug-test-user"></a><span data-ttu-id="1d67d-208">Datahug tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="1d67d-208">Creating a Datahug test user</span></span>

<span data-ttu-id="1d67d-209">Ahhoz, hogy az Azure AD-felhasználók Datahug bejelentkezni, akkor ki kell építenie a Datahug.</span><span class="sxs-lookup"><span data-stu-id="1d67d-209">To enable Azure AD users to log in to Datahug, they must be provisioned into Datahug.</span></span>  
<span data-ttu-id="1d67d-210">Datahug, ha a kézi tevékenység kiépítés.</span><span class="sxs-lookup"><span data-stu-id="1d67d-210">When Datahug, provisioning is a manual task.</span></span>

<span data-ttu-id="1d67d-211">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d67d-211">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="1d67d-212">Jelentkezzen be rendszergazdaként a Datahug vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="1d67d-212">Log in to your Datahug company site as an administrator.</span></span>

2. <span data-ttu-id="1d67d-213">Vigye a **fogaskerékre** a jobb felső sarkában, majd kattintson a **beállítások**</span><span class="sxs-lookup"><span data-stu-id="1d67d-213">Hover over the **cog** in the top right-hand corner and click **Settings**</span></span>
   
   ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/1.png)

3. <span data-ttu-id="1d67d-215">Válasszon **személyek** , és kattintson a **felhasználó hozzáadása** lap</span><span class="sxs-lookup"><span data-stu-id="1d67d-215">Choose **People** and click the **Add Users** tab</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/2.png)

4. <span data-ttu-id="1d67d-217">Írja be annak a személynek szeretné hozzon létre egy fiókot, és kattintson az e-mailt **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-217">Type the email of the person you would like to create an account for and click **Add**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-datahug-tutorial/3.png)

    > [!NOTE] 
    > <span data-ttu-id="1d67d-219">Elküldheti regisztrációs mail felhasználói kiválasztásával **üdvözlő e-mailek küldése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="1d67d-219">You can send registration mail to user by selecting **Send welcome email** checkbox.</span></span>  
    > <span data-ttu-id="1d67d-220">Létrehozásakor egy Salesforce fiókot ne küldjön az üdvözlő e-mail.</span><span class="sxs-lookup"><span data-stu-id="1d67d-220">If you are creating an account for Salesforce do not send the welcome email.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="1d67d-221">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1d67d-221">Assigning the Azure AD test user</span></span>

<span data-ttu-id="1d67d-222">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Datahug Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="1d67d-222">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Datahug.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="1d67d-224">**Britta Simon hozzárendelése Datahug, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="1d67d-224">**To assign Britta Simon to Datahug, perform the following steps:**</span></span>

1. <span data-ttu-id="1d67d-225">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-225">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="1d67d-227">Az alkalmazások listában válassza ki a **Datahug**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-227">In the applications list, select **Datahug**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-datahug-tutorial/tutorial_datahug_app.png) 

3. <span data-ttu-id="1d67d-229">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="1d67d-229">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="1d67d-231">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="1d67d-231">Click **Add** button.</span></span> <span data-ttu-id="1d67d-232">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d67d-232">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="1d67d-234">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="1d67d-234">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="1d67d-235">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d67d-235">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="1d67d-236">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="1d67d-236">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="1d67d-237">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="1d67d-237">Testing single sign-on</span></span>

<span data-ttu-id="1d67d-238">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="1d67d-238">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
<span data-ttu-id="1d67d-239">Ha a hozzáférési panelen Datahug csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Datahug alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="1d67d-239">When you click the Datahug tile in the Access Panel, you should get automatically signed-on to your Datahug application.</span></span> <span data-ttu-id="1d67d-240">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="1d67d-240">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="1d67d-241">További források</span><span class="sxs-lookup"><span data-stu-id="1d67d-241">Additional resources</span></span>

* [<span data-ttu-id="1d67d-242">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="1d67d-242">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="1d67d-243">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="1d67d-243">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-datahug-tutorial/tutorial_general_203.png

