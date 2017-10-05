---
title: "Oktatóanyag: Azure Active Directoryval integrált üvegházhatású |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és üvegházhatású között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 78ec1766-4f79-4f16-9a66-d5584c4b6151
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: d3aba4aab8ded8749db2bf8197f57a6763008c60
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-greenhouse"></a><span data-ttu-id="9ce24-103">Oktatóanyag: Azure Active Directoryval integrált üvegházhatású</span><span class="sxs-lookup"><span data-stu-id="9ce24-103">Tutorial: Azure Active Directory integration with Greenhouse</span></span>

<span data-ttu-id="9ce24-104">Ebben az oktatóanyagban elsajátíthatja üvegházhatású integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9ce24-104">In this tutorial, you learn how to integrate Greenhouse with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9ce24-105">Üvegházhatást integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9ce24-105">Integrating Greenhouse with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9ce24-106">Az Azure AD-hozzáféréssel rendelkező üvegházhatású szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="9ce24-106">You can control in Azure AD who has access to Greenhouse.</span></span>
- <span data-ttu-id="9ce24-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett üvegházhatású (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="9ce24-107">You can enable your users to automatically get signed-on to Greenhouse (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="9ce24-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="9ce24-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="9ce24-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9ce24-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9ce24-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9ce24-110">Prerequisites</span></span>

<span data-ttu-id="9ce24-111">Az Azure AD-integrációs üvegházhatású konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="9ce24-111">To configure Azure AD integration with Greenhouse, you need the following items:</span></span>

- <span data-ttu-id="9ce24-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9ce24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9ce24-113">Egy üvegházhatást egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9ce24-113">A Greenhouse single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9ce24-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9ce24-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9ce24-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9ce24-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9ce24-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9ce24-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9ce24-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9ce24-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9ce24-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9ce24-118">Scenario description</span></span>
<span data-ttu-id="9ce24-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9ce24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9ce24-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9ce24-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9ce24-121">A gyűjteményből üvegházhatású hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9ce24-121">Adding Greenhouse from the gallery</span></span>
2. <span data-ttu-id="9ce24-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9ce24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-greenhouse-from-the-gallery"></a><span data-ttu-id="9ce24-123">A gyűjteményből üvegházhatású hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9ce24-123">Adding Greenhouse from the gallery</span></span>
<span data-ttu-id="9ce24-124">Az Azure AD integrálása a üvegházhatású konfigurálásához kell hozzáadnia üvegházhatású a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9ce24-124">To configure the integration of Greenhouse into Azure AD, you need to add Greenhouse from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9ce24-125">**A gyűjteményből üvegházhatású hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ce24-125">**To add Greenhouse from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9ce24-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="9ce24-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9ce24-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="9ce24-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="9ce24-133">Írja be a keresőmezőbe, **üvegházhatású**, jelölje be **üvegházhatású** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9ce24-133">In the search box, type **Greenhouse**, select **Greenhouse** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában üvegházhatású](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9ce24-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ce24-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="9ce24-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján üvegházhatású.</span><span class="sxs-lookup"><span data-stu-id="9ce24-136">In this section, you configure and test Azure AD single sign-on with Greenhouse based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9ce24-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó üvegházhatású a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9ce24-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Greenhouse is to a user in Azure AD.</span></span> <span data-ttu-id="9ce24-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a üvegházhatású közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ce24-138">In other words, a link relationship between an Azure AD user and the related user in Greenhouse needs to be established.</span></span>

<span data-ttu-id="9ce24-139">Üvegházhatást, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9ce24-139">In Greenhouse, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9ce24-140">Az Azure AD egyszeri bejelentkezést a üvegházhatású tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9ce24-140">To configure and test Azure AD single sign-on with Greenhouse, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9ce24-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9ce24-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9ce24-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9ce24-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9ce24-143">**[Üvegházhatást tesztfelhasználó létrehozása](#create-a-greenhouse-test-user)**  - való egy megfelelője a Britta Simon üvegházhatású, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9ce24-143">**[Create a Greenhouse test user](#create-a-greenhouse-test-user)** - to have a counterpart of Britta Simon in Greenhouse that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9ce24-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ce24-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9ce24-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9ce24-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9ce24-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9ce24-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9ce24-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az üvegházhatású alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9ce24-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Greenhouse application.</span></span>

<span data-ttu-id="9ce24-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés üvegházhatású, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ce24-148">**To configure Azure AD single sign-on with Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="9ce24-149">Az Azure portálon a a **üvegházhatású** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-149">In the Azure portal, on the **Greenhouse** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="9ce24-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9ce24-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_samlbase.png)

3. <span data-ttu-id="9ce24-153">Az a **üvegházhatású tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9ce24-153">On the **Greenhouse Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezési adatokat üvegházhatású tartomány és az URL-címek](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_url.png)

    <span data-ttu-id="9ce24-155">a.</span><span class="sxs-lookup"><span data-stu-id="9ce24-155">a.</span></span> <span data-ttu-id="9ce24-156">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="9ce24-156">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    <span data-ttu-id="9ce24-157">b.</span><span class="sxs-lookup"><span data-stu-id="9ce24-157">b.</span></span> <span data-ttu-id="9ce24-158">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.greenhouse.io`</span><span class="sxs-lookup"><span data-stu-id="9ce24-158">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.greenhouse.io`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9ce24-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="9ce24-159">These values are not real.</span></span> <span data-ttu-id="9ce24-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="9ce24-160">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="9ce24-161">Ügyfél [üvegházhatású ügyfél-támogatási csoport](https://www.greenhouse.io/contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="9ce24-161">Contact [Greenhouse Client support team](https://www.greenhouse.io/contact) to get these values.</span></span> 
 


4. <span data-ttu-id="9ce24-162">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9ce24-162">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_certificate.png) 

5. <span data-ttu-id="9ce24-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-greenhouse-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9ce24-166">Egyszeri bejelentkezés konfigurálása **üvegházhatású** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [üvegházhatású támogatási csoport](http://www.greenhouse.io/contact).</span><span class="sxs-lookup"><span data-stu-id="9ce24-166">To configure single sign-on on **Greenhouse** side, you need to send the downloaded **Metadata XML** to [Greenhouse support team](http://www.greenhouse.io/contact).</span></span>

> [!TIP]
> <span data-ttu-id="9ce24-167">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9ce24-167">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9ce24-168">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9ce24-168">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9ce24-169">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9ce24-169">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9ce24-170">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9ce24-170">Create an Azure AD test user</span></span>

<span data-ttu-id="9ce24-171">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9ce24-171">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="9ce24-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ce24-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9ce24-174">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-174">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="9ce24-176">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-176">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="9ce24-178">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="9ce24-178">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="9ce24-180">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9ce24-180">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-greenhouse-tutorial/create_aaduser_04.png)

    <span data-ttu-id="9ce24-182">a.</span><span class="sxs-lookup"><span data-stu-id="9ce24-182">a.</span></span> <span data-ttu-id="9ce24-183">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-183">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9ce24-184">b.</span><span class="sxs-lookup"><span data-stu-id="9ce24-184">b.</span></span> <span data-ttu-id="9ce24-185">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="9ce24-185">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="9ce24-186">c.</span><span class="sxs-lookup"><span data-stu-id="9ce24-186">c.</span></span> <span data-ttu-id="9ce24-187">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="9ce24-187">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="9ce24-188">d.</span><span class="sxs-lookup"><span data-stu-id="9ce24-188">d.</span></span> <span data-ttu-id="9ce24-189">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-189">Click **Create**.</span></span>
 
### <a name="create-a-greenhouse-test-user"></a><span data-ttu-id="9ce24-190">Üvegházhatást tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ce24-190">Create a Greenhouse test user</span></span>

<span data-ttu-id="9ce24-191">Ahhoz, hogy az Azure AD-felhasználók üvegházhatású bejelentkezni, akkor ki kell építenie üvegházhatású be.</span><span class="sxs-lookup"><span data-stu-id="9ce24-191">In order to enable Azure AD users to log into Greenhouse, they must be provisioned into Greenhouse.</span></span> <span data-ttu-id="9ce24-192">Üvegházhatást, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="9ce24-192">In the case of Greenhouse, provisioning is a manual task.</span></span>

>[!NOTE]
><span data-ttu-id="9ce24-193">Bármely más üvegházhatású felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz üvegházhatású által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="9ce24-193">You can use any other Greenhouse user account creation tools or APIs provided by Greenhouse to provision AAD user accounts.</span></span> 

<span data-ttu-id="9ce24-194">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ce24-194">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="9ce24-195">Jelentkezzen be a **üvegházhatású** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9ce24-195">Log in to your **Greenhouse** company site as an administrator.</span></span>

2. <span data-ttu-id="9ce24-196">Kattintson a felső menüben **konfigurálása**, és kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-196">In the menu on the top, click **Configure**, and then click **Users**.</span></span>
   
   <span data-ttu-id="9ce24-197">![Felhasználók](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="9ce24-197">![Users](./media/active-directory-saas-greenhouse-tutorial/ic790791.png "Users")</span></span>

3. <span data-ttu-id="9ce24-198">Kattintson a **új felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-198">Click **New Users**.</span></span>
   
   <span data-ttu-id="9ce24-199">![Új felhasználó](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="9ce24-199">![New User](./media/active-directory-saas-greenhouse-tutorial/ic790792.png "New User")</span></span>

4. <span data-ttu-id="9ce24-200">Az a **új felhasználó hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9ce24-200">In the **Add New User** section, perform the following steps:</span></span>
   
   <span data-ttu-id="9ce24-201">![Új felhasználó hozzáadása](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "új felhasználó hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="9ce24-201">![Add New User](./media/active-directory-saas-greenhouse-tutorial/ic790793.png "Add New User")</span></span>

   <span data-ttu-id="9ce24-202">a.</span><span class="sxs-lookup"><span data-stu-id="9ce24-202">a.</span></span> <span data-ttu-id="9ce24-203">Az a **adja meg a felhasználói e-mailek** szövegmező, írja be egy érvényes Azure Active Directory-fiókot rendelkezés kívánt e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="9ce24-203">In the **Enter user emails** textbox, type the email address of a valid Azure Active Directory account you want to provision.</span></span>

   <span data-ttu-id="9ce24-204">b.</span><span class="sxs-lookup"><span data-stu-id="9ce24-204">b.</span></span> <span data-ttu-id="9ce24-205">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-205">Click **Save**.</span></span>    
   
      >[!NOTE]
      ><span data-ttu-id="9ce24-206">Az Azure Active Directory fiókhoz tulajdonosainak egy hivatkozással erősítse meg a fiókot, mielőtt aktívvá válik e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="9ce24-206">The Azure Active Directory account holders will receive an email including a link to confirm the account before it becomes active.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9ce24-207">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9ce24-207">Assign the Azure AD test user</span></span>

<span data-ttu-id="9ce24-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés üvegházhatású Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9ce24-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Greenhouse.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="9ce24-210">**Britta Simon hozzárendelése üvegházhatású, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9ce24-210">**To assign Britta Simon to Greenhouse, perform the following steps:**</span></span>

1. <span data-ttu-id="9ce24-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9ce24-213">Az alkalmazások listában válassza ki a **üvegházhatású**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-213">In the applications list, select **Greenhouse**.</span></span>

    ![Az alkalmazások listáját a üvegházhatású hivatkozás](./media/active-directory-saas-greenhouse-tutorial/tutorial_greenhouse_app.png)  

3. <span data-ttu-id="9ce24-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9ce24-215">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="9ce24-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9ce24-217">Click **Add** button.</span></span> <span data-ttu-id="9ce24-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ce24-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="9ce24-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9ce24-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9ce24-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ce24-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9ce24-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9ce24-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9ce24-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9ce24-223">Test single sign-on</span></span>

<span data-ttu-id="9ce24-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9ce24-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9ce24-225">Ha a hozzáférési panelen üvegházhatású csempére kattint, meg kell beolvasása automatikusan bejelentkezett az üvegházhatású alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9ce24-225">When you click the Greenhouse tile in the Access Panel, you should get automatically signed-on to your Greenhouse application.</span></span>
<span data-ttu-id="9ce24-226">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9ce24-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9ce24-227">További források</span><span class="sxs-lookup"><span data-stu-id="9ce24-227">Additional resources</span></span>

* [<span data-ttu-id="9ce24-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9ce24-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9ce24-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9ce24-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-greenhouse-tutorial/tutorial_general_203.png

