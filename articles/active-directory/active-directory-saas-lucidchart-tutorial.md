---
title: "Oktatóanyag: Azure Active Directoryval integrált Lucidchart |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Lucidchart között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1068d364-11f3-43b5-bd6d-26f00ecd5baa
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2017
ms.author: jeedes
ms.openlocfilehash: 2dea669f03c893632c08d30feeb3173efc2d8243
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lucidchart"></a><span data-ttu-id="3ddda-103">Oktatóanyag: Azure Active Directoryval integrált Lucidchart</span><span class="sxs-lookup"><span data-stu-id="3ddda-103">Tutorial: Azure Active Directory integration with Lucidchart</span></span>

<span data-ttu-id="3ddda-104">Ebben az oktatóanyagban elsajátíthatja Lucidchart integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3ddda-104">In this tutorial, you learn how to integrate Lucidchart with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3ddda-105">Lucidchart integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3ddda-105">Integrating Lucidchart with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3ddda-106">Megadhatja a Lucidchart hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="3ddda-106">You can control in Azure AD who has access to Lucidchart</span></span>
- <span data-ttu-id="3ddda-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Lucidchart (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3ddda-107">You can enable your users to automatically get signed-on to Lucidchart (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3ddda-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3ddda-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3ddda-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3ddda-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3ddda-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3ddda-110">Prerequisites</span></span>

<span data-ttu-id="3ddda-111">Konfigurálása az Azure AD-integrációs Lucidchart, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3ddda-111">To configure Azure AD integration with Lucidchart, you need the following items:</span></span>

- <span data-ttu-id="3ddda-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3ddda-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3ddda-113">Egy Lucidchart egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="3ddda-113">A Lucidchart single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3ddda-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3ddda-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3ddda-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3ddda-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3ddda-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3ddda-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3ddda-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3ddda-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3ddda-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3ddda-118">Scenario description</span></span>
<span data-ttu-id="3ddda-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3ddda-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3ddda-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3ddda-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3ddda-121">A gyűjteményből Lucidchart hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3ddda-121">Adding Lucidchart from the gallery</span></span>
2. <span data-ttu-id="3ddda-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3ddda-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lucidchart-from-the-gallery"></a><span data-ttu-id="3ddda-123">A gyűjteményből Lucidchart hozzáadása</span><span class="sxs-lookup"><span data-stu-id="3ddda-123">Adding Lucidchart from the gallery</span></span>
<span data-ttu-id="3ddda-124">Az Azure AD integrálása a Lucidchart konfigurálásához kell hozzáadnia Lucidchart a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3ddda-124">To configure the integration of Lucidchart into Azure AD, you need to add Lucidchart from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3ddda-125">**A gyűjteményből Lucidchart hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3ddda-125">**To add Lucidchart from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3ddda-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3ddda-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3ddda-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3ddda-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3ddda-133">Írja be a keresőmezőbe, **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-133">In the search box, type **Lucidchart**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_search.png)

5. <span data-ttu-id="3ddda-135">Az eredmények panelen válassza ki a **Lucidchart**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3ddda-135">In the results panel, select **Lucidchart**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3ddda-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3ddda-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3ddda-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Lucidchart.</span><span class="sxs-lookup"><span data-stu-id="3ddda-138">In this section, you configure and test Azure AD single sign-on with Lucidchart based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="3ddda-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Lucidchart a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="3ddda-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lucidchart is to a user in Azure AD.</span></span> <span data-ttu-id="3ddda-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Lucidchart közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3ddda-140">In other words, a link relationship between an Azure AD user and the related user in Lucidchart needs to be established.</span></span>

<span data-ttu-id="3ddda-141">Lucidchart, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="3ddda-141">In Lucidchart, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="3ddda-142">Az Azure AD egyszeri bejelentkezést a Lucidchart tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3ddda-142">To configure and test Azure AD single sign-on with Lucidchart, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3ddda-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3ddda-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3ddda-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3ddda-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="3ddda-145">**[Lucidchart tesztfelhasználó létrehozása](#creating-a-lucidchart-test-user)**  - való Britta Simon valami Lucidchart, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="3ddda-145">**[Creating a Lucidchart test user](#creating-a-lucidchart-test-user)** - to have a counterpart of Britta Simon in Lucidchart that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="3ddda-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3ddda-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="3ddda-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3ddda-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3ddda-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3ddda-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3ddda-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Lucidchart alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3ddda-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lucidchart application.</span></span>

<span data-ttu-id="3ddda-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Lucidchart, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3ddda-150">**To configure Azure AD single sign-on with Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="3ddda-151">Az Azure portálon a a **Lucidchart** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-151">In the Azure portal, on the **Lucidchart** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3ddda-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3ddda-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_samlbase.png)

3. <span data-ttu-id="3ddda-155">Az a **Lucidchart tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3ddda-155">On the **Lucidchart Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_url.png)

    <span data-ttu-id="3ddda-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://chart2.office.lucidchart.com/saml/sso/azure`</span><span class="sxs-lookup"><span data-stu-id="3ddda-157">In the **Sign-on URL** textbox, type a URL as: `https://chart2.office.lucidchart.com/saml/sso/azure`</span></span>

4. <span data-ttu-id="3ddda-158">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3ddda-158">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_certificate.png) 

5. <span data-ttu-id="3ddda-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3ddda-162">Egy másik webes böngészőablakban jelentkezzen be a Lucidchart vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3ddda-162">In a different web browser window, log into your Lucidchart company site as an administrator.</span></span>

7. <span data-ttu-id="3ddda-163">Kattintson a felső menüben **Team**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-163">In the menu on the top, click **Team**.</span></span>
   
    <span data-ttu-id="3ddda-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "csapata")</span><span class="sxs-lookup"><span data-stu-id="3ddda-164">![Team](./media/active-directory-saas-lucidchart-tutorial/ic791190.png "Team")</span></span>

8. <span data-ttu-id="3ddda-165">Kattintson a **alkalmazások \> SAML kezelése**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-165">Click **Applications \> Manage SAML**.</span></span>
   
    <span data-ttu-id="3ddda-166">![SAML kezelése](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "SAML kezelése")</span><span class="sxs-lookup"><span data-stu-id="3ddda-166">![Manage SAML](./media/active-directory-saas-lucidchart-tutorial/ic791191.png "Manage SAML")</span></span>

9. <span data-ttu-id="3ddda-167">Az a **SAML-alapú hitelesítési beállítások** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3ddda-167">On the **SAML Authentication Settings** dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="3ddda-168">a.</span><span class="sxs-lookup"><span data-stu-id="3ddda-168">a.</span></span> <span data-ttu-id="3ddda-169">Válassza ki **SAML-alapú hitelesítés engedélyezéséhez**, és kattintson a **nem kötelező**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-169">Select **Enable SAML Authentication**, and then click **Optional**.</span></span>

    <span data-ttu-id="3ddda-170">![SAML-alapú hitelesítési beállítások](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML-alapú hitelesítési beállítások")</span><span class="sxs-lookup"><span data-stu-id="3ddda-170">![SAML Authentication Settings](./media/active-directory-saas-lucidchart-tutorial/ic791192.png "SAML Authentication Settings")</span></span>
 
    <span data-ttu-id="3ddda-171">b.</span><span class="sxs-lookup"><span data-stu-id="3ddda-171">b.</span></span> <span data-ttu-id="3ddda-172">Az a **tartomány** szövegmező, írja be a tartomány, és kattintson **módosítás tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-172">In the **Domain** textbox, type your domain, and then click **Change Certificate**.</span></span>

    <span data-ttu-id="3ddda-173">![Tanúsítvány módosítása](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "tanúsítványának módosítása")</span><span class="sxs-lookup"><span data-stu-id="3ddda-173">![Change Certificate](./media/active-directory-saas-lucidchart-tutorial/ic791193.png "Change Certificate")</span></span>
 
    <span data-ttu-id="3ddda-174">c.</span><span class="sxs-lookup"><span data-stu-id="3ddda-174">c.</span></span> <span data-ttu-id="3ddda-175">Nyissa meg a letöltött metaadatfájl, másolja a tartalmat, és illessze be azt a **metaadatok feltöltése** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3ddda-175">Open your downloaded metadata file, copy the content, and then paste it into the **Upload Metadata** textbox.</span></span>

    <span data-ttu-id="3ddda-176">![Töltse fel a metaadatok](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "metaadatok feltöltése")</span><span class="sxs-lookup"><span data-stu-id="3ddda-176">![Upload Metadata](./media/active-directory-saas-lucidchart-tutorial/ic791194.png "Upload Metadata")</span></span>
 
    <span data-ttu-id="3ddda-177">d.</span><span class="sxs-lookup"><span data-stu-id="3ddda-177">d.</span></span> <span data-ttu-id="3ddda-178">Válassza ki **az új felhasználók automatikus hozzáadása a csoporthoz**, és kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-178">Select **Automatically Add new users to the team**, and then click **Save changes**.</span></span>

    <span data-ttu-id="3ddda-179">![Módosítások mentése](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "módosítások mentése")</span><span class="sxs-lookup"><span data-stu-id="3ddda-179">![Save Changes](./media/active-directory-saas-lucidchart-tutorial/ic791195.png "Save Changes")</span></span>

> [!TIP]
> <span data-ttu-id="3ddda-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3ddda-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3ddda-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3ddda-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3ddda-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3ddda-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3ddda-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ddda-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="3ddda-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3ddda-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3ddda-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3ddda-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3ddda-187">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3ddda-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3ddda-191">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3ddda-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3ddda-193">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3ddda-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lucidchart-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3ddda-195">a.</span><span class="sxs-lookup"><span data-stu-id="3ddda-195">a.</span></span> <span data-ttu-id="3ddda-196">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3ddda-197">b.</span><span class="sxs-lookup"><span data-stu-id="3ddda-197">b.</span></span> <span data-ttu-id="3ddda-198">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3ddda-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3ddda-199">c.</span><span class="sxs-lookup"><span data-stu-id="3ddda-199">c.</span></span> <span data-ttu-id="3ddda-200">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3ddda-201">d.</span><span class="sxs-lookup"><span data-stu-id="3ddda-201">d.</span></span> <span data-ttu-id="3ddda-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-202">Click **Create**.</span></span>
 
### <a name="creating-a-lucidchart-test-user"></a><span data-ttu-id="3ddda-203">Lucidchart tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3ddda-203">Creating a Lucidchart test user</span></span>

<span data-ttu-id="3ddda-204">Nincs művelet elem ahhoz, hogy a felhasználó Lucidchart történő konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="3ddda-204">There is no action item for you to configure user provisioning to Lucidchart.</span></span>  <span data-ttu-id="3ddda-205">Ha egy hozzárendelt felhasználó megpróbál bejelentkezni, a hozzáférési panelen Lucidchart, Lucidchart ellenőrzi, hogy a felhasználó létezik-e.</span><span class="sxs-lookup"><span data-stu-id="3ddda-205">When an assigned user tries to log into Lucidchart using the access panel, Lucidchart checks whether the user exists.</span></span>  

<span data-ttu-id="3ddda-206">Nincs még nincs felhasználói fiók érhető el, ha a Lucidchart automatikusan létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3ddda-206">If there is no user account available yet, it is automatically created by Lucidchart.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3ddda-207">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3ddda-207">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3ddda-208">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Lucidchart Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="3ddda-208">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lucidchart.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3ddda-210">**Britta Simon hozzárendelése Lucidchart, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3ddda-210">**To assign Britta Simon to Lucidchart, perform the following steps:**</span></span>

1. <span data-ttu-id="3ddda-211">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-211">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3ddda-213">Az alkalmazások listában válassza ki a **Lucidchart**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-213">In the applications list, select **Lucidchart**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lucidchart-tutorial/tutorial_lucidchart_app.png) 

3. <span data-ttu-id="3ddda-215">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3ddda-215">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3ddda-217">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3ddda-217">Click **Add** button.</span></span> <span data-ttu-id="3ddda-218">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ddda-218">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3ddda-220">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3ddda-220">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3ddda-221">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ddda-221">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3ddda-222">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3ddda-222">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3ddda-223">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3ddda-223">Testing single sign-on</span></span>

<span data-ttu-id="3ddda-224">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="3ddda-224">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="3ddda-225">Ha a hozzáférési panelen Lucidchart csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Lucidchart alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="3ddda-225">When you click the Lucidchart tile in the Access Panel, you should get automatically signed-on to your Lucidchart application.</span></span>
<span data-ttu-id="3ddda-226">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3ddda-226">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ddda-227">További források</span><span class="sxs-lookup"><span data-stu-id="3ddda-227">Additional resources</span></span>

* [<span data-ttu-id="3ddda-228">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3ddda-228">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3ddda-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3ddda-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lucidchart-tutorial/tutorial_general_203.png

