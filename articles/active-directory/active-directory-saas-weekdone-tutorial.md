---
title: "Oktatóanyag: Azure Active Directoryval integrált Weekdone |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Weekdone között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 34921f9a-5637-4420-ab4c-9beb34421909
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 84aa0069dce55a6623398a99e1cac6bb21bf52f7
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-weekdone"></a><span data-ttu-id="9604d-103">Oktatóanyag: Azure Active Directoryval integrált Weekdone</span><span class="sxs-lookup"><span data-stu-id="9604d-103">Tutorial: Azure Active Directory integration with Weekdone</span></span>

<span data-ttu-id="9604d-104">Ebben az oktatóanyagban elsajátíthatja Weekdone integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9604d-104">In this tutorial, you learn how to integrate Weekdone with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9604d-105">Weekdone integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9604d-105">Integrating Weekdone with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9604d-106">Megadhatja a Weekdone hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9604d-106">You can control in Azure AD who has access to Weekdone</span></span>
- <span data-ttu-id="9604d-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Weekdone (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9604d-107">You can enable your users to automatically get signed-on to Weekdone (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9604d-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9604d-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9604d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9604d-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9604d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9604d-110">Prerequisites</span></span>

<span data-ttu-id="9604d-111">Konfigurálása az Azure AD-integrációs Weekdone, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9604d-111">To configure Azure AD integration with Weekdone, you need the following items:</span></span>

- <span data-ttu-id="9604d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9604d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9604d-113">Egy Weekdone egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9604d-113">A Weekdone single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9604d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9604d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9604d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9604d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9604d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9604d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9604d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9604d-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9604d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9604d-118">Scenario description</span></span>
<span data-ttu-id="9604d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9604d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9604d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9604d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9604d-121">A gyűjteményből Weekdone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9604d-121">Adding Weekdone from the gallery</span></span>
2. <span data-ttu-id="9604d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9604d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-weekdone-from-the-gallery"></a><span data-ttu-id="9604d-123">A gyűjteményből Weekdone hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9604d-123">Adding Weekdone from the gallery</span></span>
<span data-ttu-id="9604d-124">Az Azure AD integrálása a Weekdone konfigurálásához kell hozzáadnia Weekdone a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9604d-124">To configure the integration of Weekdone into Azure AD, you need to add Weekdone from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9604d-125">**A gyűjteményből Weekdone hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9604d-125">**To add Weekdone from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9604d-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9604d-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9604d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9604d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9604d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9604d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9604d-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9604d-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9604d-133">Írja be a keresőmezőbe, **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="9604d-133">In the search box, type **Weekdone**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_search.png)

5. <span data-ttu-id="9604d-135">Az eredmények panelen válassza ki a **Weekdone**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9604d-135">In the results panel, select **Weekdone**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9604d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9604d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9604d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Weekdone.</span><span class="sxs-lookup"><span data-stu-id="9604d-138">In this section, you configure and test Azure AD single sign-on with Weekdone based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9604d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Weekdone a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9604d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Weekdone is to a user in Azure AD.</span></span> <span data-ttu-id="9604d-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Weekdone közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9604d-140">In other words, a link relationship between an Azure AD user and the related user in Weekdone needs to be established.</span></span>

<span data-ttu-id="9604d-141">Weekdone, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9604d-141">In Weekdone, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9604d-142">Az Azure AD egyszeri bejelentkezést a Weekdone tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9604d-142">To configure and test Azure AD single sign-on with Weekdone, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9604d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9604d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9604d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9604d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9604d-145">**[Weekdone tesztfelhasználó létrehozása](#creating-a-weekdone-test-user)**  - való Britta Simon valami Weekdone, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9604d-145">**[Creating a Weekdone test user](#creating-a-weekdone-test-user)** - to have a counterpart of Britta Simon in Weekdone that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9604d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9604d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9604d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9604d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9604d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9604d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9604d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Weekdone alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9604d-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Weekdone application.</span></span>

<span data-ttu-id="9604d-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Weekdone, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9604d-150">**To configure Azure AD single sign-on with Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="9604d-151">Az Azure portálon a a **Weekdone** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9604d-151">In the Azure portal, on the **Weekdone** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9604d-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9604d-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_samlbase.png)

3. <span data-ttu-id="9604d-155">Az a **Weekdone tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="9604d-155">On the **Weekdone Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url1.png)

    <span data-ttu-id="9604d-157">a.</span><span class="sxs-lookup"><span data-stu-id="9604d-157">a.</span></span> <span data-ttu-id="9604d-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="9604d-158">In the **Identifier** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

    <span data-ttu-id="9604d-159">b.</span><span class="sxs-lookup"><span data-stu-id="9604d-159">b.</span></span> <span data-ttu-id="9604d-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="9604d-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>

4. <span data-ttu-id="9604d-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="9604d-161">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="9604d-162">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="9604d-162">If you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_url2.png)

    <span data-ttu-id="9604d-164">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://weekdone.com/a/<tenantname>`</span><span class="sxs-lookup"><span data-stu-id="9604d-164">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://weekdone.com/a/<tenantname>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="9604d-165">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9604d-165">These values are not real.</span></span> <span data-ttu-id="9604d-166">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="9604d-166">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="9604d-167">Ügyfél [Weekdone ügyfél-támogatási csoport](mailto:hello@weekdone.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9604d-167">Contact [Weekdone Client support team](mailto:hello@weekdone.com) to get these values.</span></span> 

5. <span data-ttu-id="9604d-168">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9604d-168">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_certificate.png) 

6. <span data-ttu-id="9604d-170">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9604d-170">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="9604d-172">A a **Weekdone konfigurációs** kattintson **konfigurálása Weekdone** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9604d-172">On the **Weekdone Configuration** section, click **Configure Weekdone** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9604d-173">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9604d-173">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_configure.png) 

8. <span data-ttu-id="9604d-175">Egyszeri bejelentkezés konfigurálása **Weekdone** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja, Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** való [Weekdone támogatási csoport](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="9604d-175">To configure single sign-on on **Weekdone** side, you need to send the downloaded **Metadata XML, Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Weekdone support team](mailto:hello@weekdone.com).</span></span>

> [!TIP]
> <span data-ttu-id="9604d-176">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9604d-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9604d-177">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9604d-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9604d-178">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9604d-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9604d-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9604d-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="9604d-180">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9604d-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9604d-182">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9604d-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9604d-183">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9604d-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9604d-185">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9604d-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9604d-187">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9604d-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9604d-189">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9604d-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-weekdone-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9604d-191">a.</span><span class="sxs-lookup"><span data-stu-id="9604d-191">a.</span></span> <span data-ttu-id="9604d-192">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9604d-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9604d-193">b.</span><span class="sxs-lookup"><span data-stu-id="9604d-193">b.</span></span> <span data-ttu-id="9604d-194">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9604d-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9604d-195">c.</span><span class="sxs-lookup"><span data-stu-id="9604d-195">c.</span></span> <span data-ttu-id="9604d-196">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9604d-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9604d-197">d.</span><span class="sxs-lookup"><span data-stu-id="9604d-197">d.</span></span> <span data-ttu-id="9604d-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9604d-198">Click **Create**.</span></span>
 
### <a name="creating-a-weekdone-test-user"></a><span data-ttu-id="9604d-199">Weekdone tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9604d-199">Creating a Weekdone test user</span></span>

<span data-ttu-id="9604d-200">Ez a szakasz célja Weekdone Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9604d-200">The objective of this section is to create a user called Britta Simon in Weekdone.</span></span> <span data-ttu-id="9604d-201">Weekdone támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="9604d-201">Weekdone supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="9604d-202">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="9604d-202">There is no action item for you in this section.</span></span> <span data-ttu-id="9604d-203">Új felhasználó jön létre az Weekdone elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="9604d-203">A new user is created during an attempt to access Weekdone if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="9604d-204">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [Weekdone ügyfél-támogatási csoport](mailto:hello@weekdone.com).</span><span class="sxs-lookup"><span data-stu-id="9604d-204">If you need to create a user manually, you need to contact the [Weekdone Client support team](mailto:hello@weekdone.com).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9604d-205">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9604d-205">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9604d-206">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Weekdone Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9604d-206">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Weekdone.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9604d-208">**Britta Simon hozzárendelése Weekdone, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9604d-208">**To assign Britta Simon to Weekdone, perform the following steps:**</span></span>

1. <span data-ttu-id="9604d-209">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9604d-209">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9604d-211">Az alkalmazások listában válassza ki a **Weekdone**.</span><span class="sxs-lookup"><span data-stu-id="9604d-211">In the applications list, select **Weekdone**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-weekdone-tutorial/tutorial_weekdone_app.png) 

3. <span data-ttu-id="9604d-213">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9604d-213">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9604d-215">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9604d-215">Click **Add** button.</span></span> <span data-ttu-id="9604d-216">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9604d-216">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9604d-218">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9604d-218">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9604d-219">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9604d-219">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9604d-220">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9604d-220">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9604d-221">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9604d-221">Testing single sign-on</span></span>

<span data-ttu-id="9604d-222">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9604d-222">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9604d-223">Ha a hozzáférési panelen Weekdone csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Weekdone alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9604d-223">When you click the Weekdone tile in the Access Panel, you should get automatically signed-on to your Weekdone application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9604d-224">További források</span><span class="sxs-lookup"><span data-stu-id="9604d-224">Additional resources</span></span>

* [<span data-ttu-id="9604d-225">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9604d-225">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9604d-226">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9604d-226">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-weekdone-tutorial/tutorial_general_203.png

