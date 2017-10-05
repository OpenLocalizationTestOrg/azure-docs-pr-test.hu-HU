---
title: "Oktatóanyag: Azure Active Directoryval integrált BenSelect |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és BenSelect között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffa17478-3ea1-4356-a289-545b5b9a4494
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: f8caa023da05863372b7ef92b47a92168445d741
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benselect"></a><span data-ttu-id="49027-103">Oktatóanyag: Azure Active Directoryval integrált BenSelect</span><span class="sxs-lookup"><span data-stu-id="49027-103">Tutorial: Azure Active Directory integration with BenSelect</span></span>

<span data-ttu-id="49027-104">Ebben az oktatóanyagban elsajátíthatja BenSelect integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="49027-104">In this tutorial, you learn how to integrate BenSelect with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="49027-105">BenSelect integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="49027-105">Integrating BenSelect with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="49027-106">Megadhatja a BenSelect hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="49027-106">You can control in Azure AD who has access to BenSelect</span></span>
- <span data-ttu-id="49027-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett BenSelect (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="49027-107">You can enable your users to automatically get signed-on to BenSelect (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="49027-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="49027-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="49027-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="49027-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="49027-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="49027-110">Prerequisites</span></span>

<span data-ttu-id="49027-111">Konfigurálása az Azure AD-integrációs BenSelect, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="49027-111">To configure Azure AD integration with BenSelect, you need the following items:</span></span>

- <span data-ttu-id="49027-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="49027-112">An Azure AD subscription</span></span>
- <span data-ttu-id="49027-113">Egy BenSelect egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="49027-113">A BenSelect single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="49027-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="49027-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="49027-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="49027-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="49027-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="49027-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="49027-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="49027-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="49027-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="49027-118">Scenario description</span></span>
<span data-ttu-id="49027-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="49027-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="49027-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="49027-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="49027-121">A gyűjteményből BenSelect hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49027-121">Adding BenSelect from the gallery</span></span>
2. <span data-ttu-id="49027-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49027-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-benselect-from-the-gallery"></a><span data-ttu-id="49027-123">A gyűjteményből BenSelect hozzáadása</span><span class="sxs-lookup"><span data-stu-id="49027-123">Adding BenSelect from the gallery</span></span>
<span data-ttu-id="49027-124">Az Azure AD integrálása a BenSelect konfigurálásához kell hozzáadnia BenSelect a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="49027-124">To configure the integration of BenSelect into Azure AD, you need to add BenSelect from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="49027-125">**A gyűjteményből BenSelect hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49027-125">**To add BenSelect from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="49027-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49027-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="49027-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="49027-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="49027-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49027-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="49027-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="49027-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="49027-133">Írja be a keresőmezőbe, **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="49027-133">In the search box, type **BenSelect**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_search.png)

5. <span data-ttu-id="49027-135">Az eredmények panelen válassza ki a **BenSelect**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="49027-135">In the results panel, select **BenSelect**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="49027-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="49027-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="49027-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BenSelect</span><span class="sxs-lookup"><span data-stu-id="49027-138">In this section, you configure and test Azure AD single sign-on with BenSelect based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="49027-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó BenSelect a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="49027-139">For single sign-on to work, Azure AD needs to know what the counterpart user in BenSelect is to a user in Azure AD.</span></span> <span data-ttu-id="49027-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a BenSelect közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="49027-140">In other words, a link relationship between an Azure AD user and the related user in BenSelect needs to be established.</span></span>

<span data-ttu-id="49027-141">BenSelect, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="49027-141">In BenSelect, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="49027-142">Az Azure AD egyszeri bejelentkezést a BenSelect tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="49027-142">To configure and test Azure AD single sign-on with BenSelect, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="49027-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="49027-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="49027-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="49027-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="49027-145">**[BenSelect tesztfelhasználó létrehozása](#creating-a-benselect-test-user)**  - való Britta Simon valami BenSelect, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="49027-145">**[Creating a BenSelect test user](#creating-a-benselect-test-user)** - to have a counterpart of Britta Simon in BenSelect that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="49027-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49027-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="49027-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="49027-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="49027-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="49027-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="49027-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az BenSelect alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="49027-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your BenSelect application.</span></span>

<span data-ttu-id="49027-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés BenSelect, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49027-150">**To configure Azure AD single sign-on with BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="49027-151">Az Azure portálon a a **BenSelect** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="49027-151">In the Azure portal, on the **BenSelect** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="49027-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="49027-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_samlbase.png)

3. <span data-ttu-id="49027-155">Az a **BenSelect tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="49027-155">On the **BenSelect Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_url.png)

    <span data-ttu-id="49027-157">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span><span class="sxs-lookup"><span data-stu-id="49027-157">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.benselect.com/enroll/login.aspx?Path=<tenant name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="49027-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="49027-158">This value is not real.</span></span> <span data-ttu-id="49027-159">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="49027-159">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="49027-160">Ügyfél [BenSelect támogatási csoport](mailto:support@selerix.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="49027-160">Contact [BenSelect support team](mailto:support@selerix.com) to get this value.</span></span>
 
4. <span data-ttu-id="49027-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Raw)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="49027-161">On the **SAML Signing Certificate** section, click **Certificate(Raw)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_certificate.png) 

5. <span data-ttu-id="49027-163">BenSelect alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="49027-163">BenSelect application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="49027-164">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="49027-164">Configure the following claims for this application.</span></span> <span data-ttu-id="49027-165">Ezek az attribútumok értékének kezelheti a **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="49027-165">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="49027-166">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="49027-166">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_06.png)

6. <span data-ttu-id="49027-168">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="49027-168">In the **User Attributes** section on the **Single sign-on** dialog:</span></span>

    <span data-ttu-id="49027-169">a.</span><span class="sxs-lookup"><span data-stu-id="49027-169">a.</span></span> <span data-ttu-id="49027-170">Az a **felhasználói azonosító** legördülő listában válassza ki **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="49027-170">In the **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>

    <span data-ttu-id="49027-171">b.</span><span class="sxs-lookup"><span data-stu-id="49027-171">b.</span></span> <span data-ttu-id="49027-172">Az a **Mail** legördülő listában válassza ki **user.userprincipalname**.</span><span class="sxs-lookup"><span data-stu-id="49027-172">In the **Mail** dropdown list, select **user.userprincipalname**.</span></span>

7. <span data-ttu-id="49027-173">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="49027-173">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="49027-175">A a **BenSelect konfigurációs** kattintson **konfigurálása BenSelect** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="49027-175">On the **BenSelect Configuration** section, click **Configure BenSelect** to open **Configure sign-on** window.</span></span> <span data-ttu-id="49027-176">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="49027-176">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_configure.png) 

9. <span data-ttu-id="49027-178">Egyszeri bejelentkezés konfigurálása **BenSelect** oldalon kell küldeniük a letöltött **Certificate(Raw)** és **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**való [BenSelect támogatási csoport](mailto:support@selerix.com).</span><span class="sxs-lookup"><span data-stu-id="49027-178">To configure single sign-on on **BenSelect** side, you need to send the downloaded **Certificate(Raw)** and **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [BenSelect support team](mailto:support@selerix.com).</span></span>

   >[!NOTE]
   ><span data-ttu-id="49027-179">Említse meg, hogy ezt az integrációt igényel az SHA256 algoritmust, (SHA1 nem támogatott) kell az egyszeri bejelentkezés beállítása app2101 hasonlóan a megfelelő kiszolgálón stb.</span><span class="sxs-lookup"><span data-stu-id="49027-179">You need to mention that this integration requires the SHA256 algorithm (SHA1 is not supported) to set the SSO on the appropriate server like app2101 etc.</span></span> 
   
> [!TIP]
> <span data-ttu-id="49027-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="49027-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="49027-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="49027-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="49027-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="49027-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="49027-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49027-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="49027-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="49027-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="49027-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49027-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="49027-187">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="49027-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="49027-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="49027-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="49027-191">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="49027-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="49027-193">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="49027-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-benselect-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="49027-195">a.</span><span class="sxs-lookup"><span data-stu-id="49027-195">a.</span></span> <span data-ttu-id="49027-196">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="49027-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="49027-197">b.</span><span class="sxs-lookup"><span data-stu-id="49027-197">b.</span></span> <span data-ttu-id="49027-198">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="49027-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="49027-199">c.</span><span class="sxs-lookup"><span data-stu-id="49027-199">c.</span></span> <span data-ttu-id="49027-200">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="49027-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="49027-201">d.</span><span class="sxs-lookup"><span data-stu-id="49027-201">d.</span></span> <span data-ttu-id="49027-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="49027-202">Click **Create**.</span></span>
 
### <a name="creating-a-benselect-test-user"></a><span data-ttu-id="49027-203">BenSelect tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="49027-203">Creating a BenSelect test user</span></span>

<span data-ttu-id="49027-204">Ez a szakasz célja BenSelect Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="49027-204">The objective of this section is to create a user called Britta Simon in BenSelect.</span></span> <span data-ttu-id="49027-205">Együttműködve [BenSelect támogatási csoport](mailto:support@selerix.com) a felhasználók hozzáadása a BenSelect fiók.</span><span class="sxs-lookup"><span data-stu-id="49027-205">Work with [BenSelect support team](mailto:support@selerix.com) to add the users in the BenSelect account.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="49027-206">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="49027-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="49027-207">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés BenSelect Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="49027-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to BenSelect.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="49027-209">**Britta Simon hozzárendelése BenSelect, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="49027-209">**To assign Britta Simon to BenSelect, perform the following steps:**</span></span>

1. <span data-ttu-id="49027-210">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="49027-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="49027-212">Az alkalmazások listában válassza ki a **BenSelect**.</span><span class="sxs-lookup"><span data-stu-id="49027-212">In the applications list, select **BenSelect**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benselect-tutorial/tutorial_benselect_app.png) 

3. <span data-ttu-id="49027-214">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="49027-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="49027-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="49027-216">Click **Add** button.</span></span> <span data-ttu-id="49027-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49027-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="49027-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="49027-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="49027-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49027-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="49027-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="49027-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="49027-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="49027-222">Testing single sign-on</span></span>

<span data-ttu-id="49027-223">Ebben a szakaszban a Azure AD SSO konfigurációját, a hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="49027-223">In this section, you test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="49027-224">Ha a hozzáférési panelen BenSelect csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az BenSelect alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="49027-224">When you click the BenSelect tile in the Access Panel, you should get automatically signed-on to your BenSelect application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49027-225">További források</span><span class="sxs-lookup"><span data-stu-id="49027-225">Additional resources</span></span>

* [<span data-ttu-id="49027-226">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="49027-226">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="49027-227">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="49027-227">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-benselect-tutorial/tutorial_general_203.png

