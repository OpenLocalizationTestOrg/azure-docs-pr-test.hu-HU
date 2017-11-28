---
title: "Oktatóanyag: Azure Active Directoryval integrált Neota logika Studio |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Neota logika Studio között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 842605e6-a91d-42cc-a0bb-e23e67173ae2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: jeedes
ms.openlocfilehash: 99018277392cab44a6b579ad45b4611739a803d8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-neota-logic-studio"></a><span data-ttu-id="cebb8-103">Oktatóanyag: Azure Active Directoryval integrált Neota logika Studio</span><span class="sxs-lookup"><span data-stu-id="cebb8-103">Tutorial: Azure Active Directory integration with Neota Logic Studio</span></span>

<span data-ttu-id="cebb8-104">Ebben az oktatóanyagban elsajátíthatja Neota logika Studio integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cebb8-104">In this tutorial, you learn how to integrate Neota Logic Studio with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cebb8-105">Neota logika Studio integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="cebb8-105">Integrating Neota Logic Studio with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="cebb8-106">Megadhatja a Neota logika Studio hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cebb8-106">You can control in Azure AD who has access to Neota Logic Studio</span></span>
- <span data-ttu-id="cebb8-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Neota logika Studio (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="cebb8-107">You can enable your users to automatically get signed-on to Neota Logic Studio (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cebb8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="cebb8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="cebb8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="cebb8-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="cebb8-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cebb8-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cebb8-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cebb8-111">Prerequisites</span></span>

<span data-ttu-id="cebb8-112">Az Azure AD-integráció konfigurálása a Neota logika Studio, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="cebb8-112">To configure Azure AD integration with Neota Logic Studio, you need the following items:</span></span>

- <span data-ttu-id="cebb8-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cebb8-113">An Azure AD subscription</span></span>
- <span data-ttu-id="cebb8-114">Egy Neota logika Studio egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="cebb8-114">A Neota Logic Studio single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cebb8-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="cebb8-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cebb8-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="cebb8-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cebb8-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cebb8-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="cebb8-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cebb8-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cebb8-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cebb8-119">Scenario description</span></span>

<span data-ttu-id="cebb8-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cebb8-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cebb8-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cebb8-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cebb8-122">A gyűjteményből Neota logika Studio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cebb8-122">Adding Neota Logic Studio from the gallery</span></span>
2. <span data-ttu-id="cebb8-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cebb8-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-neota-logic-studio-from-the-gallery"></a><span data-ttu-id="cebb8-124">A gyűjteményből Neota logika Studio hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cebb8-124">Adding Neota Logic Studio from the gallery</span></span>

<span data-ttu-id="cebb8-125">Az Azure AD integrálása a Neota logika Studio konfigurálásához kell hozzáadnia Neota logika Studio a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="cebb8-125">To configure the integration of Neota Logic Studio into Azure AD, you need to add Neota Logic Studio from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="cebb8-126">**A gyűjteményből Neota logika Studio hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebb8-126">**To add Neota Logic Studio from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="cebb8-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cebb8-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="cebb8-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cebb8-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cebb8-134">Írja be a keresőmezőbe, **Neota logika Studio**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-134">In the search box, type **Neota Logic Studio**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_search.png)

5. <span data-ttu-id="cebb8-136">Az eredmények panelen válassza ki a **Neota logika Studio**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="cebb8-136">In the results panel, select **Neota Logic Studio**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cebb8-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cebb8-138">Configuring and testing Azure AD single sign-on</span></span>

<span data-ttu-id="cebb8-139">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Neota logika Studio "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="cebb8-139">In this section, you configure and test Azure AD single sign-on with Neota Logic Studio based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="cebb8-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Neota logika Studio a felhasználók az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cebb8-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Neota Logic Studio is to a user in Azure AD.</span></span> <span data-ttu-id="cebb8-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Neota logika Studio közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="cebb8-141">In other words, a link relationship between an Azure AD user and the related user in Neota Logic Studio needs to be established.</span></span>

<span data-ttu-id="cebb8-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Neota logika Studio.</span><span class="sxs-lookup"><span data-stu-id="cebb8-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Neota Logic Studio.</span></span>

<span data-ttu-id="cebb8-143">Az Azure AD az egyszeri bejelentkezés Neota logika Studio tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="cebb8-143">To configure and test Azure AD single sign-on with Neota Logic Studio, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="cebb8-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="cebb8-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="cebb8-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="cebb8-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cebb8-146">**[Neota logika Studio tesztfelhasználó létrehozása](#creating-a-neota-logic-studio-test-user)**  - való egy megfelelője a Britta Simon Neota logika Studio, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="cebb8-146">**[Creating a Neota Logic Studio test user](#creating-a-neota-logic-studio-test-user)** - to have a counterpart of Britta Simon in Neota Logic Studio that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="cebb8-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cebb8-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cebb8-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cebb8-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cebb8-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cebb8-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cebb8-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Neota logika Studio alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cebb8-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Neota Logic Studio application.</span></span>

<span data-ttu-id="cebb8-151">**Az Azure AD az egyszeri bejelentkezés konfigurálása Neota logika Studio, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="cebb8-151">**To configure Azure AD single sign-on with Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="cebb8-152">Az Azure portálon a a **Neota logika Studio** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-152">In the Azure portal, on the **Neota Logic Studio** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cebb8-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="cebb8-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_samlbase.png)

3. <span data-ttu-id="cebb8-156">Az a **Neota logika Studio tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cebb8-156">On the **Neota Logic Studio Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_url.png)

    <span data-ttu-id="cebb8-158">a.</span><span class="sxs-lookup"><span data-stu-id="cebb8-158">a.</span></span> <span data-ttu-id="cebb8-159">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub domain>.neotalogic.com/a/<sub application>`</span><span class="sxs-lookup"><span data-stu-id="cebb8-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/a/<sub application>`</span></span>

    <span data-ttu-id="cebb8-160">b.</span><span class="sxs-lookup"><span data-stu-id="cebb8-160">b.</span></span> <span data-ttu-id="cebb8-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<sub domain>.neotalogic.com/wb`</span><span class="sxs-lookup"><span data-stu-id="cebb8-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<sub domain>.neotalogic.com/wb`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="cebb8-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="cebb8-162">These values are not the real.</span></span> <span data-ttu-id="cebb8-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím-és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="cebb8-163">Update these values with the actual Sign-On URL & Identifier.</span></span> <span data-ttu-id="cebb8-164">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="cebb8-164">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="cebb8-165">Ügyfél [Neota logika Studio ügyfél-támogatási csoport](https://www.neotalogic.com/contact-us/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="cebb8-165">Contact [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to get these values.</span></span> 
 
4. <span data-ttu-id="cebb8-166">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cebb8-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_certificate.png) 

5. <span data-ttu-id="cebb8-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cebb8-170">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [Neota logika Studio támogatási](https://www.neotalogic.com/contact-us/) vonja össze, és adja meg a letöltött **metaadatainak XML-kódja** fájlt.</span><span class="sxs-lookup"><span data-stu-id="cebb8-170">To get SSO configured for your application, Contact [Neota Logic Studio support](https://www.neotalogic.com/contact-us/) team and provide them with downloaded **Metadata XML** file.</span></span>

> [!TIP]
> <span data-ttu-id="cebb8-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="cebb8-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="cebb8-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="cebb8-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="cebb8-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="cebb8-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cebb8-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cebb8-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="cebb8-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="cebb8-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cebb8-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="cebb8-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="cebb8-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cebb8-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-180">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cebb8-182">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="cebb8-182">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cebb8-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="cebb8-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-neotalogicstudio-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cebb8-186">a.</span><span class="sxs-lookup"><span data-stu-id="cebb8-186">a.</span></span> <span data-ttu-id="cebb8-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cebb8-188">b.</span><span class="sxs-lookup"><span data-stu-id="cebb8-188">b.</span></span> <span data-ttu-id="cebb8-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cebb8-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cebb8-190">c.</span><span class="sxs-lookup"><span data-stu-id="cebb8-190">c.</span></span> <span data-ttu-id="cebb8-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="cebb8-192">d.</span><span class="sxs-lookup"><span data-stu-id="cebb8-192">d.</span></span> <span data-ttu-id="cebb8-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-193">Click **Create**.</span></span>
 
### <a name="creating-a-neota-logic-studio-test-user"></a><span data-ttu-id="cebb8-194">Neota logika Studio tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cebb8-194">Creating a Neota Logic Studio test user</span></span>

<span data-ttu-id="cebb8-195">Ebben a szakaszban egy Neota logika Studio Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="cebb8-195">In this section, you create a user called Britta Simon in Neota Logic Studio.</span></span> <span data-ttu-id="cebb8-196">a [Neota logika Studio ügyfél-támogatási csoport](https://www.neotalogic.com/contact-us/) a felhasználók hozzáadása a Neota logika Studio platform.</span><span class="sxs-lookup"><span data-stu-id="cebb8-196">work with [Neota Logic Studio Client support team](https://www.neotalogic.com/contact-us/) to add the users in the Neota Logic Studio platform.</span></span> <span data-ttu-id="cebb8-197">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="cebb8-197">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="cebb8-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cebb8-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="cebb8-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Neota logika Studio Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="cebb8-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Neota Logic Studio.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cebb8-201">**Britta Simon hozzárendelése Neota logika Studio, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="cebb8-201">**To assign Britta Simon to Neota Logic Studio, perform the following steps:**</span></span>

1. <span data-ttu-id="cebb8-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cebb8-204">Az alkalmazások listában válassza ki a **Neota logika Studio**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-204">In the applications list, select **Neota Logic Studio**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_neotalogicstudio_app.png) 

3. <span data-ttu-id="cebb8-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cebb8-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cebb8-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cebb8-208">Click **Add** button.</span></span> <span data-ttu-id="cebb8-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebb8-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cebb8-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cebb8-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="cebb8-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebb8-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cebb8-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cebb8-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cebb8-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cebb8-214">Testing single sign-on</span></span>

<span data-ttu-id="cebb8-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cebb8-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="cebb8-216">Kattintson a hozzáférési panelen Neota logika Studio csempére, a szervezete bejelentkezési oldalára irányítja.</span><span class="sxs-lookup"><span data-stu-id="cebb8-216">Click the Neota Logic Studio tile in the Access Panel, you will be redirected to Organization sign-on page.</span></span> <span data-ttu-id="cebb8-217">Sikeres bejelentkezés után meg fog kell bejelentkezett a Neota logika Studio alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="cebb8-217">After successful login, you will be signed-on to your Neota Logic Studio application.</span></span> <span data-ttu-id="cebb8-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="cebb8-218">For more information about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

<span data-ttu-id="cebb8-219">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="cebb8-219">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="cebb8-220">További források</span><span class="sxs-lookup"><span data-stu-id="cebb8-220">Additional resources</span></span>

* [<span data-ttu-id="cebb8-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="cebb8-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cebb8-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cebb8-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-neotalogicstudio-tutorial/tutorial_general_203.png

