---
title: "Oktatóanyag: Azure Active Directoryval integrált mezőben |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a mező között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5f3517f8-30f2-4be7-9e47-43d702701797
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 2cc2afe8ff3f0063224c94eb0b8135347051b0aa
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-box"></a><span data-ttu-id="54ceb-103">Oktatóanyag: Azure Active Directory-integráció segítségével</span><span class="sxs-lookup"><span data-stu-id="54ceb-103">Tutorial: Azure Active Directory integration with Box</span></span>

<span data-ttu-id="54ceb-104">Ebben az oktatóanyagban elsajátíthatja mezőben integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="54ceb-104">In this tutorial, you learn how to integrate Box with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="54ceb-105">Mezőbe integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="54ceb-105">Integrating Box with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="54ceb-106">Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik a mezőben</span><span class="sxs-lookup"><span data-stu-id="54ceb-106">You can control in Azure AD who has access to Box</span></span>
- <span data-ttu-id="54ceb-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett (egyszeri bejelentkezés) mezőben az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="54ceb-107">You can enable your users to automatically get signed-on to Box (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="54ceb-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="54ceb-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="54ceb-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="54ceb-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="54ceb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54ceb-110">Prerequisites</span></span>

<span data-ttu-id="54ceb-111">Az Azure AD-integráció konfigurálása a jelölését, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="54ceb-111">To configure Azure AD integration with Box, you need the following items:</span></span>

- <span data-ttu-id="54ceb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="54ceb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="54ceb-113">A mezőben egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="54ceb-113">A Box single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="54ceb-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="54ceb-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="54ceb-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="54ceb-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="54ceb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="54ceb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="54ceb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="54ceb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="54ceb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="54ceb-118">Scenario description</span></span>
<span data-ttu-id="54ceb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="54ceb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="54ceb-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="54ceb-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="54ceb-121">Mező hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="54ceb-121">Adding Box from the gallery</span></span>
2. <span data-ttu-id="54ceb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="54ceb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-box-from-the-gallery"></a><span data-ttu-id="54ceb-123">Mező hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="54ceb-123">Adding Box from the gallery</span></span>
<span data-ttu-id="54ceb-124">Konfigurálása az Azure AD integrálása a mezőbe, szükség mező hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="54ceb-124">To configure the integration of Box into Azure AD, you need to add Box from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="54ceb-125">**Adja hozzá a mezőbe a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54ceb-125">**To add Box from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="54ceb-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="54ceb-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="54ceb-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="54ceb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="54ceb-131">Kattintson a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="54ceb-131">Click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="54ceb-133">Írja be a keresőmezőbe, **mezőben**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-133">In the search box, type **Box**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/tutorial_box_search.png)

5. <span data-ttu-id="54ceb-135">Az eredmények panelen válassza ki a **mezőben**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="54ceb-135">In the results panel, select **Box**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/tutorial_box_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="54ceb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="54ceb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="54ceb-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="54ceb-138">In this section, you configure and test Azure AD single sign-on with Box based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="54ceb-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó mezőben a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="54ceb-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Box is to a user in Azure AD.</span></span> <span data-ttu-id="54ceb-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a mezőbe közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="54ceb-140">In other words, a link relationship between an Azure AD user and the related user in Box needs to be established.</span></span>

<span data-ttu-id="54ceb-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="54ceb-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Box.</span></span>

<span data-ttu-id="54ceb-142">Az Azure AD egyszeri bejelentkezést a jelölését tesztelése és konfigurálása, kell végrehajtani a következő építőelemeket:</span><span class="sxs-lookup"><span data-stu-id="54ceb-142">To configure and test Azure AD single sign-on with Box, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="54ceb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="54ceb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="54ceb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="54ceb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="54ceb-145">**[A mezőbe tesztfelhasználó létrehozása](#creating-a-box-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon be, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="54ceb-145">**[Creating a Box test user](#creating-a-box-test-user)** - to have a counterpart of Britta Simon in Box that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="54ceb-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="54ceb-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="54ceb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="54ceb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="54ceb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ceb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="54ceb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az mezőben alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="54ceb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Box application.</span></span>

<span data-ttu-id="54ceb-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása mezőbe, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="54ceb-150">**To configure Azure AD single sign-on with Box, perform the following steps:**</span></span>

1. <span data-ttu-id="54ceb-151">Az Azure portálon a a **mezőben** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-151">In the Azure portal, on the **Box** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="54ceb-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="54ceb-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_samlbase.png)

3. <span data-ttu-id="54ceb-155">Az a **mezőben tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="54ceb-155">On the **Box Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_url.png)

    <span data-ttu-id="54ceb-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.box.com`</span><span class="sxs-lookup"><span data-stu-id="54ceb-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.box.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="54ceb-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="54ceb-158">This value is not real.</span></span> <span data-ttu-id="54ceb-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="54ceb-159">Update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="54ceb-160">Ügyfél [mezőben ügyfél-támogatási csoport](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="54ceb-160">Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) to get this value.</span></span> 
 
4. <span data-ttu-id="54ceb-161">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="54ceb-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_certificate.png) 

5. <span data-ttu-id="54ceb-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="54ceb-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="54ceb-165">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [mezőben ügyfél-támogatási csoport](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) és adja meg a letöltött XML-fájl.</span><span class="sxs-lookup"><span data-stu-id="54ceb-165">To get SSO configured for your application, Contact [Box Client support team](https://community.box.com/t5/custom/page/page-id/submit_sso_questionaire) and provide them with the downloaded XML file.</span></span>

> [!TIP]
> <span data-ttu-id="54ceb-166">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="54ceb-166">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="54ceb-167">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="54ceb-167">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="54ceb-168">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="54ceb-168">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="54ceb-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="54ceb-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="54ceb-170">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="54ceb-170">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="54ceb-172">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54ceb-172">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="54ceb-173">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="54ceb-173">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="54ceb-175">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-175">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="54ceb-177">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="54ceb-177">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="54ceb-179">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="54ceb-179">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-box-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="54ceb-181">a.</span><span class="sxs-lookup"><span data-stu-id="54ceb-181">a.</span></span> <span data-ttu-id="54ceb-182">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-182">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="54ceb-183">b.</span><span class="sxs-lookup"><span data-stu-id="54ceb-183">b.</span></span> <span data-ttu-id="54ceb-184">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="54ceb-184">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="54ceb-185">c.</span><span class="sxs-lookup"><span data-stu-id="54ceb-185">c.</span></span> <span data-ttu-id="54ceb-186">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-186">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="54ceb-187">d.</span><span class="sxs-lookup"><span data-stu-id="54ceb-187">d.</span></span> <span data-ttu-id="54ceb-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="54ceb-188">Click **Create**.</span></span>
 
### <a name="creating-a-box-test-user"></a><span data-ttu-id="54ceb-189">A mezőben tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="54ceb-189">Creating a Box test user</span></span>

<span data-ttu-id="54ceb-190">Ebben a szakaszban a felhasználók Britta Simon nevű létrehozása a mezőbe.</span><span class="sxs-lookup"><span data-stu-id="54ceb-190">In this section, a user called Britta Simon is created in Box.</span></span> <span data-ttu-id="54ceb-191">Mezőbe támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="54ceb-191">Box supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="54ceb-192">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="54ceb-192">There is no action item for you in this section.</span></span> <span data-ttu-id="54ceb-193">Ha a felhasználó nem létezik a mezőbe, egy új mező elérésére tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="54ceb-193">If a user doesn't already exist in Box, a new one is created when you attempt to access Box.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="54ceb-194">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="54ceb-194">Assigning the Azure AD test user</span></span>

<span data-ttu-id="54ceb-195">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés be Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="54ceb-195">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Box.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="54ceb-197">**Britta Simon hozzárendelése mezőben, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="54ceb-197">**To assign Britta Simon to Box, perform the following steps:**</span></span>

1. <span data-ttu-id="54ceb-198">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-198">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="54ceb-200">Az alkalmazások listában válassza ki a **mezőben**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-200">In the applications list, select **Box**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-box-tutorial/tutorial_box_app.png) 

3. <span data-ttu-id="54ceb-202">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="54ceb-202">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="54ceb-204">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="54ceb-204">Click **Add** button.</span></span> <span data-ttu-id="54ceb-205">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54ceb-205">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="54ceb-207">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="54ceb-207">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="54ceb-208">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54ceb-208">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="54ceb-209">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="54ceb-209">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="54ceb-210">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="54ceb-210">Testing single sign-on</span></span>

<span data-ttu-id="54ceb-211">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="54ceb-211">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="54ceb-212">Ha a hozzáférési Panel bezárásához mozaik gombra kattint, bejelentkezési oldalt az beszerzése aláírt-on mezőben Alkalmazásmódosítások szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="54ceb-212">When you click the Box tile in the Access Panel, you should get login page to get signed-on to your Box application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54ceb-213">További források</span><span class="sxs-lookup"><span data-stu-id="54ceb-213">Additional resources</span></span>

* [<span data-ttu-id="54ceb-214">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="54ceb-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="54ceb-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="54ceb-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="54ceb-216">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="54ceb-216">Configure User Provisioning</span></span>](active-directory-saas-box-userprovisioning-tutorial.md)


<!--Image references-->

[1]: ./media/active-directory-saas-box-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-box-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-box-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-box-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-box-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-box-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-box-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-box-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-box-tutorial/tutorial_general_203.png

