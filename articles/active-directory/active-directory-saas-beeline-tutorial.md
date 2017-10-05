---
title: "Oktatóanyag: Azure Active Directoryval integrált BeeLine |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BeeLine között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 0726859d-1dac-44a0-810b-da56d89039ee
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 93acbd90bbe5f0a40bf3f56edb766a0fdd30f68f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-beeline"></a><span data-ttu-id="c8dc7-103">Oktatóanyag: Azure Active Directoryval integrált BeeLine</span><span class="sxs-lookup"><span data-stu-id="c8dc7-103">Tutorial: Azure Active Directory integration with BeeLine</span></span>

<span data-ttu-id="c8dc7-104">Ebben az oktatóanyagban elsajátíthatja BeeLine integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8dc7-104">In this tutorial, you learn how to integrate BeeLine with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8dc7-105">BeeLine integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-105">Integrating BeeLine with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c8dc7-106">Megadhatja a BeeLine hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c8dc7-106">You can control in Azure AD who has access to BeeLine</span></span>
- <span data-ttu-id="c8dc7-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BeeLine (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c8dc7-107">You can enable your users to automatically get signed-on to BeeLine (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c8dc7-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c8dc7-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="c8dc7-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8dc7-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8dc7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8dc7-110">Prerequisites</span></span>

<span data-ttu-id="c8dc7-111">Konfigurálása az Azure AD-integrációs BeeLine, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-111">To configure Azure AD integration with BeeLine, you need the following items:</span></span>

- <span data-ttu-id="c8dc7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c8dc7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8dc7-113">Egy BeeLine egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c8dc7-113">A BeeLine single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8dc7-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8dc7-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8dc7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8dc7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8dc7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8dc7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-118">Scenario description</span></span>
<span data-ttu-id="c8dc7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8dc7-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8dc7-121">A gyűjteményből BeeLine hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-121">Adding BeeLine from the gallery</span></span>
2. <span data-ttu-id="c8dc7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8dc7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-beeline-from-the-gallery"></a><span data-ttu-id="c8dc7-123">A gyűjteményből BeeLine hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-123">Adding BeeLine from the gallery</span></span>
<span data-ttu-id="c8dc7-124">Az Azure AD integrálása a BeeLine konfigurálásához kell hozzáadnia BeeLine a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-124">To configure the integration of BeeLine into Azure AD, you need to add BeeLine from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c8dc7-125">**A gyűjteményből BeeLine hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8dc7-125">**To add BeeLine from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c8dc7-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c8dc7-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c8dc7-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c8dc7-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c8dc7-133">Írja be a keresőmezőbe, **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-133">In the search box, type **BeeLine**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_search.png)

5. <span data-ttu-id="c8dc7-135">Az eredmények panelen válassza ki a **BeeLine**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-135">In the results panel, select **BeeLine**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c8dc7-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8dc7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c8dc7-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BeeLine</span><span class="sxs-lookup"><span data-stu-id="c8dc7-138">In this section, you configure and test Azure AD single sign-on with BeeLine based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="c8dc7-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BeeLine a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BeeLine is to a user in Azure AD.</span></span> <span data-ttu-id="c8dc7-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BeeLine közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-140">In other words, a link relationship between an Azure AD user and the related user in BeeLine needs to be established.</span></span>

<span data-ttu-id="c8dc7-141">BeeLine, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-141">In BeeLine, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c8dc7-142">Az Azure AD egyszeri bejelentkezést a BeeLine tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-142">To configure and test Azure AD single sign-on with BeeLine, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c8dc7-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c8dc7-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8dc7-145">**[BeeLine tesztfelhasználó létrehozása](#creating-a-beeline-test-user)**  - való Britta Simon valami BeeLine, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-145">**[Creating a BeeLine test user](#creating-a-beeline-test-user)** - to have a counterpart of Britta Simon in BeeLine that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8dc7-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8dc7-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c8dc7-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c8dc7-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BeeLine alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BeeLine application.</span></span>

<span data-ttu-id="c8dc7-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BeeLine, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8dc7-150">**To configure Azure AD single sign-on with BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="c8dc7-151">Az Azure portálon a a **BeeLine** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-151">In the Azure portal, on the **BeeLine** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c8dc7-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_samlbase.png)

3. <span data-ttu-id="c8dc7-155">Az a **BeeLine tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-155">On the **BeeLine Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_url.png)

    <span data-ttu-id="c8dc7-157">a.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-157">a.</span></span> <span data-ttu-id="c8dc7-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://projects.beeline.net/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="c8dc7-158">In the **Identifier** textbox, type a URL using the following pattern: `https://projects.beeline.net/<instancename>`</span></span>

    <span data-ttu-id="c8dc7-159">b.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-159">b.</span></span> <span data-ttu-id="c8dc7-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-160">In the **Reply URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|
    | `https://projects.beeline.net/<instancename>/SSO_External.ashx`|
    | `https://projects.beeline.net/<companyname>/SSO_External.ashx` |

    > [!NOTE] 
    > <span data-ttu-id="c8dc7-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-161">These values are not real.</span></span> <span data-ttu-id="c8dc7-162">Frissítheti ezeket az értékeket a tényleges azonosítója és a válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-162">Update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="c8dc7-163">Ügyfél [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-163">Contact [BeeLine support team](https://www.beeline.com/contact-us/) to get these values.</span></span>
 
4. <span data-ttu-id="c8dc7-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_certificate.png) 

5. <span data-ttu-id="c8dc7-166">A Beeline alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-166">Your Beeline application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="c8dc7-167">Adjon együttműködve [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) először a megfelelő felhasználói azonosító, amely az alkalmazás rendelendő azonosításához.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-167">Please work with [BeeLine support team](https://www.beeline.com/contact-us/) first to identify the correct user identifier which will be mapped into the application.</span></span> <span data-ttu-id="c8dc7-168">Is hajtsa végre az útmutatások a [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) kapcsolatos az attribútumot, amely szeretnének használni, ez a leképezés.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-168">Also please take the guidance from [BeeLine support team](https://www.beeline.com/contact-us/) about the attribute which they want to use for this mapping.</span></span> <span data-ttu-id="c8dc7-169">Ennek az attribútumnak az értéke kezelheti a **felhasználói attribútumok** az alkalmazás lapján.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-169">You can manage the value of this attribute from the **User Attributes** tab of the application.</span></span> <span data-ttu-id="c8dc7-170">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-170">The following screenshot shows an example for this.</span></span> <span data-ttu-id="c8dc7-171">Itt azt leképezett a **felhasználói azonosító** jogcím a **userprincipalname** attribútum, amely egyedi felhasználói azonosító, amelyet a program minden sikeres SAML-válasz a Beeline alkalmazását biztosít.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-171">Here we have mapped the **User Identifier** claim with the **userprincipalname** attribute, which provides unique user ID, which will be sent to the Beeline application in the every successful SAML Response.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_attribute.png)  

6. <span data-ttu-id="c8dc7-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="c8dc7-175">A a **BeeLine konfigurációs** kattintson **konfigurálása BeeLine** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-175">On the **BeeLine Configuration** section, click **Configure BeeLine** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c8dc7-176">Másolás a **Sign-Out URL-cím** és **SAML Entitásazonosító** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c8dc7-176">Copy the **Sign-Out URL** and **SAML Entity ID** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_configure.png) 

8. <span data-ttu-id="c8dc7-178">Egyszeri bejelentkezés konfigurálása **BeeLine** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** és **SAML Entitásazonosító**, **Sign-Out URL-cím** való [BeeLine támogatási csoport](https://www.beeline.com/contact-us/).</span><span class="sxs-lookup"><span data-stu-id="c8dc7-178">To configure single sign-on on **BeeLine** side, you need to send the downloaded **Metadata XML** and **SAML Entity ID**, **Sign-Out URL** to [BeeLine support team](https://www.beeline.com/contact-us/).</span></span>

> [!TIP]
> <span data-ttu-id="c8dc7-179">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c8dc7-179">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c8dc7-180">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-180">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c8dc7-181">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8dc7-181">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c8dc7-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="c8dc7-183">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-183">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c8dc7-185">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8dc7-185">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c8dc7-186">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-186">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c8dc7-188">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-188">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c8dc7-190">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-190">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c8dc7-192">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c8dc7-192">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-beeline-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c8dc7-194">a.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-194">a.</span></span> <span data-ttu-id="c8dc7-195">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-195">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8dc7-196">b.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-196">b.</span></span> <span data-ttu-id="c8dc7-197">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-197">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c8dc7-198">c.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-198">c.</span></span> <span data-ttu-id="c8dc7-199">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-199">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c8dc7-200">d.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-200">d.</span></span> <span data-ttu-id="c8dc7-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-201">Click **Create**.</span></span>
 
### <a name="creating-a-beeline-test-user"></a><span data-ttu-id="c8dc7-202">BeeLine tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-202">Creating a BeeLine test user</span></span>

<span data-ttu-id="c8dc7-203">Ebben a szakaszban egy Beeline Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-203">In this section, you create a user called Britta Simon in Beeline.</span></span> <span data-ttu-id="c8dc7-204">Beeline alkalmazást úgy kell létrehozni, az alkalmazásban, az egyszeri bejelentkezés végrehajtása előtt minden felhasználó kell.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-204">Beeline application needs all the users to be provisioned in the application before doing Single Sign On.</span></span> <span data-ttu-id="c8dc7-205">Így a munkahelyi a [BeeLine támogatási csoport](https://www.beeline.com/contact-us/) ezek a felhasználók az alkalmazás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-205">So work with the [BeeLine support team](https://www.beeline.com/contact-us/) to provision all these users into the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c8dc7-206">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c8dc7-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c8dc7-207">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BeeLine Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BeeLine.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c8dc7-209">**Britta Simon hozzárendelése BeeLine, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8dc7-209">**To assign Britta Simon to BeeLine, perform the following steps:**</span></span>

1. <span data-ttu-id="c8dc7-210">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c8dc7-212">Az alkalmazások listában válassza ki a **BeeLine**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-212">In the applications list, select **BeeLine**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-beeline-tutorial/tutorial_beeline_app.png) 

3. <span data-ttu-id="c8dc7-214">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c8dc7-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-216">Click **Add** button.</span></span> <span data-ttu-id="c8dc7-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c8dc7-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c8dc7-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8dc7-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c8dc7-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8dc7-222">Testing single sign-on</span></span>

<span data-ttu-id="c8dc7-223">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span> <span data-ttu-id="c8dc7-224">Ha a hozzáférési panelen Beeline csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Beeline alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c8dc7-224">When you click the Beeline tile in the Access Panel, you should get automatically signed-on to your Beeline application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8dc7-225">További források</span><span class="sxs-lookup"><span data-stu-id="c8dc7-225">Additional resources</span></span>

* [<span data-ttu-id="c8dc7-226">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c8dc7-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8dc7-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c8dc7-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-beeline-tutorial/tutorial_general_203.png

