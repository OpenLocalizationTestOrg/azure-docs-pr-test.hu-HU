---
title: "Oktatóanyag: Azure Active Directoryval integrált gyorsmeneti |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és gyorsmeneti között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e68cede7-1130-4813-bd55-22a9a6fc6bf5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: 515dd397c46df7c8c82afab9b50051e34db69d7a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-overdrive"></a><span data-ttu-id="f4730-103">Oktatóanyag: Azure Active Directoryval integrált gyorsmeneti</span><span class="sxs-lookup"><span data-stu-id="f4730-103">Tutorial: Azure Active Directory integration with Overdrive</span></span> 

<span data-ttu-id="f4730-104">Ebben az oktatóanyagban elsajátíthatja gyorsmeneti integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f4730-104">In this tutorial, you learn how to integrate Overdrive with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f4730-105">Gyorsmeneti integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f4730-105">Integrating Overdrive with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f4730-106">Megadhatja a gyorsmeneti hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f4730-106">You can control in Azure AD who has access to Overdrive</span></span> 
- <span data-ttu-id="f4730-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett gyorsmeneti (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f4730-107">You can enable your users to automatically get signed-on to Overdrive (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f4730-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f4730-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="f4730-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f4730-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f4730-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f4730-110">Prerequisites</span></span>

<span data-ttu-id="f4730-111">Az Azure AD-integrációs gyorsmeneti konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="f4730-111">To configure Azure AD integration with Overdrive, you need the following items:</span></span>

- <span data-ttu-id="f4730-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f4730-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f4730-113">Egy gyorsmeneti egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f4730-113">An Overdrive single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f4730-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f4730-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f4730-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f4730-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f4730-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f4730-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f4730-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f4730-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f4730-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f4730-118">Scenario description</span></span>
<span data-ttu-id="f4730-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f4730-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f4730-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f4730-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f4730-121">A gyűjteményből gyorsmeneti hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f4730-121">Adding Overdrive from the gallery</span></span>
2. <span data-ttu-id="f4730-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f4730-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-overdrive-from-the-gallery"></a><span data-ttu-id="f4730-123">A gyűjteményből gyorsmeneti hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f4730-123">Adding Overdrive from the gallery</span></span>
<span data-ttu-id="f4730-124">Az Azure AD-be gyorsmeneti integrálása konfigurálásához kell hozzáadnia gyorsmeneti a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f4730-124">To configure the integration of Overdrive into Azure AD, you need to add Overdrive from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f4730-125">**A gyűjteményből gyorsmeneti hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f4730-125">**To add Overdrive from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f4730-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f4730-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f4730-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f4730-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f4730-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f4730-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f4730-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="f4730-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f4730-133">Írja be a keresőmezőbe, **gyorsmeneti**.</span><span class="sxs-lookup"><span data-stu-id="f4730-133">In the search box, type **Overdrive**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_search.png)

5. <span data-ttu-id="f4730-135">Az eredmények panelen válassza ki a **gyorsmeneti**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f4730-135">In the results panel, select **Overdrive**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f4730-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f4730-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f4730-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján gyorsmeneti.</span><span class="sxs-lookup"><span data-stu-id="f4730-138">In this section, you configure and test Azure AD single sign-on with Overdrive based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f4730-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó gyorsmeneti a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f4730-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Overdrive is to a user in Azure AD.</span></span> <span data-ttu-id="f4730-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a gyorsmeneti közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f4730-140">In other words, a link relationship between an Azure AD user and the related user in Overdrive needs to be established.</span></span>

<span data-ttu-id="f4730-141">Gyorsmeneti, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="f4730-141">In Overdrive, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="f4730-142">Gyorsmeneti az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f4730-142">To configure and test Azure AD single sign-on with Overdrive, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f4730-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f4730-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f4730-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f4730-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f4730-145">**[Egy gyorsmeneti tesztfelhasználó létrehozása](#creating-an-overdrive-test-user)**  - való egy megfelelője a Britta Simon gyorsmeneti, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="f4730-145">**[Creating an Overdrive test user](#creating-an-overdrive-test-user)** - to have a counterpart of Britta Simon in Overdrive that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="f4730-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f4730-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f4730-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f4730-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f4730-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f4730-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f4730-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az gyorsmeneti alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f4730-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Overdrive  application.</span></span>

<span data-ttu-id="f4730-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés gyorsmeneti, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f4730-150">**To configure Azure AD single sign-on with Overdrive, perform the following steps:**</span></span>

1. <span data-ttu-id="f4730-151">Az Azure portálon a a **gyorsmeneti** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f4730-151">In the Azure portal, on the **Overdrive** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f4730-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f4730-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_samlbase.png)

3. <span data-ttu-id="f4730-155">Az a **gyorsmeneti tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="f4730-155">On the **Overdrive Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_url.png)

    <span data-ttu-id="f4730-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`http://<subdomain>.libraryreserve.com`</span><span class="sxs-lookup"><span data-stu-id="f4730-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<subdomain>.libraryreserve.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f4730-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="f4730-158">The value is not real.</span></span> <span data-ttu-id="f4730-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="f4730-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="f4730-160">Ügyfél [gyorsmeneti ügyfél-támogatási csoport](https://help.overdrive.com/) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="f4730-160">Contact [Overdrive Client support team](https://help.overdrive.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="f4730-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f4730-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_certificate.png) 

5. <span data-ttu-id="f4730-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f4730-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f4730-165">Egyszeri bejelentkezés konfigurálása **gyorsmeneti** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [gyorsmeneti támogatási csoport](https://help.overdrive.com/).</span><span class="sxs-lookup"><span data-stu-id="f4730-165">To configure single sign-on on **Overdrive** side, you need to send the downloaded **Metadata XML** to [Overdrive support team](https://help.overdrive.com/).</span></span> <span data-ttu-id="f4730-166">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="f4730-166">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f4730-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="f4730-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="f4730-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="f4730-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="f4730-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f4730-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f4730-170">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4730-170">Creating an Azure AD test user</span></span>
<span data-ttu-id="f4730-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f4730-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f4730-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f4730-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f4730-174">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f4730-174">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f4730-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f4730-176">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f4730-178">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f4730-178">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f4730-180">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f4730-180">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-overdrive-books-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f4730-182">a.</span><span class="sxs-lookup"><span data-stu-id="f4730-182">a.</span></span> <span data-ttu-id="f4730-183">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f4730-183">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f4730-184">b.</span><span class="sxs-lookup"><span data-stu-id="f4730-184">b.</span></span> <span data-ttu-id="f4730-185">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f4730-185">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f4730-186">c.</span><span class="sxs-lookup"><span data-stu-id="f4730-186">c.</span></span> <span data-ttu-id="f4730-187">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f4730-187">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f4730-188">d.</span><span class="sxs-lookup"><span data-stu-id="f4730-188">d.</span></span> <span data-ttu-id="f4730-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f4730-189">Click **Create**.</span></span>
 
### <a name="creating-an-overdrive-test-user"></a><span data-ttu-id="f4730-190">Egy gyorsmeneti tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f4730-190">Creating an Overdrive test user</span></span>

<span data-ttu-id="f4730-191">Nincs művelet elem ahhoz, hogy a felhasználók átadása gyorsmeneti való konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f4730-191">There is no action item for you to configure user provisioning to OverDrive.</span></span>  

<span data-ttu-id="f4730-192">Ha egy hozzárendelt felhasználó próbál bejelentkezni az gyorsmeneti, gyorsmeneti fiók automatikusan létrejön, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="f4730-192">When an assigned user tries to log in to OverDrive, an OverDrive account is automatically created if necessary.</span></span>

>[!NOTE]
><span data-ttu-id="f4730-193">Bármely más gyorsmeneti felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz gyorsmeneti által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="f4730-193">You can use any other OverDrive user account creation tools or APIs provided by OverDrive to provision AAD user accounts.</span></span>
>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f4730-194">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f4730-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f4730-195">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés gyorsmeneti Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f4730-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Overdrive.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f4730-197">**Britta Simon hozzárendelése gyorsmeneti, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f4730-197">**To assign Britta Simon to Overdrive, perform the following steps:**</span></span>

1. <span data-ttu-id="f4730-198">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f4730-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f4730-200">Az alkalmazások listában válassza ki a **gyorsmeneti**.</span><span class="sxs-lookup"><span data-stu-id="f4730-200">In the applications list, select **Overdrive**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-overdrive-books-tutorial/tutorial_overdrivebooks_app.png) 

3. <span data-ttu-id="f4730-202">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f4730-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f4730-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f4730-204">Click **Add** button.</span></span> <span data-ttu-id="f4730-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f4730-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f4730-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f4730-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f4730-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f4730-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f4730-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f4730-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f4730-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f4730-210">Testing single sign-on</span></span>

<span data-ttu-id="f4730-211">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="f4730-211">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f4730-212">Ha a hozzáférési panelen gyorsmeneti csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az gyorsmeneti alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f4730-212">When you click the Overdrive tile in the Access Panel, you should get automatically signed-on to your Overdrive application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f4730-213">További források</span><span class="sxs-lookup"><span data-stu-id="f4730-213">Additional resources</span></span>

* [<span data-ttu-id="f4730-214">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f4730-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f4730-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f4730-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-overdrive-books-tutorial/tutorial_general_203.png

