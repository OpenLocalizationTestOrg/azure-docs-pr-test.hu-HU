---
title: "Oktatóanyag: Azure Active Directoryval integrált eszköz banki |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és az eszköz Bank között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3006ad6e-8831-41cd-94aa-7e7ae770ce7b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 17bc0082e3721b50269cb4b17884c0e4a4cbcb5d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asset-bank"></a><span data-ttu-id="3bb08-103">Oktatóanyag: Azure Active Directoryval integrált eszköz banki</span><span class="sxs-lookup"><span data-stu-id="3bb08-103">Tutorial: Azure Active Directory integration with Asset Bank</span></span>

<span data-ttu-id="3bb08-104">Ebben az oktatóanyagban elsajátíthatja eszköz banki integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3bb08-104">In this tutorial, you learn how to integrate Asset Bank with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3bb08-105">Eszköz banki integrálása az Azure AD a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3bb08-105">Integrating Asset Bank with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3bb08-106">Megadhatja a eszköz banki hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3bb08-106">You can control in Azure AD who has access to Asset Bank</span></span>
- <span data-ttu-id="3bb08-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett eszköz Banknak (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="3bb08-107">You can enable your users to automatically get signed-on to Asset Bank (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3bb08-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3bb08-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3bb08-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3bb08-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3bb08-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3bb08-110">Prerequisites</span></span>

<span data-ttu-id="3bb08-111">Az Azure AD-integráció konfigurálása eszköz banknál, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3bb08-111">To configure Azure AD integration with Asset Bank, you need the following items:</span></span>

- <span data-ttu-id="3bb08-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3bb08-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3bb08-113">Egy eszköz banki egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="3bb08-113">An Asset Bank single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3bb08-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3bb08-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3bb08-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3bb08-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3bb08-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3bb08-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3bb08-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3bb08-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3bb08-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3bb08-118">Scenario description</span></span>
<span data-ttu-id="3bb08-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3bb08-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3bb08-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3bb08-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3bb08-121">Ha felveszi eszköz banki a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3bb08-121">Adding Asset Bank from the gallery</span></span>
2. <span data-ttu-id="3bb08-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3bb08-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asset-bank-from-the-gallery"></a><span data-ttu-id="3bb08-123">Ha felveszi eszköz banki a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3bb08-123">Adding Asset Bank from the gallery</span></span>
<span data-ttu-id="3bb08-124">Az Azure AD integrálása a Bank eszköz konfigurálásához kell hozzáadnia eszköz banki a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3bb08-124">To configure the integration of Asset Bank into Azure AD, you need to add Asset Bank from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3bb08-125">**A gyűjteményből eszköz banki hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bb08-125">**To add Asset Bank from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3bb08-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3bb08-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3bb08-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3bb08-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3bb08-133">Írja be a keresőmezőbe, **eszköz banki**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-133">In the search box, type **Asset Bank**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_search.png)

5. <span data-ttu-id="3bb08-135">Az eredmények panelen válassza ki a **eszköz banki**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3bb08-135">In the results panel, select **Asset Bank**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3bb08-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3bb08-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3bb08-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján eszköz banknál</span><span class="sxs-lookup"><span data-stu-id="3bb08-138">In this section, you configure and test Azure AD single sign-on with Asset Bank based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3bb08-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó, eszköz bankban a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3bb08-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Asset Bank is to a user in Azure AD.</span></span> <span data-ttu-id="3bb08-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Bank eszköz közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3bb08-140">In other words, a link relationship between an Azure AD user and the related user in Asset Bank needs to be established.</span></span>

<span data-ttu-id="3bb08-141">Eszköz bankban, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3bb08-141">In Asset Bank, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3bb08-142">Az Azure AD az egyszeri bejelentkezés eszköz banknál tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3bb08-142">To configure and test Azure AD single sign-on with Asset Bank, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3bb08-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3bb08-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3bb08-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3bb08-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3bb08-145">**[Egy eszköz banki tesztfelhasználó létrehozása](#creating-an-asset-bank-test-user)**  - való egy megfelelője a Britta Simon eszköz banki, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3bb08-145">**[Creating an Asset Bank test user](#creating-an-asset-bank-test-user)** - to have a counterpart of Britta Simon in Asset Bank that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3bb08-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3bb08-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3bb08-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3bb08-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3bb08-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3bb08-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3bb08-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az eszköz banki alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3bb08-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Asset Bank application.</span></span>

<span data-ttu-id="3bb08-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása eszköz banki, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bb08-150">**To configure Azure AD single sign-on with Asset Bank, perform the following steps:**</span></span>

1. <span data-ttu-id="3bb08-151">Az Azure portálon a a **eszköz banki** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-151">In the Azure portal, on the **Asset Bank** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3bb08-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3bb08-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_samlbase.png)

3. <span data-ttu-id="3bb08-155">Az a **eszköz banki tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3bb08-155">On the **Asset Bank Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_url.png)

    <span data-ttu-id="3bb08-157">a.</span><span class="sxs-lookup"><span data-stu-id="3bb08-157">a.</span></span> <span data-ttu-id="3bb08-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.assetbank-server.com`</span><span class="sxs-lookup"><span data-stu-id="3bb08-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.assetbank-server.com`</span></span>

    <span data-ttu-id="3bb08-159">b.</span><span class="sxs-lookup"><span data-stu-id="3bb08-159">b.</span></span> <span data-ttu-id="3bb08-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.assetbank-server.com/shibboleth`</span><span class="sxs-lookup"><span data-stu-id="3bb08-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.assetbank-server.com/shibboleth`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3bb08-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="3bb08-161">These values are not real.</span></span> <span data-ttu-id="3bb08-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3bb08-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3bb08-163">Ügyfél [eszköz banki ügyfél-támogatási csoport](mailto:support@assetbank.co.uk) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3bb08-163">Contact [Asset Bank Client support team](mailto:support@assetbank.co.uk) to get these values.</span></span> 
 
4. <span data-ttu-id="3bb08-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3bb08-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_certificate.png) 

5. <span data-ttu-id="3bb08-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-assetbank-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3bb08-168">Egyszeri bejelentkezés konfigurálása **eszköz banki** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [eszköz banki támogatási csoport](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="3bb08-168">To configure single sign-on on **Asset Bank** side, you need to send the downloaded **Metadata XML** to [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span> 


> [!TIP]
> <span data-ttu-id="3bb08-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3bb08-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3bb08-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3bb08-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3bb08-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3bb08-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3bb08-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bb08-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="3bb08-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3bb08-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3bb08-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bb08-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3bb08-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3bb08-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3bb08-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3bb08-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3bb08-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3bb08-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-assetbank-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3bb08-184">a.</span><span class="sxs-lookup"><span data-stu-id="3bb08-184">a.</span></span> <span data-ttu-id="3bb08-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3bb08-186">b.</span><span class="sxs-lookup"><span data-stu-id="3bb08-186">b.</span></span> <span data-ttu-id="3bb08-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3bb08-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3bb08-188">c.</span><span class="sxs-lookup"><span data-stu-id="3bb08-188">c.</span></span> <span data-ttu-id="3bb08-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3bb08-190">d.</span><span class="sxs-lookup"><span data-stu-id="3bb08-190">d.</span></span> <span data-ttu-id="3bb08-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-191">Click **Create**.</span></span>
 
### <a name="creating-an-asset-bank-test-user"></a><span data-ttu-id="3bb08-192">Egy eszköz banki tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3bb08-192">Creating an Asset Bank test user</span></span>

<span data-ttu-id="3bb08-193">Ez a szakasz célja Britta Simon eszköz Bank nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3bb08-193">The objective of this section is to create a user called Britta Simon in Asset Bank.</span></span> <span data-ttu-id="3bb08-194">Eszköz Bank lehetővé teszi, hogy közvetlenül az időponthoz kötött kiépítés, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="3bb08-194">Asset Bank supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="3bb08-195">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="3bb08-195">There is no action item for you in this section.</span></span> <span data-ttu-id="3bb08-196">Új felhasználó jön létre az eszköz banki elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="3bb08-196">A new user is created during an attempt to access Asset Bank if it doesn't exist yet.</span></span> 

>[!NOTE]
><span data-ttu-id="3bb08-197">Hozza létre a felhasználó manuálisan kell, ha szeretné-e lépjen kapcsolatba a [eszköz banki támogatási csoport](mailto:support@assetbank.co.uk).</span><span class="sxs-lookup"><span data-stu-id="3bb08-197">If you need to create a user manually, you need to contact the [Asset Bank support team](mailto:support@assetbank.co.uk).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3bb08-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3bb08-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3bb08-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés eszköz banki Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3bb08-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Asset Bank.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3bb08-201">**Az eszköz banki Britta Simon rendeléséhez hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3bb08-201">**To assign Britta Simon to Asset Bank, perform the following steps:**</span></span>

1. <span data-ttu-id="3bb08-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3bb08-204">Az alkalmazások listában válassza ki a **eszköz banki**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-204">In the applications list, select **Asset Bank**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-assetbank-tutorial/tutorial_assetbank_app.png) 

3. <span data-ttu-id="3bb08-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3bb08-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3bb08-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3bb08-208">Click **Add** button.</span></span> <span data-ttu-id="3bb08-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bb08-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3bb08-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3bb08-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3bb08-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bb08-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3bb08-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3bb08-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3bb08-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3bb08-214">Testing single sign-on</span></span>

<span data-ttu-id="3bb08-215">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="3bb08-215">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3bb08-216">Ha a hozzáférési Panel eszköz banki mozaik gombra kattint, akkor kell beolvasása automatikusan bejelentkezett az eszköz banki alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3bb08-216">When you click the Asset Bank tile in the Access Panel, you should get automatically signed-on to your Asset Bank application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3bb08-217">További források</span><span class="sxs-lookup"><span data-stu-id="3bb08-217">Additional resources</span></span>

* [<span data-ttu-id="3bb08-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3bb08-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3bb08-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3bb08-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-assetbank-tutorial/tutorial_general_203.png

