---
title: "Oktatóanyag: Azure Active Directoryval integrált EasyTerritory |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és EasyTerritory között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: d29b362d-e986-4f67-8ff2-e158e49353aa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 46f99496397e2ed39b1d9410453dac7983ced612
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-easyterritory"></a><span data-ttu-id="dee97-103">Oktatóanyag: Azure Active Directoryval integrált EasyTerritory</span><span class="sxs-lookup"><span data-stu-id="dee97-103">Tutorial: Azure Active Directory integration with EasyTerritory</span></span>

<span data-ttu-id="dee97-104">Ebben az oktatóanyagban elsajátíthatja EasyTerritory integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dee97-104">In this tutorial, you learn how to integrate EasyTerritory with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dee97-105">EasyTerritory integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dee97-105">Integrating EasyTerritory with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dee97-106">Az Azure AD, aki hozzáfér EasyTerritory szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="dee97-106">You can control in Azure AD who has access to EasyTerritory.</span></span>
- <span data-ttu-id="dee97-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett EasyTerritory (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="dee97-107">You can enable your users to automatically get signed-on to EasyTerritory (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="dee97-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="dee97-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="dee97-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dee97-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dee97-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dee97-110">Prerequisites</span></span>

<span data-ttu-id="dee97-111">Konfigurálása az Azure AD-integrációs EasyTerritory, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dee97-111">To configure Azure AD integration with EasyTerritory, you need the following items:</span></span>

- <span data-ttu-id="dee97-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dee97-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dee97-113">Egy EasyTerritory egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="dee97-113">A EasyTerritory single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dee97-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dee97-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dee97-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dee97-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dee97-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dee97-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dee97-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dee97-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dee97-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dee97-118">Scenario description</span></span>
<span data-ttu-id="dee97-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dee97-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dee97-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dee97-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dee97-121">A gyűjteményből EasyTerritory hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dee97-121">Adding EasyTerritory from the gallery</span></span>
2. <span data-ttu-id="dee97-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dee97-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-easyterritory-from-the-gallery"></a><span data-ttu-id="dee97-123">A gyűjteményből EasyTerritory hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dee97-123">Adding EasyTerritory from the gallery</span></span>
<span data-ttu-id="dee97-124">Az Azure AD integrálása a EasyTerritory konfigurálásához kell hozzáadnia EasyTerritory a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dee97-124">To configure the integration of EasyTerritory into Azure AD, you need to add EasyTerritory from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dee97-125">**A gyűjteményből EasyTerritory hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dee97-125">**To add EasyTerritory from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dee97-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dee97-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="dee97-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dee97-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dee97-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dee97-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="dee97-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dee97-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="dee97-133">Írja be a keresőmezőbe, **EasyTerritory**, jelölje be **EasyTerritory** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dee97-133">In the search box, type **EasyTerritory**, select **EasyTerritory** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában EasyTerritory](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="dee97-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dee97-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="dee97-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján EasyTerritory.</span><span class="sxs-lookup"><span data-stu-id="dee97-136">In this section, you configure and test Azure AD single sign-on with EasyTerritory based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dee97-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó EasyTerritory a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dee97-137">For single sign-on to work, Azure AD needs to know what the counterpart user in EasyTerritory is to a user in Azure AD.</span></span> <span data-ttu-id="dee97-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a EasyTerritory közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dee97-138">In other words, a link relationship between an Azure AD user and the related user in EasyTerritory needs to be established.</span></span>

<span data-ttu-id="dee97-139">EasyTerritory, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dee97-139">In EasyTerritory, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dee97-140">Az Azure AD egyszeri bejelentkezést a EasyTerritory tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dee97-140">To configure and test Azure AD single sign-on with EasyTerritory, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dee97-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dee97-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dee97-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dee97-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dee97-143">**[EasyTerritory tesztfelhasználó létrehozása](#create-a-easyterritory-test-user)**  - való Britta Simon valami EasyTerritory, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dee97-143">**[Create a EasyTerritory test user](#create-a-easyterritory-test-user)** - to have a counterpart of Britta Simon in EasyTerritory that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dee97-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dee97-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dee97-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dee97-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="dee97-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dee97-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="dee97-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az EasyTerritory alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dee97-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your EasyTerritory application.</span></span>

<span data-ttu-id="dee97-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés EasyTerritory, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dee97-148">**To configure Azure AD single sign-on with EasyTerritory, perform the following steps:**</span></span>

1. <span data-ttu-id="dee97-149">Az Azure portálon a a **EasyTerritory** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dee97-149">In the Azure portal, on the **EasyTerritory** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="dee97-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dee97-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_samlbase.png)

3. <span data-ttu-id="dee97-153">Az a **EasyTerritory tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás által kezdeményezett IDP módban, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dee97-153">On the **EasyTerritory Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk EasyTerritory tartomány és az URL-címek](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_url.png)

    <span data-ttu-id="dee97-155">a.</span><span class="sxs-lookup"><span data-stu-id="dee97-155">a.</span></span> <span data-ttu-id="dee97-156">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://apps.easyterritory.com/<tenant id>/dev/`</span><span class="sxs-lookup"><span data-stu-id="dee97-156">In the **Identifier** textbox, type a URL using the following pattern: `https://apps.easyterritory.com/<tenant id>/dev/`</span></span>

    <span data-ttu-id="dee97-157">b.</span><span class="sxs-lookup"><span data-stu-id="dee97-157">b.</span></span> <span data-ttu-id="dee97-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://apps.easyterritory.com/<tenant id>/dev/authservices/acs`</span><span class="sxs-lookup"><span data-stu-id="dee97-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://apps.easyterritory.com/<tenant id>/dev/authservices/acs`</span></span>

4. <span data-ttu-id="dee97-159">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="dee97-159">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk EasyTerritory tartomány és az URL-címek](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_url1.png)

    <span data-ttu-id="dee97-161">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.easyterritory.com/`</span><span class="sxs-lookup"><span data-stu-id="dee97-161">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.easyterritory.com/`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="dee97-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="dee97-162">These values are not real.</span></span> <span data-ttu-id="dee97-163">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="dee97-163">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="dee97-164">Ügyfél [EasyTerritory ügyfél-támogatási csoport](mailto:sales@easyterritory.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="dee97-164">Contact [EasyTerritory Client support team](mailto:sales@easyterritory.com) to get these values.</span></span> 

5. <span data-ttu-id="dee97-165">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dee97-165">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_certificate.png) 

6. <span data-ttu-id="dee97-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dee97-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-easyterritory-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="dee97-169">Egyszeri bejelentkezés konfigurálása **EasyTerritory** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [EasyTerritory támogatási csoport](mailto:sales@easyterritory.com).</span><span class="sxs-lookup"><span data-stu-id="dee97-169">To configure single sign-on on **EasyTerritory** side, you need to send the downloaded **Metadata XML** to [EasyTerritory support team](mailto:sales@easyterritory.com).</span></span> <span data-ttu-id="dee97-170">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="dee97-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="dee97-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dee97-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dee97-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dee97-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dee97-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dee97-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="dee97-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="dee97-174">Create an Azure AD test user</span></span>

<span data-ttu-id="dee97-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dee97-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="dee97-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dee97-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dee97-178">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="dee97-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="dee97-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dee97-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="dee97-182">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dee97-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="dee97-184">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dee97-184">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-easyterritory-tutorial/create_aaduser_04.png)

    <span data-ttu-id="dee97-186">a.</span><span class="sxs-lookup"><span data-stu-id="dee97-186">a.</span></span> <span data-ttu-id="dee97-187">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dee97-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dee97-188">b.</span><span class="sxs-lookup"><span data-stu-id="dee97-188">b.</span></span> <span data-ttu-id="dee97-189">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dee97-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="dee97-190">c.</span><span class="sxs-lookup"><span data-stu-id="dee97-190">c.</span></span> <span data-ttu-id="dee97-191">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="dee97-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="dee97-192">d.</span><span class="sxs-lookup"><span data-stu-id="dee97-192">d.</span></span> <span data-ttu-id="dee97-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dee97-193">Click **Create**.</span></span>
 
### <a name="create-a-easyterritory-test-user"></a><span data-ttu-id="dee97-194">EasyTerritory tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dee97-194">Create a EasyTerritory test user</span></span>

<span data-ttu-id="dee97-195">Ebben a szakaszban egy EasyTerritory Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dee97-195">In this section, you create a user called Britta Simon in EasyTerritory.</span></span> <span data-ttu-id="dee97-196">Adjon együttműködve [EasyTerritory támogatási csoport](mailto:sales@easyterritory.com) a felhasználók hozzáadása a EasyTerritory platform.</span><span class="sxs-lookup"><span data-stu-id="dee97-196">Please work with [EasyTerritory support team](mailto:sales@easyterritory.com) to add the users in the EasyTerritory platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="dee97-197">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="dee97-197">Assign the Azure AD test user</span></span>

<span data-ttu-id="dee97-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés EasyTerritory Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dee97-198">In this section, you enable Britta Simon to use Azure single sign-on by granting access to EasyTerritory.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="dee97-200">**Britta Simon hozzárendelése EasyTerritory, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dee97-200">**To assign Britta Simon to EasyTerritory, perform the following steps:**</span></span>

1. <span data-ttu-id="dee97-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dee97-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dee97-203">Az alkalmazások listában válassza ki a **EasyTerritory**.</span><span class="sxs-lookup"><span data-stu-id="dee97-203">In the applications list, select **EasyTerritory**.</span></span>

    ![Az alkalmazások listáját a EasyTerritory hivatkozás](./media/active-directory-saas-easyterritory-tutorial/tutorial_easyterritory_app.png)  

3. <span data-ttu-id="dee97-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dee97-205">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="dee97-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dee97-207">Click **Add** button.</span></span> <span data-ttu-id="dee97-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dee97-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="dee97-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dee97-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dee97-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dee97-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dee97-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dee97-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="dee97-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dee97-213">Test single sign-on</span></span>

<span data-ttu-id="dee97-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dee97-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dee97-215">Ha a hozzáférési panelen EasyTerritory csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az EasyTerritory alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="dee97-215">When you click the EasyTerritory tile in the Access Panel, you should get automatically signed-on to your EasyTerritory application.</span></span>
<span data-ttu-id="dee97-216">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dee97-216">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="dee97-217">További források</span><span class="sxs-lookup"><span data-stu-id="dee97-217">Additional resources</span></span>

* [<span data-ttu-id="dee97-218">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dee97-218">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dee97-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dee97-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)




<!--Image references-->

[1]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-easyterritory-tutorial/tutorial_general_203.png

