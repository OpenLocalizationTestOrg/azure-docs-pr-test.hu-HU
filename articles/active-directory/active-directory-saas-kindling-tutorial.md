---
title: "Oktatóanyag: Azure Active Directoryval integrált Kindling |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Kindling között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 71229751-74eb-4c2c-abb4-07caa95754c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2017
ms.author: jeedes
ms.openlocfilehash: 131c2c3f46c60193d512b1779e917c8322732fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-kindling"></a><span data-ttu-id="675e3-103">Oktatóanyag: Azure Active Directoryval integrált Kindling</span><span class="sxs-lookup"><span data-stu-id="675e3-103">Tutorial: Azure Active Directory integration with Kindling</span></span>

<span data-ttu-id="675e3-104">Ebben az oktatóanyagban elsajátíthatja Kindling integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="675e3-104">In this tutorial, you learn how to integrate Kindling with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="675e3-105">Kindling integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="675e3-105">Integrating Kindling with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="675e3-106">Megadhatja a Kindling hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="675e3-106">You can control in Azure AD who has access to Kindling</span></span>
- <span data-ttu-id="675e3-107">Engedélyezheti a automatikusan lekérni a bejelentkezett felhasználók a saját Azure AD-fiókok (egyszeri bejelentkezés) Kindling</span><span class="sxs-lookup"><span data-stu-id="675e3-107">You can enable your users to automatically get signed-on to Kindling (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="675e3-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="675e3-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="675e3-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="675e3-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="675e3-110">[alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="675e3-110">[what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="675e3-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="675e3-111">Prerequisites</span></span>

<span data-ttu-id="675e3-112">Konfigurálása az Azure AD-integrációs Kindling, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="675e3-112">To configure Azure AD integration with Kindling, you need the following items:</span></span>

- <span data-ttu-id="675e3-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="675e3-113">An Azure AD subscription</span></span>
- <span data-ttu-id="675e3-114">Egy Kindling egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="675e3-114">A Kindling single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="675e3-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="675e3-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="675e3-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="675e3-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="675e3-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="675e3-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="675e3-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="675e3-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="675e3-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="675e3-119">Scenario description</span></span>
<span data-ttu-id="675e3-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="675e3-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="675e3-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="675e3-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="675e3-122">A gyűjteményből Kindling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="675e3-122">Adding Kindling from the gallery</span></span>
2. <span data-ttu-id="675e3-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="675e3-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-kindling-from-the-gallery"></a><span data-ttu-id="675e3-124">A gyűjteményből Kindling hozzáadása</span><span class="sxs-lookup"><span data-stu-id="675e3-124">Adding Kindling from the gallery</span></span>
<span data-ttu-id="675e3-125">Az Azure AD-be Kindling integrálása konfigurálásához kell hozzáadnia Kindling a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="675e3-125">To configure the integration of Kindling into Azure AD, you need to add Kindling from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="675e3-126">**A gyűjteményből Kindling hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="675e3-126">**To add Kindling from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="675e3-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="675e3-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="675e3-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="675e3-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="675e3-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="675e3-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="675e3-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="675e3-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="675e3-134">Írja be a keresőmezőbe, **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="675e3-134">In the search box, type **Kindling**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_search.png)

5. <span data-ttu-id="675e3-136">Az eredmények panelen válassza ki a **Kindling**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="675e3-136">In the results panel, select **Kindling**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="675e3-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="675e3-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="675e3-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Kindling</span><span class="sxs-lookup"><span data-stu-id="675e3-139">In this section, you configure and test Azure AD single sign-on with Kindling based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="675e3-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Kindling a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="675e3-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Kindling is to a user in Azure AD.</span></span> <span data-ttu-id="675e3-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Kindling közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="675e3-141">In other words, a link relationship between an Azure AD user and the related user in Kindling needs to be established.</span></span>

<span data-ttu-id="675e3-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Kindling a.</span><span class="sxs-lookup"><span data-stu-id="675e3-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Kindling.</span></span>

<span data-ttu-id="675e3-143">Az Azure AD egyszeri bejelentkezést a Kindling tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="675e3-143">To configure and test Azure AD single sign-on with Kindling, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="675e3-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="675e3-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="675e3-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="675e3-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="675e3-146">**[Kindling tesztfelhasználó létrehozása](#creating-a-kindling-test-user)**  - való Britta Simon valami Kindling, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="675e3-146">**[Creating a Kindling test user](#creating-a-kindling-test-user)** - to have a counterpart of Britta Simon in Kindling that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="675e3-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="675e3-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="675e3-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="675e3-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="675e3-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="675e3-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="675e3-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Kindling alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="675e3-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Kindling application.</span></span>

<span data-ttu-id="675e3-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Kindling, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="675e3-151">**To configure Azure AD single sign-on with Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="675e3-152">Az Azure portálon a a **Kindling** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="675e3-152">In the Azure portal, on the **Kindling** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="675e3-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="675e3-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_samlbase.png)

3. <span data-ttu-id="675e3-156">Az a **Kindling tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="675e3-156">On the **Kindling Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_url.png)

    <span data-ttu-id="675e3-158">a.</span><span class="sxs-lookup"><span data-stu-id="675e3-158">a.</span></span> <span data-ttu-id="675e3-159">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.kindlingapp.com`</span><span class="sxs-lookup"><span data-stu-id="675e3-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com`</span></span>

    <span data-ttu-id="675e3-160">b.</span><span class="sxs-lookup"><span data-stu-id="675e3-160">b.</span></span>  <span data-ttu-id="675e3-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span><span class="sxs-lookup"><span data-stu-id="675e3-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.kindlingapp.com/saml/module.php/saml/sp/metadata.php/clientIDP`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="675e3-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="675e3-162">These values are not the real.</span></span> <span data-ttu-id="675e3-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="675e3-163">Update these values with the actual Sign-on URL and Identifier.</span></span> <span data-ttu-id="675e3-164">Ügyfél [Kindling támogatási csoport](mailto:support@kindlingapp.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="675e3-164">Contact [Kindling support team](mailto:support@kindlingapp.com) to get these values.</span></span>
 
4. <span data-ttu-id="675e3-165">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="675e3-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_certificate.png) 

5. <span data-ttu-id="675e3-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="675e3-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="675e3-169">A a **Kindling konfigurációs** kattintson **konfigurálása Kindling** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="675e3-169">On the **Kindling Configuration** section, click **Configure Kindling** to open **Configure sign-on** window.</span></span> <span data-ttu-id="675e3-170">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="675e3-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_configure.png) 

7. <span data-ttu-id="675e3-172">Egyszeri bejelentkezés konfigurálása **Kindling** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [Kindling támogatási csoport](mailto:support@kindlingapp.com).</span><span class="sxs-lookup"><span data-stu-id="675e3-172">To configure single sign-on on **Kindling** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Kindling support team](mailto:support@kindlingapp.com).</span></span>

> [!TIP]
> <span data-ttu-id="675e3-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="675e3-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="675e3-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="675e3-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="675e3-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="675e3-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="675e3-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="675e3-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="675e3-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="675e3-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="675e3-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="675e3-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="675e3-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="675e3-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="675e3-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="675e3-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="675e3-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="675e3-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="675e3-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="675e3-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-kindling-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="675e3-188">a.</span><span class="sxs-lookup"><span data-stu-id="675e3-188">a.</span></span> <span data-ttu-id="675e3-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="675e3-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="675e3-190">b.</span><span class="sxs-lookup"><span data-stu-id="675e3-190">b.</span></span> <span data-ttu-id="675e3-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="675e3-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="675e3-192">c.</span><span class="sxs-lookup"><span data-stu-id="675e3-192">c.</span></span> <span data-ttu-id="675e3-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="675e3-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="675e3-194">d.</span><span class="sxs-lookup"><span data-stu-id="675e3-194">d.</span></span> <span data-ttu-id="675e3-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="675e3-195">Click **Create**.</span></span>
 
### <a name="creating-a-kindling-test-user"></a><span data-ttu-id="675e3-196">Kindling tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="675e3-196">Creating a Kindling test user</span></span>

<span data-ttu-id="675e3-197">Ez a szakasz célja Kindling Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="675e3-197">The objective of this section is to create a user called Britta Simon in Kindling.</span></span> <span data-ttu-id="675e3-198">Kindling támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="675e3-198">Kindling supports just-in-time provisioning.</span></span> <span data-ttu-id="675e3-199">Már engedélyezett legyen [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="675e3-199">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="675e3-200">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="675e3-200">There is no action item for you in this section.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="675e3-201">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="675e3-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="675e3-202">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Kindling Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="675e3-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Kindling.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="675e3-204">**Britta Simon hozzárendelése Kindling, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="675e3-204">**To assign Britta Simon to Kindling, perform the following steps:**</span></span>

1. <span data-ttu-id="675e3-205">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="675e3-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="675e3-207">Az alkalmazások listában válassza ki a **Kindling**.</span><span class="sxs-lookup"><span data-stu-id="675e3-207">In the applications list, select **Kindling**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-kindling-tutorial/tutorial_kindling_app.png) 

3. <span data-ttu-id="675e3-209">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="675e3-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="675e3-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="675e3-211">Click **Add** button.</span></span> <span data-ttu-id="675e3-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="675e3-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="675e3-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="675e3-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="675e3-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="675e3-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="675e3-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="675e3-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="675e3-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="675e3-217">Testing single sign-on</span></span>

<span data-ttu-id="675e3-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="675e3-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="675e3-219">Ha a hozzáférési panelen Kindling csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Kindling alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="675e3-219">When you click the Kindling tile in the Access Panel, you should get automatically signed-on to your Kindling application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="675e3-220">További források</span><span class="sxs-lookup"><span data-stu-id="675e3-220">Additional resources</span></span>

* [<span data-ttu-id="675e3-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="675e3-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="675e3-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="675e3-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-kindling-tutorial/tutorial_general_203.png

