---
title: "Oktatóanyag: Azure Active Directoryval integrált Mindflash |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Mindflash között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: bdf91993-aaaa-4598-89b7-77ef8ca065d5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/30/2017
ms.author: jeedes
ms.openlocfilehash: 90de7b6a82d88f9407a35fbfebe8a652928d76cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mindflash"></a><span data-ttu-id="20b26-103">Oktatóanyag: Azure Active Directoryval integrált Mindflash</span><span class="sxs-lookup"><span data-stu-id="20b26-103">Tutorial: Azure Active Directory integration with Mindflash</span></span>

<span data-ttu-id="20b26-104">Ebben az oktatóanyagban elsajátíthatja Mindflash integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20b26-104">In this tutorial, you learn how to integrate Mindflash with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20b26-105">Mindflash integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="20b26-105">Integrating Mindflash with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="20b26-106">Megadhatja a Mindflash hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="20b26-106">You can control in Azure AD who has access to Mindflash</span></span>
- <span data-ttu-id="20b26-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Mindflash (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="20b26-107">You can enable your users to automatically get signed-on to Mindflash (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="20b26-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="20b26-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="20b26-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20b26-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20b26-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="20b26-110">Prerequisites</span></span>

<span data-ttu-id="20b26-111">Konfigurálása az Azure AD-integrációs Mindflash, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="20b26-111">To configure Azure AD integration with Mindflash, you need the following items:</span></span>

- <span data-ttu-id="20b26-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="20b26-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20b26-113">Egy Mindflash egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="20b26-113">A Mindflash single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="20b26-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="20b26-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="20b26-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="20b26-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20b26-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="20b26-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="20b26-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20b26-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="20b26-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="20b26-118">Scenario description</span></span>
<span data-ttu-id="20b26-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="20b26-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20b26-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="20b26-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20b26-121">A gyűjteményből Mindflash hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20b26-121">Adding Mindflash from the gallery</span></span>
2. <span data-ttu-id="20b26-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20b26-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mindflash-from-the-gallery"></a><span data-ttu-id="20b26-123">A gyűjteményből Mindflash hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20b26-123">Adding Mindflash from the gallery</span></span>
<span data-ttu-id="20b26-124">Az Azure AD integrálása a Mindflash konfigurálásához kell hozzáadnia Mindflash a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="20b26-124">To configure the integration of Mindflash into Azure AD, you need to add Mindflash from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="20b26-125">**A gyűjteményből Mindflash hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="20b26-125">**To add Mindflash from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="20b26-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20b26-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="20b26-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="20b26-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="20b26-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20b26-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="20b26-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="20b26-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="20b26-133">Írja be a keresőmezőbe, **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="20b26-133">In the search box, type **Mindflash**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_search.png)

5. <span data-ttu-id="20b26-135">Az eredmények panelen válassza ki a **Mindflash**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="20b26-135">In the results panel, select **Mindflash**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="20b26-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20b26-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="20b26-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Mindflash.</span><span class="sxs-lookup"><span data-stu-id="20b26-138">In this section, you configure and test Azure AD single sign-on with Mindflash based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="20b26-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Mindflash a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="20b26-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Mindflash is to a user in Azure AD.</span></span> <span data-ttu-id="20b26-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Mindflash közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="20b26-140">In other words, a link relationship between an Azure AD user and the related user in Mindflash needs to be established.</span></span>

<span data-ttu-id="20b26-141">Mindflash, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="20b26-141">In Mindflash, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="20b26-142">Az Azure AD egyszeri bejelentkezést a Mindflash tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="20b26-142">To configure and test Azure AD single sign-on with Mindflash, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="20b26-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="20b26-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="20b26-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="20b26-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="20b26-145">**[Mindflash tesztfelhasználó létrehozása](#creating-a-mindflash-test-user)**  - való Britta Simon valami Mindflash, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="20b26-145">**[Creating a Mindflash test user](#creating-a-mindflash-test-user)** - to have a counterpart of Britta Simon in Mindflash that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="20b26-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="20b26-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20b26-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="20b26-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="20b26-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20b26-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="20b26-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Mindflash alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="20b26-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Mindflash application.</span></span>

<span data-ttu-id="20b26-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Mindflash, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="20b26-150">**To configure Azure AD single sign-on with Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="20b26-151">Az Azure portálon a a **Mindflash** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="20b26-151">In the Azure portal, on the **Mindflash** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="20b26-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="20b26-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_samlbase.png)

3. <span data-ttu-id="20b26-155">Az a **Mindflash tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="20b26-155">On the **Mindflash Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_url.png)

    <span data-ttu-id="20b26-157">a.</span><span class="sxs-lookup"><span data-stu-id="20b26-157">a.</span></span> <span data-ttu-id="20b26-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="20b26-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    <span data-ttu-id="20b26-159">b.</span><span class="sxs-lookup"><span data-stu-id="20b26-159">b.</span></span> <span data-ttu-id="20b26-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.mindflash.com`</span><span class="sxs-lookup"><span data-stu-id="20b26-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.mindflash.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="20b26-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="20b26-161">These values are not real.</span></span> <span data-ttu-id="20b26-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="20b26-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="20b26-163">Ügyfél [Mindflash ügyfél-támogatási csoport](https://www.mindflash.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="20b26-163">Contact [Mindflash Client support team](https://www.mindflash.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="20b26-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="20b26-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_certificate.png) 

5. <span data-ttu-id="20b26-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="20b26-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="20b26-168">Egyszeri bejelentkezés konfigurálása **Mindflash** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Mindflash támogatási csoport](https://www.mindflash.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="20b26-168">To configure single sign-on on **Mindflash** side, you need to send the downloaded **Metadata XML** to [Mindflash support team](https://www.mindflash.com/contact/).</span></span>

> [!TIP]
> <span data-ttu-id="20b26-169">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="20b26-169">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="20b26-170">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="20b26-170">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="20b26-171">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="20b26-171">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="20b26-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20b26-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="20b26-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="20b26-173">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="20b26-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="20b26-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="20b26-176">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20b26-176">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="20b26-178">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="20b26-178">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="20b26-180">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="20b26-180">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="20b26-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="20b26-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mindflash-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="20b26-184">a.</span><span class="sxs-lookup"><span data-stu-id="20b26-184">a.</span></span> <span data-ttu-id="20b26-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20b26-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20b26-186">b.</span><span class="sxs-lookup"><span data-stu-id="20b26-186">b.</span></span> <span data-ttu-id="20b26-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="20b26-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="20b26-188">c.</span><span class="sxs-lookup"><span data-stu-id="20b26-188">c.</span></span> <span data-ttu-id="20b26-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="20b26-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="20b26-190">d.</span><span class="sxs-lookup"><span data-stu-id="20b26-190">d.</span></span> <span data-ttu-id="20b26-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="20b26-191">Click **Create**.</span></span>
 
### <a name="creating-a-mindflash-test-user"></a><span data-ttu-id="20b26-192">Mindflash tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20b26-192">Creating a Mindflash test user</span></span>

<span data-ttu-id="20b26-193">Ahhoz, hogy az Azure AD-felhasználók Mindflash bejelentkezni, akkor ki kell építenie Mindflash be.</span><span class="sxs-lookup"><span data-stu-id="20b26-193">In order to enable Azure AD users to log into Mindflash, they must be provisioned into Mindflash.</span></span> <span data-ttu-id="20b26-194">Mindflash, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="20b26-194">In the case of Mindflash, provisioning is a manual task.</span></span>

### <a name="to-provision-a-user-accounts-perform-the-following-steps"></a><span data-ttu-id="20b26-195">A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="20b26-195">To provision a user accounts, perform the following steps:</span></span>

1. <span data-ttu-id="20b26-196">Jelentkezzen be a **Mindflash** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="20b26-196">Log in to your **Mindflash** company site as an administrator.</span></span>

2. <span data-ttu-id="20b26-197">Ugrás a **felhasználók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="20b26-197">Go to **Manage Users**.</span></span>
   
    <span data-ttu-id="20b26-198">![Felhasználók kezelése](./media/active-directory-saas-mindflash-tutorial/ic787140.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="20b26-198">![Manage Users](./media/active-directory-saas-mindflash-tutorial/ic787140.png "Manage Users")</span></span>

3. <span data-ttu-id="20b26-199">Kattintson a **felhasználó hozzáadása**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="20b26-199">Click the **Add Users**, and then click **New**.</span></span>

4. <span data-ttu-id="20b26-200">Az a **az új felhasználók hozzáadása** területen az alábbi lépésekkel egy érvényes Azure AD fiók kiépítése:</span><span class="sxs-lookup"><span data-stu-id="20b26-200">In the **Add New Users** section, perform the following steps of a valid Azure AD account you want to provision:</span></span>
   
    <span data-ttu-id="20b26-201">![Új felhasználók hozzáadása az](./media/active-directory-saas-mindflash-tutorial/ic787141.png "új felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="20b26-201">![Add New Users](./media/active-directory-saas-mindflash-tutorial/ic787141.png "Add New Users")</span></span>
   
    <span data-ttu-id="20b26-202">a.</span><span class="sxs-lookup"><span data-stu-id="20b26-202">a.</span></span> <span data-ttu-id="20b26-203">Az a **Utónév** szövegmezőhöz típus **Utónév** , a felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="20b26-203">In the **First name** textbox, type **First name** of the user as **Britta**.</span></span>

    <span data-ttu-id="20b26-204">b.</span><span class="sxs-lookup"><span data-stu-id="20b26-204">b.</span></span> <span data-ttu-id="20b26-205">Az a **Vezetéknév** szövegmezőhöz típus **Vezetéknév** , a felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="20b26-205">In the **Last name** textbox, type **Last name** of the user as **Simon**.</span></span>
    
    <span data-ttu-id="20b26-206">c.</span><span class="sxs-lookup"><span data-stu-id="20b26-206">c.</span></span> <span data-ttu-id="20b26-207">Az a **E-mail** szövegmezőhöz típus **E-mail cím** , a felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="20b26-207">In the **Email** textbox, type **Email Address** of the user as **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="20b26-208">b.</span><span class="sxs-lookup"><span data-stu-id="20b26-208">b.</span></span> <span data-ttu-id="20b26-209">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="20b26-209">Click **Add**.</span></span>

>[!NOTE]
><span data-ttu-id="20b26-210">Bármely más Mindflash felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Mindflash által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="20b26-210">You can use any other Mindflash user account creation tools or APIs provided by Mindflash to provision AAD user accounts.</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="20b26-211">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="20b26-211">Assigning the Azure AD test user</span></span>

<span data-ttu-id="20b26-212">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Mindflash Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="20b26-212">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Mindflash.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="20b26-214">**Britta Simon hozzárendelése Mindflash, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="20b26-214">**To assign Britta Simon to Mindflash, perform the following steps:**</span></span>

1. <span data-ttu-id="20b26-215">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20b26-215">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="20b26-217">Az alkalmazások listában válassza ki a **Mindflash**.</span><span class="sxs-lookup"><span data-stu-id="20b26-217">In the applications list, select **Mindflash**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mindflash-tutorial/tutorial_mindflash_app.png) 

3. <span data-ttu-id="20b26-219">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="20b26-219">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="20b26-221">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="20b26-221">Click **Add** button.</span></span> <span data-ttu-id="20b26-222">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20b26-222">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="20b26-224">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="20b26-224">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="20b26-225">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20b26-225">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20b26-226">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20b26-226">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="20b26-227">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="20b26-227">Testing single sign-on</span></span>

<span data-ttu-id="20b26-228">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="20b26-228">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="20b26-229">Ha a hozzáférési panelen Mindflash csempére kattint, Mindflash alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="20b26-229">When you click the Mindflash tile in the Access Panel, you should get login page of Mindflash application.</span></span>
<span data-ttu-id="20b26-230">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="20b26-230">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="20b26-231">További források</span><span class="sxs-lookup"><span data-stu-id="20b26-231">Additional resources</span></span>

* [<span data-ttu-id="20b26-232">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="20b26-232">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20b26-233">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="20b26-233">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mindflash-tutorial/tutorial_general_203.png

