---
title: "Oktatóanyag: Azure Active Directoryval integrált Nomadic |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Nomadic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 13d02b1c-d98a-40b1-824f-afa45a2deb6a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 1817a1395c2ffa7268abfff268d9d041f7f21a57
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nomadic"></a><span data-ttu-id="2e322-103">Oktatóanyag: Azure Active Directoryval integrált Nomadic</span><span class="sxs-lookup"><span data-stu-id="2e322-103">Tutorial: Azure Active Directory integration with Nomadic</span></span>

<span data-ttu-id="2e322-104">Ebben az oktatóanyagban elsajátíthatja Nomadic integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2e322-104">In this tutorial, you learn how to integrate Nomadic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2e322-105">Nomadic integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2e322-105">Integrating Nomadic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2e322-106">Az Azure AD, aki hozzáfér Nomadic szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="2e322-106">You can control in Azure AD who has access to Nomadic.</span></span>
- <span data-ttu-id="2e322-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Nomadic (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="2e322-107">You can enable your users to automatically get signed-on to Nomadic (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="2e322-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="2e322-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="2e322-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2e322-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2e322-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e322-110">Prerequisites</span></span>

<span data-ttu-id="2e322-111">Konfigurálása az Azure AD-integrációs Nomadic, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="2e322-111">To configure Azure AD integration with Nomadic, you need the following items:</span></span>

- <span data-ttu-id="2e322-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2e322-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2e322-113">Egy Nomadic egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2e322-113">A Nomadic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2e322-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="2e322-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2e322-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="2e322-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2e322-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2e322-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2e322-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [itt egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2e322-117">If you don't have an Azure AD trial environment, you can [get a one-month trial here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2e322-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2e322-118">Scenario description</span></span>
<span data-ttu-id="2e322-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2e322-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2e322-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2e322-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2e322-121">Adja hozzá a Nomadic a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="2e322-121">Add Nomadic from the gallery</span></span>
2. <span data-ttu-id="2e322-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2e322-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-nomadic-from-the-gallery"></a><span data-ttu-id="2e322-123">Adja hozzá a Nomadic a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="2e322-123">Add Nomadic from the gallery</span></span>
<span data-ttu-id="2e322-124">Az Azure AD integrálása a Nomadic konfigurálásához kell hozzáadnia Nomadic a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="2e322-124">To configure the integration of Nomadic into Azure AD, you need to add Nomadic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2e322-125">**A gyűjteményből Nomadic hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2e322-125">**To add Nomadic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2e322-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2e322-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="2e322-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2e322-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2e322-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2e322-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="2e322-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="2e322-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="2e322-133">Írja be a keresőmezőbe, **Nomadic**, jelölje be **Nomadic** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2e322-133">In the search box, type **Nomadic**, select **Nomadic** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Nomadic](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="2e322-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2e322-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="2e322-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Nomadic.</span><span class="sxs-lookup"><span data-stu-id="2e322-136">In this section, you configure and test Azure AD single sign-on with Nomadic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="2e322-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Nomadic a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="2e322-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Nomadic is to a user in Azure AD.</span></span> <span data-ttu-id="2e322-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Nomadic közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2e322-138">In other words, a link relationship between an Azure AD user and the related user in Nomadic needs to be established.</span></span>

<span data-ttu-id="2e322-139">Nomadic, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="2e322-139">In Nomadic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2e322-140">Az Azure AD egyszeri bejelentkezést a Nomadic tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="2e322-140">To configure and test Azure AD single sign-on with Nomadic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2e322-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="2e322-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2e322-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="2e322-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2e322-143">**[Nomadic tesztfelhasználó létrehozása](#create-a-nomadic-test-user)**  - való Britta Simon valami Nomadic, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="2e322-143">**[Create a Nomadic test user](#create-a-nomadic-test-user)** - to have a counterpart of Britta Simon in Nomadic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2e322-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2e322-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2e322-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2e322-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="2e322-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2e322-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="2e322-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Nomadic alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2e322-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nomadic application.</span></span>

<span data-ttu-id="2e322-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Nomadic, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2e322-148">**To configure Azure AD single sign-on with Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="2e322-149">Az Azure portálon a a **Nomadic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2e322-149">In the Azure portal, on the **Nomadic** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="2e322-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2e322-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_samlbase.png)

3. <span data-ttu-id="2e322-153">Az a **Nomadic tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2e322-153">On the **Nomadic Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat nomadic tartomány- és URL-címek](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_url.png)

    <span data-ttu-id="2e322-155">a.</span><span class="sxs-lookup"><span data-stu-id="2e322-155">a.</span></span> <span data-ttu-id="2e322-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.nomadic.fm/signin`</span><span class="sxs-lookup"><span data-stu-id="2e322-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/signin`</span></span>

    <span data-ttu-id="2e322-157">b.</span><span class="sxs-lookup"><span data-stu-id="2e322-157">b.</span></span> <span data-ttu-id="2e322-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-cím: `https://<company name>.nomadic.fm/auth/saml2/sp`,`https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span><span class="sxs-lookup"><span data-stu-id="2e322-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<company name>.nomadic.fm/auth/saml2/sp`, `https://<company name>.staging.nomadic.fm/auth/saml2/sp`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2e322-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2e322-159">These values are not real.</span></span> <span data-ttu-id="2e322-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2e322-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2e322-161">Ügyfél [Nomadic ügyfél-támogatási csoport](mailto:help@nomadic.fm) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2e322-161">Contact [Nomadic Client support team](mailto:help@nomadic.fm) to get these values.</span></span> 
 


4. <span data-ttu-id="2e322-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2e322-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_certificate.png) 

5. <span data-ttu-id="2e322-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e322-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-nomadic-tutorial/tutorial_general_400.png)

6.  <span data-ttu-id="2e322-166">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [Nomadic támogatási csoport](mailto:help@nomadic.fm) és adja meg a letöltött **metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="2e322-166">To get SSO configured for your application, contact [Nomadic support team](mailto:help@nomadic.fm) and provide them with the downloaded **metadata**.</span></span>

> [!TIP]
> <span data-ttu-id="2e322-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="2e322-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2e322-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="2e322-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2e322-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2e322-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="2e322-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="2e322-170">Create an Azure AD test user</span></span>

<span data-ttu-id="2e322-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="2e322-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="2e322-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2e322-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2e322-174">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e322-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-nomadic-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="2e322-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2e322-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-nomadic-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="2e322-178">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2e322-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-nomadic-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="2e322-180">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2e322-180">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-nomadic-tutorial/create_aaduser_04.png)

    <span data-ttu-id="2e322-182">a.</span><span class="sxs-lookup"><span data-stu-id="2e322-182">a.</span></span> <span data-ttu-id="2e322-183">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2e322-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2e322-184">b.</span><span class="sxs-lookup"><span data-stu-id="2e322-184">b.</span></span> <span data-ttu-id="2e322-185">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2e322-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="2e322-186">c.</span><span class="sxs-lookup"><span data-stu-id="2e322-186">c.</span></span> <span data-ttu-id="2e322-187">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2e322-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="2e322-188">d.</span><span class="sxs-lookup"><span data-stu-id="2e322-188">d.</span></span> <span data-ttu-id="2e322-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2e322-189">Click **Create**.</span></span>
 
### <a name="create-a-nomadic-test-user"></a><span data-ttu-id="2e322-190">Nomadic tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e322-190">Create a Nomadic test user</span></span>

<span data-ttu-id="2e322-191">Ebben a szakaszban egy Nomadic Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2e322-191">In this section, you create a user called Britta Simon in Nomadic.</span></span> <span data-ttu-id="2e322-192">Adjon együttműködve [Nomadic támogatási csoport](mailto:help@nomadic.fm) a felhasználók hozzáadása a Nomadic platform.</span><span class="sxs-lookup"><span data-stu-id="2e322-192">Please work with [Nomadic support team](mailto:help@nomadic.fm) to add the users in the Nomadic platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="2e322-193">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="2e322-193">Assign the Azure AD test user</span></span>

<span data-ttu-id="2e322-194">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Nomadic Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="2e322-194">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nomadic.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="2e322-196">**Britta Simon hozzárendelése Nomadic, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2e322-196">**To assign Britta Simon to Nomadic, perform the following steps:**</span></span>

1. <span data-ttu-id="2e322-197">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2e322-197">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2e322-199">Az alkalmazások listában válassza ki a **Nomadic**.</span><span class="sxs-lookup"><span data-stu-id="2e322-199">In the applications list, select **Nomadic**.</span></span>

    ![Az alkalmazások listáját a Nomadic hivatkozásra](./media/active-directory-saas-nomadic-tutorial/tutorial_nomadic_app.png)  

3. <span data-ttu-id="2e322-201">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2e322-201">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="2e322-203">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2e322-203">Click **Add** button.</span></span> <span data-ttu-id="2e322-204">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2e322-204">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="2e322-206">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2e322-206">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2e322-207">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2e322-207">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2e322-208">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2e322-208">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="2e322-209">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2e322-209">Test single sign-on</span></span>

<span data-ttu-id="2e322-210">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2e322-210">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="2e322-211">Ha Nomadic csempére kattint a hozzáférési panelen, meg kell beolvasni automatikusan bejelentkezett az Nomadic alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="2e322-211">When you click the Nomadic tile in the Access Panel, you should get automatically signed-on to your Nomadic application.</span></span>
<span data-ttu-id="2e322-212">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2e322-212">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2e322-213">További források</span><span class="sxs-lookup"><span data-stu-id="2e322-213">Additional resources</span></span>

* [<span data-ttu-id="2e322-214">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="2e322-214">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2e322-215">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2e322-215">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nomadic-tutorial/tutorial_general_203.png

