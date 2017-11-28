---
title: "Oktatóanyag: Azure Active Directoryval integrált MaxxPoint |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és MaxxPoint között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 15ba026e-96fc-4ae8-b135-0169da810e99
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 8a7481b71df5ca407dbed5da3d3cc26991504c82
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-maxxpoint"></a><span data-ttu-id="ce110-103">Oktatóanyag: Azure Active Directoryval integrált MaxxPoint</span><span class="sxs-lookup"><span data-stu-id="ce110-103">Tutorial: Azure Active Directory integration with MaxxPoint</span></span>

<span data-ttu-id="ce110-104">Ebben az oktatóanyagban elsajátíthatja MaxxPoint integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ce110-104">In this tutorial, you learn how to integrate MaxxPoint with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="ce110-105">MaxxPoint integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="ce110-105">Integrating MaxxPoint with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="ce110-106">Megadhatja a MaxxPoint hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="ce110-106">You can control in Azure AD who has access to MaxxPoint</span></span>
- <span data-ttu-id="ce110-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett MaxxPoint (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="ce110-107">You can enable your users to automatically get signed-on to MaxxPoint (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="ce110-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="ce110-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="ce110-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="ce110-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ce110-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ce110-110">Prerequisites</span></span>

<span data-ttu-id="ce110-111">Konfigurálása az Azure AD-integrációs MaxxPoint, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="ce110-111">To configure Azure AD integration with MaxxPoint, you need the following items:</span></span>

- <span data-ttu-id="ce110-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="ce110-112">An Azure AD subscription</span></span>
- <span data-ttu-id="ce110-113">Egy MaxxPoint egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="ce110-113">A MaxxPoint single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="ce110-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="ce110-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="ce110-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="ce110-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="ce110-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce110-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="ce110-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ce110-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="ce110-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="ce110-118">Scenario description</span></span>
<span data-ttu-id="ce110-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="ce110-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="ce110-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="ce110-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="ce110-121">A gyűjteményből MaxxPoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce110-121">Adding MaxxPoint from the gallery</span></span>
2. <span data-ttu-id="ce110-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ce110-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-maxxpoint-from-the-gallery"></a><span data-ttu-id="ce110-123">A gyűjteményből MaxxPoint hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ce110-123">Adding MaxxPoint from the gallery</span></span>
<span data-ttu-id="ce110-124">Az Azure AD integrálása a MaxxPoint konfigurálásához kell hozzáadnia MaxxPoint a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="ce110-124">To configure the integration of MaxxPoint into Azure AD, you need to add MaxxPoint from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="ce110-125">**A gyűjteményből MaxxPoint hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ce110-125">**To add MaxxPoint from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="ce110-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ce110-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="ce110-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="ce110-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="ce110-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ce110-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="ce110-131">Kattintson a **új alkalmazás** gombra új alkalmazás hozzáadása párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="ce110-131">Click **New application** button on the top of dialog to add new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="ce110-133">Írja be a keresőmezőbe, **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="ce110-133">In the search box, type **MaxxPoint**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_001.png)

5. <span data-ttu-id="ce110-135">Az eredmények panelen válassza ki a **MaxxPoint**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="ce110-135">In the results panel, select **MaxxPoint**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_0001.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="ce110-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="ce110-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="ce110-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján MaxxPoint.</span><span class="sxs-lookup"><span data-stu-id="ce110-138">In this section, you configure and test Azure AD single sign-on with MaxxPoint based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="ce110-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó MaxxPoint a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="ce110-139">For single sign-on to work, Azure AD needs to know what the counterpart user in MaxxPoint is to a user in Azure AD.</span></span> <span data-ttu-id="ce110-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a MaxxPoint közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ce110-140">In other words, a link relationship between an Azure AD user and the related user in MaxxPoint needs to be established.</span></span>

<span data-ttu-id="ce110-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** MaxxPoint a.</span><span class="sxs-lookup"><span data-stu-id="ce110-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in MaxxPoint.</span></span>

<span data-ttu-id="ce110-142">Az Azure AD egyszeri bejelentkezést a MaxxPoint tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="ce110-142">To configure and test Azure AD single sign-on with MaxxPoint, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="ce110-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="ce110-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="ce110-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="ce110-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="ce110-145">**[MaxxPoint tesztfelhasználó létrehozása](#creating-a-maxxpoint-test-user)**  - való Britta Simon valami MaxxPoint, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="ce110-145">**[Creating a MaxxPoint test user](#creating-a-maxxpoint-test-user)** - to have a counterpart of Britta Simon in MaxxPoint that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="ce110-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ce110-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="ce110-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="ce110-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="ce110-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="ce110-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="ce110-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az MaxxPoint alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ce110-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your MaxxPoint application.</span></span>

<span data-ttu-id="ce110-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés MaxxPoint, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ce110-150">**To configure Azure AD single sign-on with MaxxPoint, perform the following steps:**</span></span>

1. <span data-ttu-id="ce110-151">Az Azure portálon a a **MaxxPoint** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="ce110-151">In the Azure portal, on the **MaxxPoint** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="ce110-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ce110-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_300.png)

3. <span data-ttu-id="ce110-155">Az a **MaxxPoint tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre a lépéseket nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="ce110-155">On the **MaxxPoint Domain and URLs** section, If you wish to configure the application in **IDP initiated mode**, no need to perform any steps.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_02.png)
    
4. <span data-ttu-id="ce110-157">Az a **MaxxPoint tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="ce110-157">On the **MaxxPoint Domain and URLs** section, If you wish to configure the application in **SP initiated mode**, perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_03.png)

    <span data-ttu-id="ce110-159">a.</span><span class="sxs-lookup"><span data-stu-id="ce110-159">a.</span></span> <span data-ttu-id="ce110-160">Kattintson a **megjelenítése speciális URL-beállításainak** beállítás</span><span class="sxs-lookup"><span data-stu-id="ce110-160">Click **Show advanced URL settings** option</span></span>

    <span data-ttu-id="ce110-161">b.</span><span class="sxs-lookup"><span data-stu-id="ce110-161">b.</span></span> <span data-ttu-id="ce110-162">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span><span class="sxs-lookup"><span data-stu-id="ce110-162">In the **Sign On URL** textbox, type a URL using the following pattern: `https://maxxpoint.westipc.com/default/sso/login/entity/<customer-id>-azure`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="ce110-163">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="ce110-163">Please note that this is not the real value.</span></span> <span data-ttu-id="ce110-164">Ezt az értéket a tényleges bejelentkezési URL-cím frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="ce110-164">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="ce110-165">MaxxPoint team hívható meg **888-728-0950** lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="ce110-165">Call MaxxPoint team on **888-728-0950** to get this value.</span></span>

5. <span data-ttu-id="ce110-166">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="ce110-166">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_06.png) 

6. <span data-ttu-id="ce110-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="ce110-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="ce110-170">Az alkalmazáshoz konfigurált SSO beszerzéséhez hívja MaxxPoint támogatási csoport **888-728-0950** és azok lesz segítséget nyújt további való adja meg a letöltött **metaadatainak XML-kódja** fájlt.</span><span class="sxs-lookup"><span data-stu-id="ce110-170">To get SSO configured for your application, call MaxxPoint support team on **888-728-0950** and they'll assist you further on how to provide them the downloaded **Metadata XML** file.</span></span> 

> [!TIP]
> <span data-ttu-id="ce110-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="ce110-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="ce110-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="ce110-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="ce110-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="ce110-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="ce110-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce110-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="ce110-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="ce110-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="ce110-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ce110-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="ce110-178">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="ce110-178">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="ce110-180">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ce110-180">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="ce110-182">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ce110-182">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="ce110-184">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="ce110-184">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-maxxpoint-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="ce110-186">a.</span><span class="sxs-lookup"><span data-stu-id="ce110-186">a.</span></span> <span data-ttu-id="ce110-187">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="ce110-187">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="ce110-188">b.</span><span class="sxs-lookup"><span data-stu-id="ce110-188">b.</span></span> <span data-ttu-id="ce110-189">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="ce110-189">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="ce110-190">c.</span><span class="sxs-lookup"><span data-stu-id="ce110-190">c.</span></span> <span data-ttu-id="ce110-191">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="ce110-191">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="ce110-192">d.</span><span class="sxs-lookup"><span data-stu-id="ce110-192">d.</span></span> <span data-ttu-id="ce110-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="ce110-193">Click **Create**.</span></span> 

### <a name="creating-a-maxxpoint-test-user"></a><span data-ttu-id="ce110-194">MaxxPoint tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="ce110-194">Creating a MaxxPoint test user</span></span>

<span data-ttu-id="ce110-195">Ebben a szakaszban egy MaxxPoint Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ce110-195">In this section, you create a user called Britta Simon in MaxxPoint.</span></span> <span data-ttu-id="ce110-196">Hívja a MaxxPoint támogatási csoport **888-728-0950** a felhasználók hozzáadása az MaxxPoint alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="ce110-196">Please call MaxxPoint support team on **888-728-0950** to add the users in the MaxxPoint application.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="ce110-197">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="ce110-197">Assigning the Azure AD test user</span></span>

<span data-ttu-id="ce110-198">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés MaxxPoint Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="ce110-198">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to MaxxPoint.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="ce110-200">**Britta Simon hozzárendelése MaxxPoint, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="ce110-200">**To assign Britta Simon to MaxxPoint, perform the following steps:**</span></span>

1. <span data-ttu-id="ce110-201">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="ce110-201">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="ce110-203">Az alkalmazások listában válassza ki a **MaxxPoint**.</span><span class="sxs-lookup"><span data-stu-id="ce110-203">In the applications list, select **MaxxPoint**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-maxxpoint-tutorial/tutorial_maxxpoint_50.png) 

3. <span data-ttu-id="ce110-205">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="ce110-205">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="ce110-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="ce110-207">Click **Add** button.</span></span> <span data-ttu-id="ce110-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ce110-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="ce110-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="ce110-210">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="ce110-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ce110-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="ce110-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="ce110-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="ce110-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="ce110-213">Testing single sign-on</span></span>

<span data-ttu-id="ce110-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="ce110-214">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="ce110-215">Ha a hozzáférési panelen MaxxPoint csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az MaxxPoint alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="ce110-215">When you click the MaxxPoint tile in the Access Panel, you should get automatically signed-on to your MaxxPoint application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="ce110-216">További források</span><span class="sxs-lookup"><span data-stu-id="ce110-216">Additional resources</span></span>

* [<span data-ttu-id="ce110-217">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="ce110-217">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="ce110-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="ce110-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-maxxpoint-tutorial/tutorial_general_203.png