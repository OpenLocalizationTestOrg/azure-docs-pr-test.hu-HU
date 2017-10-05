---
title: "Oktatóanyag: Azure Active Directoryval integrált Workrite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Workrite között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2a5c2956-a011-4d5c-877b-80679b6587b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 4358c4c621634c17cbbd7fa1c72f12746b8e4a2a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workrite"></a><span data-ttu-id="c8406-103">Oktatóanyag: Azure Active Directoryval integrált Workrite</span><span class="sxs-lookup"><span data-stu-id="c8406-103">Tutorial: Azure Active Directory integration with Workrite</span></span>

<span data-ttu-id="c8406-104">Ebben az oktatóanyagban elsajátíthatja Workrite integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c8406-104">In this tutorial, you learn how to integrate Workrite with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c8406-105">Workrite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c8406-105">Integrating Workrite with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c8406-106">Az Azure AD, aki hozzáfér Workrite szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="c8406-106">You can control in Azure AD who has access to Workrite.</span></span>
- <span data-ttu-id="c8406-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Workrite (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="c8406-107">You can enable your users to automatically get signed-on to Workrite (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="c8406-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="c8406-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="c8406-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c8406-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c8406-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c8406-110">Prerequisites</span></span>

<span data-ttu-id="c8406-111">Konfigurálása az Azure AD-integrációs Workrite, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c8406-111">To configure Azure AD integration with Workrite, you need the following items:</span></span>

- <span data-ttu-id="c8406-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c8406-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c8406-113">Egy Workrite egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="c8406-113">A Workrite single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c8406-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c8406-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c8406-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c8406-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c8406-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c8406-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="c8406-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c8406-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c8406-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c8406-118">Scenario description</span></span>
<span data-ttu-id="c8406-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c8406-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c8406-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c8406-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c8406-121">A gyűjteményből Workrite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8406-121">Adding Workrite from the gallery</span></span>
2. <span data-ttu-id="c8406-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c8406-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workrite-from-the-gallery"></a><span data-ttu-id="c8406-123">A gyűjteményből Workrite hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c8406-123">Adding Workrite from the gallery</span></span>
<span data-ttu-id="c8406-124">Az Azure AD integrálása a Workrite konfigurálásához kell hozzáadnia Workrite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c8406-124">To configure the integration of Workrite into Azure AD, you need to add Workrite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c8406-125">**A gyűjteményből Workrite hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8406-125">**To add Workrite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c8406-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c8406-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="c8406-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c8406-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c8406-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8406-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="c8406-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="c8406-133">Írja be a keresőmezőbe, **Workrite**, jelölje be **Workrite** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c8406-133">In the search box, type **Workrite**, select **Workrite** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Workrite](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="c8406-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8406-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="c8406-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Workrite.</span><span class="sxs-lookup"><span data-stu-id="c8406-136">In this section, you configure and test Azure AD single sign-on with Workrite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c8406-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Workrite a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c8406-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Workrite is to a user in Azure AD.</span></span> <span data-ttu-id="c8406-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Workrite közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c8406-138">In other words, a link relationship between an Azure AD user and the related user in Workrite needs to be established.</span></span>

<span data-ttu-id="c8406-139">Workrite, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="c8406-139">In Workrite, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="c8406-140">Az Azure AD egyszeri bejelentkezést a Workrite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c8406-140">To configure and test Azure AD single sign-on with Workrite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c8406-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c8406-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c8406-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c8406-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c8406-143">**[Workrite tesztfelhasználó létrehozása](#create-a-workrite-test-user)**  - való Britta Simon valami Workrite, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="c8406-143">**[Create a Workrite test user](#create-a-workrite-test-user)** - to have a counterpart of Britta Simon in Workrite that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="c8406-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8406-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c8406-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c8406-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="c8406-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c8406-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="c8406-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Workrite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c8406-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workrite application.</span></span>

<span data-ttu-id="c8406-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Workrite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8406-148">**To configure Azure AD single sign-on with Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c8406-149">Az Azure portálon a a **Workrite** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c8406-149">In the Azure portal, on the **Workrite** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="c8406-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c8406-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_samlbase.png)

3. <span data-ttu-id="c8406-153">Az a **Workrite tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c8406-153">On the **Workrite Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Workrite tartomány és az URL-címek](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_url.png)

    <span data-ttu-id="c8406-155">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="c8406-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://app.workrite.co.uk/securelogin/samlgateway.aspx?id=<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="c8406-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="c8406-156">This value is not real.</span></span> <span data-ttu-id="c8406-157">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c8406-157">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="c8406-158">Ügyfél [Workrite ügyfél-támogatási csoport](mailto:support@workrite.co.uk) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="c8406-158">Contact [Workrite Client support team](mailto:support@workrite.co.uk) to get this value.</span></span>

4. <span data-ttu-id="c8406-159">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c8406-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_certificate.png) 

5. <span data-ttu-id="c8406-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-workrite-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c8406-163">A a **Workrite konfigurációs** kattintson **konfigurálása Workrite** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c8406-163">On the **Workrite Configuration** section, click **Configure Workrite** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c8406-164">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c8406-164">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Workrite konfiguráció](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_configure.png) 

7. <span data-ttu-id="c8406-166">Egyszeri bejelentkezés konfigurálása **Workrite** oldalon kell küldeniük a letöltött **Certificate(Base64), Sign-Out URL-címe, SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** való [Workrite támogatási csoport](mailto:support@workrite.co.uk).</span><span class="sxs-lookup"><span data-stu-id="c8406-166">To configure single sign-on on **Workrite** side, you need to send the downloaded **Certificate(Base64), Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [Workrite support team](mailto:support@workrite.co.uk).</span></span>

> [!TIP]
> <span data-ttu-id="c8406-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="c8406-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="c8406-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="c8406-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="c8406-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="c8406-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="c8406-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c8406-170">Create an Azure AD test user</span></span>

<span data-ttu-id="c8406-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c8406-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="c8406-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8406-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c8406-174">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-workrite-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="c8406-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c8406-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-workrite-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="c8406-178">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c8406-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-workrite-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="c8406-180">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c8406-180">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-workrite-tutorial/create_aaduser_04.png)

    <span data-ttu-id="c8406-182">a.</span><span class="sxs-lookup"><span data-stu-id="c8406-182">a.</span></span> <span data-ttu-id="c8406-183">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c8406-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c8406-184">b.</span><span class="sxs-lookup"><span data-stu-id="c8406-184">b.</span></span> <span data-ttu-id="c8406-185">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="c8406-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="c8406-186">c.</span><span class="sxs-lookup"><span data-stu-id="c8406-186">c.</span></span> <span data-ttu-id="c8406-187">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c8406-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="c8406-188">d.</span><span class="sxs-lookup"><span data-stu-id="c8406-188">d.</span></span> <span data-ttu-id="c8406-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-189">Click **Create**.</span></span>
 
### <a name="create-a-workrite-test-user"></a><span data-ttu-id="c8406-190">Workrite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c8406-190">Create a Workrite test user</span></span>

<span data-ttu-id="c8406-191">Ez a szakasz célja Workrite Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c8406-191">The objective of this section is to create a user called Britta Simon in Workrite.</span></span>

<span data-ttu-id="c8406-192">**A felhasználó Britta Simon meghívta Workrite létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8406-192">**To create a user called Britta Simon in Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c8406-193">Jelentkezzen be rendszergazdaként a workrite vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="c8406-193">Sign on to your workrite company site as administrator.</span></span>

2. <span data-ttu-id="c8406-194">Kattintson a navigációs ablaktáblában **Admin**.</span><span class="sxs-lookup"><span data-stu-id="c8406-194">In the navigation pane, click **Admin**.</span></span>
   
    ![Felügyeleti vezérlő][400]

3. <span data-ttu-id="c8406-196">Ugrás a Gyorshivatkozások, és kattintson **felhasználó létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c8406-196">Go to Quick Links, and then click **Create a User**.</span></span>
   
    ![Hozzon létre felhasználói szakasz][401]

4. <span data-ttu-id="c8406-198">Az a **felhasználó létrehozása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c8406-198">On the **Create User** dialog, perform the following steps:</span></span>
   
    ![Hozzon létre felhasználói Dailog][402]
    
    <span data-ttu-id="c8406-200">a.</span><span class="sxs-lookup"><span data-stu-id="c8406-200">a.</span></span> <span data-ttu-id="c8406-201">Az a **E-mail** szövegmező, a felhasználó e-mail címe típusát, például Brittasimon@contoso.com.</span><span class="sxs-lookup"><span data-stu-id="c8406-201">In the **Email** textbox, type the email address of user like Brittasimon@contoso.com.</span></span>

    <span data-ttu-id="c8406-202">b.</span><span class="sxs-lookup"><span data-stu-id="c8406-202">b.</span></span> <span data-ttu-id="c8406-203">Az a **Keresztnév** szövegmező, írja be a felhasználó Britta például a keresztnév.</span><span class="sxs-lookup"><span data-stu-id="c8406-203">In the **First Name** textbox, type the firstname of user like Britta.</span></span>

    <span data-ttu-id="c8406-204">c.</span><span class="sxs-lookup"><span data-stu-id="c8406-204">c.</span></span> <span data-ttu-id="c8406-205">Az a **vezetékneve** szövegmező, írja be például a Simon felhasználó vezetékneve.</span><span class="sxs-lookup"><span data-stu-id="c8406-205">In the **Surname** textbox, type the surname of user like Simon.</span></span>
    
    <span data-ttu-id="c8406-206">d.</span><span class="sxs-lookup"><span data-stu-id="c8406-206">d.</span></span> <span data-ttu-id="c8406-207">Válassza ki **ügyfél rendszergazda** , **válassza ki a szerepkör**.</span><span class="sxs-lookup"><span data-stu-id="c8406-207">Select **Client Administrator** as **Choose Role**.</span></span>
    
    <span data-ttu-id="c8406-208">e.</span><span class="sxs-lookup"><span data-stu-id="c8406-208">e.</span></span> <span data-ttu-id="c8406-209">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-209">Click **Save**.</span></span>   

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="c8406-210">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="c8406-210">Assign the Azure AD test user</span></span>

<span data-ttu-id="c8406-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Workrite Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c8406-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workrite.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="c8406-213">**Britta Simon hozzárendelése Workrite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c8406-213">**To assign Britta Simon to Workrite, perform the following steps:**</span></span>

1. <span data-ttu-id="c8406-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c8406-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c8406-216">Az alkalmazások listában válassza ki a **Workrite**.</span><span class="sxs-lookup"><span data-stu-id="c8406-216">In the applications list, select **Workrite**.</span></span>

    ![Az alkalmazások listáját a Workrite hivatkozás](./media/active-directory-saas-workrite-tutorial/tutorial_workrite_app.png)  

3. <span data-ttu-id="c8406-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c8406-218">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="c8406-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c8406-220">Click **Add** button.</span></span> <span data-ttu-id="c8406-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8406-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="c8406-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c8406-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c8406-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8406-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c8406-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c8406-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="c8406-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c8406-226">Test single sign-on</span></span>

<span data-ttu-id="c8406-227">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c8406-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="c8406-228">Ha a hozzáférési panelen Workrite csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Workrite alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="c8406-228">When you click the Workrite tile in the Access Panel, you should get automatically signed-on to your Workrite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c8406-229">További források</span><span class="sxs-lookup"><span data-stu-id="c8406-229">Additional resources</span></span>

* [<span data-ttu-id="c8406-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c8406-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c8406-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c8406-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workrite-tutorial/tutorial_general_203.png
[400]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_400.png
[401]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_401.png
[402]: ./media/active-directory-saas-workrite-tutorial/tutorial_workrite_402.png

