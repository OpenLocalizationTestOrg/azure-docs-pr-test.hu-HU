---
title: "Oktatóanyag: Azure Active Directoryval integrált dő János Factiva |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és dő János Factiva között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b36e97e8-37a6-4096-a894-530427ee1331
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/11/2017
ms.author: jeedes
ms.openlocfilehash: dab48c24ff25fd68df1ee540bb8f0929e7e81bcb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-dow-jones-factiva"></a><span data-ttu-id="82f37-103">Oktatóanyag: Azure Active Directoryval integrált dő János Factiva</span><span class="sxs-lookup"><span data-stu-id="82f37-103">Tutorial: Azure Active Directory integration with Dow Jones Factiva</span></span>

<span data-ttu-id="82f37-104">Ebben az oktatóanyagban elsajátíthatja dő János Factiva integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="82f37-104">In this tutorial, you learn how to integrate Dow Jones Factiva with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="82f37-105">János Factiva dő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="82f37-105">Integrating Dow Jones Factiva with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="82f37-106">Szabályozhatja, aki hozzáférhet dő János Factiva Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="82f37-106">You can control in Azure AD who has access to Dow Jones Factiva</span></span>
- <span data-ttu-id="82f37-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a dő János Factiva (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="82f37-107">You can enable your users to automatically get signed-on to Dow Jones Factiva (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="82f37-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="82f37-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="82f37-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="82f37-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82f37-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="82f37-110">Prerequisites</span></span>

<span data-ttu-id="82f37-111">Az Azure AD-integrációs dő János Factiva konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="82f37-111">To configure Azure AD integration with Dow Jones Factiva, you need the following items:</span></span>

- <span data-ttu-id="82f37-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="82f37-112">An Azure AD subscription</span></span>
- <span data-ttu-id="82f37-113">Egy dő János Factiva egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="82f37-113">A Dow Jones Factiva single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="82f37-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="82f37-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="82f37-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="82f37-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="82f37-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="82f37-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="82f37-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="82f37-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="82f37-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="82f37-118">Scenario description</span></span>
<span data-ttu-id="82f37-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="82f37-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="82f37-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="82f37-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="82f37-121">János Factiva dő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="82f37-121">Adding Dow Jones Factiva from the gallery</span></span>
2. <span data-ttu-id="82f37-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="82f37-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-dow-jones-factiva-from-the-gallery"></a><span data-ttu-id="82f37-123">János Factiva dő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="82f37-123">Adding Dow Jones Factiva from the gallery</span></span>
<span data-ttu-id="82f37-124">Az Azure AD integrálása a dő János Factiva konfigurálásához kell hozzáadnia dő János Factiva a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="82f37-124">To configure the integration of Dow Jones Factiva into Azure AD, you need to add Dow Jones Factiva from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="82f37-125">**A gyűjteményből dő János Factiva hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82f37-125">**To add Dow Jones Factiva from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="82f37-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="82f37-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="82f37-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="82f37-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="82f37-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="82f37-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="82f37-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="82f37-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="82f37-133">Írja be a keresőmezőbe, **dő János Factiva**.</span><span class="sxs-lookup"><span data-stu-id="82f37-133">In the search box, type **Dow Jones Factiva**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_search.png)

5. <span data-ttu-id="82f37-135">Az eredmények panelen válassza ki a **dő János Factiva**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="82f37-135">In the results panel, select **Dow Jones Factiva**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="82f37-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="82f37-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="82f37-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a dő János Factiva "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="82f37-138">In this section, you configure and test Azure AD single sign-on with Dow Jones Factiva based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="82f37-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó dő János Factiva a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="82f37-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Dow Jones Factiva is to a user in Azure AD.</span></span> <span data-ttu-id="82f37-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a dő János Factiva közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="82f37-140">In other words, a link relationship between an Azure AD user and the related user in Dow Jones Factiva needs to be established.</span></span>

<span data-ttu-id="82f37-141">János Factiva dő, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="82f37-141">In Dow Jones Factiva, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="82f37-142">Az Azure AD egyszeri bejelentkezést a dő János Factiva tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="82f37-142">To configure and test Azure AD single sign-on with Dow Jones Factiva, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="82f37-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="82f37-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="82f37-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="82f37-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="82f37-145">**[János Factiva dő tesztfelhasználó létrehozása](#creating-a-dow-jones-factiva-test-user)**  - való egy megfelelője a Britta Simon dő János Factiva, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="82f37-145">**[Creating a Dow Jones Factiva test user](#creating-a-dow-jones-factiva-test-user)** - to have a counterpart of Britta Simon in Dow Jones Factiva that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="82f37-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="82f37-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="82f37-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="82f37-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="82f37-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="82f37-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="82f37-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az dő János Factiva alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="82f37-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Dow Jones Factiva application.</span></span>

<span data-ttu-id="82f37-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés dő János Factiva, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82f37-150">**To configure Azure AD single sign-on with Dow Jones Factiva, perform the following steps:**</span></span>

1. <span data-ttu-id="82f37-151">Az Azure portálon a a **dő János Factiva** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="82f37-151">In the Azure portal, on the **Dow Jones Factiva** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="82f37-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="82f37-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_samlbase.png)

3. <span data-ttu-id="82f37-155">Az a **dő János Factiva tartomány és az URL-címek** szakaszban, a felhasználó nem rendelkezik, az alkalmazás már előre integrálva van az Azure-ral bármely lépések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="82f37-155">On the **Dow Jones Factiva Domain and URLs** section, the user does not have to perform any steps as the app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_url.png)

4. <span data-ttu-id="82f37-157">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="82f37-157">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_certificate.png) 

5. <span data-ttu-id="82f37-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="82f37-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="82f37-161">Egyszeri bejelentkezés konfigurálása **dő János Factiva** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [dő János Factiva támogatási csoport](https://www.dowjones.com/contact/).</span><span class="sxs-lookup"><span data-stu-id="82f37-161">To configure single sign-on on **Dow Jones Factiva** side, you need to send the downloaded **Metadata XML** to [Dow Jones Factiva support team](https://www.dowjones.com/contact/).</span></span> <span data-ttu-id="82f37-162">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="82f37-162">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="82f37-163">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="82f37-163">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="82f37-164">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="82f37-164">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="82f37-165">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="82f37-165">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="82f37-166">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="82f37-166">Creating an Azure AD test user</span></span>
<span data-ttu-id="82f37-167">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="82f37-167">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="82f37-169">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82f37-169">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="82f37-170">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="82f37-170">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="82f37-172">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="82f37-172">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="82f37-174">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="82f37-174">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="82f37-176">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="82f37-176">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-dowjones-factiva-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="82f37-178">a.</span><span class="sxs-lookup"><span data-stu-id="82f37-178">a.</span></span> <span data-ttu-id="82f37-179">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="82f37-179">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="82f37-180">b.</span><span class="sxs-lookup"><span data-stu-id="82f37-180">b.</span></span> <span data-ttu-id="82f37-181">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="82f37-181">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="82f37-182">c.</span><span class="sxs-lookup"><span data-stu-id="82f37-182">c.</span></span> <span data-ttu-id="82f37-183">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="82f37-183">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="82f37-184">d.</span><span class="sxs-lookup"><span data-stu-id="82f37-184">d.</span></span> <span data-ttu-id="82f37-185">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="82f37-185">Click **Create**.</span></span>
 
### <a name="creating-a-dow-jones-factiva-test-user"></a><span data-ttu-id="82f37-186">János Factiva dő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="82f37-186">Creating a Dow Jones Factiva test user</span></span>

<span data-ttu-id="82f37-187">Ebben a szakaszban egy dő János Factiva Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="82f37-187">In this section, you create a user called Britta Simon in Dow Jones Factiva.</span></span> <span data-ttu-id="82f37-188">Adjon együttműködve dő [János Factiva támogatási csoport](https://www.dowjones.com/contact/) a felhasználók hozzáadása a dő János Factiva platform.</span><span class="sxs-lookup"><span data-stu-id="82f37-188">Please work with Dow [Jones Factiva support team](https://www.dowjones.com/contact/) to add the users in the Dow Jones Factiva platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="82f37-189">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="82f37-189">Assigning the Azure AD test user</span></span>

<span data-ttu-id="82f37-190">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés dő János Factiva Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="82f37-190">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Dow Jones Factiva.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="82f37-192">**Britta Simon hozzárendelése dő János Factiva, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="82f37-192">**To assign Britta Simon to Dow Jones Factiva, perform the following steps:**</span></span>

1. <span data-ttu-id="82f37-193">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="82f37-193">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="82f37-195">Az alkalmazások listában válassza ki a **dő János Factiva**.</span><span class="sxs-lookup"><span data-stu-id="82f37-195">In the applications list, select **Dow Jones Factiva**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_dowjonesfactiva_app.png) 

3. <span data-ttu-id="82f37-197">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="82f37-197">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="82f37-199">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="82f37-199">Click **Add** button.</span></span> <span data-ttu-id="82f37-200">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82f37-200">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="82f37-202">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="82f37-202">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="82f37-203">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82f37-203">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="82f37-204">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="82f37-204">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="82f37-205">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="82f37-205">Testing single sign-on</span></span>

<span data-ttu-id="82f37-206">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="82f37-206">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="82f37-207">Ha a hozzáférési panelen dő János Factiva csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az dő János Factiva alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="82f37-207">When you click the Dow Jones Factiva tile in the Access Panel, you should get automatically signed-on to your Dow Jones Factiva application.</span></span>
<span data-ttu-id="82f37-208">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="82f37-208">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="82f37-209">További források</span><span class="sxs-lookup"><span data-stu-id="82f37-209">Additional resources</span></span>

* [<span data-ttu-id="82f37-210">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="82f37-210">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="82f37-211">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="82f37-211">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-dowjones-factiva-tutorial/tutorial_general_203.png

