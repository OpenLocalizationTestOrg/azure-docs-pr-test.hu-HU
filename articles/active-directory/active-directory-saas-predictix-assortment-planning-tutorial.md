---
title: "Oktatóanyag: Azure Active Directoryval integrált Predictix szortiment megtervezése |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Predictix szortiment tervezési között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 37e686ff-f8e5-40b1-9d7e-f64b076917b7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: cc7ad4aa5260276e26406b6b79c039372e5ee69f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-assortment-planning"></a><span data-ttu-id="35aab-103">Oktatóanyag: Azure Active Directoryval integrált Predictix szortiment tervezése</span><span class="sxs-lookup"><span data-stu-id="35aab-103">Tutorial: Azure Active Directory integration with Predictix Assortment Planning</span></span>

<span data-ttu-id="35aab-104">Ebben az oktatóanyagban elsajátíthatja Predictix szortiment tervezési integrálása Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="35aab-104">In this tutorial, you learn how to integrate Predictix Assortment Planning with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="35aab-105">Predictix szortiment tervezési integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="35aab-105">Integrating Predictix Assortment Planning with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="35aab-106">Az Azure AD, aki hozzáfér a Predictix szortiment tervezési szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="35aab-106">You can control in Azure AD who has access to Predictix Assortment Planning.</span></span>
- <span data-ttu-id="35aab-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Predictix szortiment tervezési (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="35aab-107">You can enable your users to automatically get signed-on to Predictix Assortment Planning (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="35aab-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="35aab-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="35aab-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="35aab-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="35aab-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="35aab-110">Prerequisites</span></span>

<span data-ttu-id="35aab-111">Konfigurálása az Azure AD-integrációs Predictix szortiment tervezési, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="35aab-111">To configure Azure AD integration with Predictix Assortment Planning, you need the following items:</span></span>

- <span data-ttu-id="35aab-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="35aab-112">An Azure AD subscription</span></span>
- <span data-ttu-id="35aab-113">Egy Predictix szortiment tervezési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="35aab-113">A Predictix Assortment Planning single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="35aab-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="35aab-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="35aab-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="35aab-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="35aab-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="35aab-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="35aab-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="35aab-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="35aab-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="35aab-118">Scenario description</span></span>
<span data-ttu-id="35aab-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="35aab-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="35aab-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="35aab-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="35aab-121">Predictix szortiment tervezési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="35aab-121">Adding Predictix Assortment Planning from the gallery</span></span>
2. <span data-ttu-id="35aab-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="35aab-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-assortment-planning-from-the-gallery"></a><span data-ttu-id="35aab-123">Predictix szortiment tervezési hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="35aab-123">Adding Predictix Assortment Planning from the gallery</span></span>
<span data-ttu-id="35aab-124">Az Azure AD integrálása a Predictix szortiment tervezési konfigurálásához kell hozzáadnia Predictix szortiment tervezési a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="35aab-124">To configure the integration of Predictix Assortment Planning into Azure AD, you need to add Predictix Assortment Planning from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="35aab-125">**Adja hozzá a Predictix szortiment tervezési a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="35aab-125">**To add Predictix Assortment Planning from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="35aab-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="35aab-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="35aab-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="35aab-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="35aab-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="35aab-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="35aab-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="35aab-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="35aab-133">Írja be a keresőmezőbe, **Predictix szortiment tervezési**, jelölje be **Predictix szortiment tervezési** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="35aab-133">In the search box, type **Predictix Assortment Planning**, select **Predictix Assortment Planning** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában tervezési szortiment Predictix](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="35aab-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="35aab-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="35aab-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés Predictix szortiment tervezésének "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="35aab-136">In this section, you configure and test Azure AD single sign-on with Predictix Assortment Planning based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="35aab-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Predictix szortiment tervezési a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="35aab-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Predictix Assortment Planning is to a user in Azure AD.</span></span> <span data-ttu-id="35aab-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Predictix szortiment tervezési közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="35aab-138">In other words, a link relationship between an Azure AD user and the related user in Predictix Assortment Planning needs to be established.</span></span>

<span data-ttu-id="35aab-139">A Predictix szortiment tervezéséhez, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="35aab-139">In Predictix Assortment Planning, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="35aab-140">Az Azure AD az egyszeri bejelentkezés Predictix szortiment tervezésének tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="35aab-140">To configure and test Azure AD single sign-on with Predictix Assortment Planning, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="35aab-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="35aab-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="35aab-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="35aab-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="35aab-143">**[Predictix szortiment tervezési tesztfelhasználó létrehozása](#create-a-predictix-assortment-planning-test-user)**  - való egy megfelelője a Britta Simon Predictix szortiment tervezési, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="35aab-143">**[Create a Predictix Assortment Planning test user](#create-a-predictix-assortment-planning-test-user)** - to have a counterpart of Britta Simon in Predictix Assortment Planning that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="35aab-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="35aab-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="35aab-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="35aab-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="35aab-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="35aab-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="35aab-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Predictix szortiment tervezési alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="35aab-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Predictix Assortment Planning application.</span></span>

<span data-ttu-id="35aab-148">**Az Azure AD az egyszeri bejelentkezés konfigurálása Predictix szortiment tervezési, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="35aab-148">**To configure Azure AD single sign-on with Predictix Assortment Planning, perform the following steps:**</span></span>

1. <span data-ttu-id="35aab-149">Az Azure portálon a a **Predictix szortiment tervezési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="35aab-149">In the Azure portal, on the **Predictix Assortment Planning** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="35aab-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="35aab-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_samlbase.png)

3. <span data-ttu-id="35aab-153">Az a **Predictix szortiment tervezési tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="35aab-153">On the **Predictix Assortment Planning Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Predictix szortiment tervezési tartomány és az URL-címek](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_url.png)

    <span data-ttu-id="35aab-155">a.</span><span class="sxs-lookup"><span data-stu-id="35aab-155">a.</span></span> <span data-ttu-id="35aab-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="35aab-156">In the **Sign-on URL** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com/sso/request`|
    | `https://<sub-domain>.dev.ap.predictix.com/`|

    <span data-ttu-id="35aab-157">b.</span><span class="sxs-lookup"><span data-stu-id="35aab-157">b.</span></span> <span data-ttu-id="35aab-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:</span><span class="sxs-lookup"><span data-stu-id="35aab-158">In the **Identifier** textbox, type a URL using the following pattern:</span></span>
    | |
    |--|--|
    | `https://<sub-domain>.ap.predictix.com`|
    | `https://<sub-domain>.dev.ap.predictix.com`|
    
    > [!NOTE] 
    > <span data-ttu-id="35aab-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="35aab-159">These values are not real.</span></span> <span data-ttu-id="35aab-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="35aab-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="35aab-161">Ügyfél [Predictix szortiment tervezési ügyfél-támogatási csoport](http://www.infor.com/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="35aab-161">Contact [Predictix Assortment Planning Client support team](http://www.infor.com/support) to get these values.</span></span> 
 


4. <span data-ttu-id="35aab-162">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="35aab-162">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_certificate.png) 

5. <span data-ttu-id="35aab-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="35aab-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="35aab-166">A a **Predictix szortiment tervezési konfigurációs** kattintson **konfigurálása Predictix szortiment tervezési** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="35aab-166">On the **Predictix Assortment Planning Configuration** section, click **Configure Predictix Assortment Planning** to open **Configure sign-on** window.</span></span> <span data-ttu-id="35aab-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="35aab-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Predictix szortiment tervezési konfiguráció](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_configure.png) 

7. <span data-ttu-id="35aab-169">Egyszeri bejelentkezés konfigurálása **Predictix szortiment tervezési** oldalon kell küldeniük a letöltött **Certificate(Base64)**, **SAML Entitásazonosító**, **SAML Az egyszeri bejelentkezési URL-címe**, és **Sign-Out URL-cím** való [Predictix szortiment tervezési támogatási csoport](http://www.infor.com/support).</span><span class="sxs-lookup"><span data-stu-id="35aab-169">To configure single sign-on on **Predictix Assortment Planning** side, you need to send the downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL**  to [Predictix Assortment Planning support team](http://www.infor.com/support).</span></span> <span data-ttu-id="35aab-170">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="35aab-170">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="35aab-171">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="35aab-171">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="35aab-172">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="35aab-172">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="35aab-173">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="35aab-173">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="35aab-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="35aab-174">Create an Azure AD test user</span></span>

<span data-ttu-id="35aab-175">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="35aab-175">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="35aab-177">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="35aab-177">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="35aab-178">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="35aab-178">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="35aab-180">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="35aab-180">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="35aab-182">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="35aab-182">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="35aab-184">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="35aab-184">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-predictix-assortment-planning-tutorial/create_aaduser_04.png)

    <span data-ttu-id="35aab-186">a.</span><span class="sxs-lookup"><span data-stu-id="35aab-186">a.</span></span> <span data-ttu-id="35aab-187">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="35aab-187">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="35aab-188">b.</span><span class="sxs-lookup"><span data-stu-id="35aab-188">b.</span></span> <span data-ttu-id="35aab-189">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="35aab-189">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="35aab-190">c.</span><span class="sxs-lookup"><span data-stu-id="35aab-190">c.</span></span> <span data-ttu-id="35aab-191">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="35aab-191">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="35aab-192">d.</span><span class="sxs-lookup"><span data-stu-id="35aab-192">d.</span></span> <span data-ttu-id="35aab-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="35aab-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-assortment-planning-test-user"></a><span data-ttu-id="35aab-194">Predictix szortiment tervezési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="35aab-194">Create a Predictix Assortment Planning test user</span></span>

<span data-ttu-id="35aab-195">Ebben a szakaszban egy felhasználó Britta Simon meghívta Predictix szortiment tervezési hoz létre.</span><span class="sxs-lookup"><span data-stu-id="35aab-195">In this section, you create a user called Britta Simon in Predictix Assortment Planning.</span></span> <span data-ttu-id="35aab-196">Adjon együttműködve [Predictix szortiment tervezési támogatási csoport](http://www.infor.com/contact/) a felhasználók hozzáadása a Predictix szortiment tervezési platform.</span><span class="sxs-lookup"><span data-stu-id="35aab-196">Please work with [Predictix Assortment Planning support team](http://www.infor.com/contact/) to add the users in the Predictix Assortment Planning platform.</span></span>
 > [!NOTE]
 > <span data-ttu-id="35aab-197">Az Azure Active Directory fióktulajdonos kap egy e-mailt, és a következő hivatkozás a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="35aab-197">The Azure Active Directory account holder receives an email and follows a link to confirm their account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="35aab-198">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="35aab-198">Assign the Azure AD test user</span></span>

<span data-ttu-id="35aab-199">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Predictix szortiment tervezési Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="35aab-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Predictix Assortment Planning.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="35aab-201">**Britta Simon hozzárendelése Predictix szortiment tervezési, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="35aab-201">**To assign Britta Simon to Predictix Assortment Planning, perform the following steps:**</span></span>

1. <span data-ttu-id="35aab-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="35aab-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="35aab-204">Az alkalmazások listában válassza ki a **Predictix szortiment tervezési**.</span><span class="sxs-lookup"><span data-stu-id="35aab-204">In the applications list, select **Predictix Assortment Planning**.</span></span>

    ![Az alkalmazások listáját a Predictix szortiment tervezési hivatkozás](./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_predictixassortmentplanning_app.png)  

3. <span data-ttu-id="35aab-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="35aab-206">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="35aab-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="35aab-208">Click **Add** button.</span></span> <span data-ttu-id="35aab-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="35aab-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="35aab-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="35aab-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="35aab-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="35aab-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="35aab-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="35aab-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="35aab-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="35aab-214">Test single sign-on</span></span>

<span data-ttu-id="35aab-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="35aab-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="35aab-216">Ha a hozzáférési panelen Predictix szortiment tervezési csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Predictix szortiment tervezési alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="35aab-216">When you click the Predictix Assortment Planning tile in the Access Panel, you should get automatically signed-on to your Predictix Assortment Planning application.</span></span>
<span data-ttu-id="35aab-217">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="35aab-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="35aab-218">További források</span><span class="sxs-lookup"><span data-stu-id="35aab-218">Additional resources</span></span>

* [<span data-ttu-id="35aab-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="35aab-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="35aab-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="35aab-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictix-assortment-planning-tutorial/tutorial_general_203.png

