---
title: "Oktatóanyag: Azure Active Directoryval integrált alkották |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és alkották között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 03760032-3d76-4b47-ab84-241f72fbd561
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/15/2017
ms.author: jeedes
ms.openlocfilehash: edd2f9446515531f1147a8abf99295b618b89b25
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamwork"></a><span data-ttu-id="f5a36-103">Oktatóanyag: Azure Active Directoryval integrált alkották</span><span class="sxs-lookup"><span data-stu-id="f5a36-103">Tutorial: Azure Active Directory integration with Teamwork</span></span>

<span data-ttu-id="f5a36-104">Ebben az oktatóanyagban elsajátíthatja alkották integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f5a36-104">In this tutorial, you learn how to integrate Teamwork with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f5a36-105">Alkották integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="f5a36-105">Integrating Teamwork with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="f5a36-106">Megadhatja a alkották hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f5a36-106">You can control in Azure AD who has access to Teamwork</span></span>
- <span data-ttu-id="f5a36-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett alkották (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f5a36-107">You can enable your users to automatically get signed-on to Teamwork (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f5a36-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="f5a36-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="f5a36-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f5a36-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f5a36-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f5a36-110">Prerequisites</span></span>

<span data-ttu-id="f5a36-111">Az Azure AD-integrációs alkották konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="f5a36-111">To configure Azure AD integration with Teamwork, you need the following items:</span></span>

- <span data-ttu-id="f5a36-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f5a36-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f5a36-113">Egy alkották egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="f5a36-113">A Teamwork single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="f5a36-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="f5a36-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="f5a36-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="f5a36-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f5a36-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f5a36-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="f5a36-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f5a36-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="f5a36-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f5a36-118">Scenario description</span></span>
<span data-ttu-id="f5a36-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f5a36-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f5a36-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f5a36-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f5a36-121">A gyűjteményből alkották hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f5a36-121">Adding Teamwork from the gallery</span></span>
2. <span data-ttu-id="f5a36-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f5a36-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-teamwork-from-the-gallery"></a><span data-ttu-id="f5a36-123">A gyűjteményből alkották hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f5a36-123">Adding Teamwork from the gallery</span></span>
<span data-ttu-id="f5a36-124">Az Azure AD integrálása alkották konfigurálásához kell hozzáadnia alkották a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="f5a36-124">To configure the integration of Teamwork into Azure AD, you need to add Teamwork from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="f5a36-125">**A gyűjteményből alkották hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f5a36-125">**To add Teamwork from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a36-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f5a36-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="f5a36-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f5a36-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="f5a36-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f5a36-133">Írja be a keresőmezőbe, **alkották**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-133">In the search box, type **Teamwork**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_001.png)

5. <span data-ttu-id="f5a36-135">Az eredmények panelen válassza ki a **alkották**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f5a36-135">In the results panel, select **Teamwork**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f5a36-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f5a36-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f5a36-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján alkották.</span><span class="sxs-lookup"><span data-stu-id="f5a36-138">In this section, you configure and test Azure AD single sign-on with Teamwork based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f5a36-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó alkották a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="f5a36-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Teamwork is to a user in Azure AD.</span></span> <span data-ttu-id="f5a36-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a alkották közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="f5a36-140">In other words, a link relationship between an Azure AD user and the related user in Teamwork needs to be established.</span></span>

<span data-ttu-id="f5a36-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** alkották a.</span><span class="sxs-lookup"><span data-stu-id="f5a36-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Teamwork.</span></span>

<span data-ttu-id="f5a36-142">Az Azure AD egyszeri bejelentkezést a alkották tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="f5a36-142">To configure and test Azure AD single sign-on with Teamwork, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="f5a36-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="f5a36-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="f5a36-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="f5a36-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f5a36-145">**[Alkották tesztfelhasználó létrehozása](#creating-a-teamwork-test-user)**  - való egy megfelelője a Britta Simon alkották, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f5a36-145">**[Creating a Teamwork test user](#creating-a-teamwork-test-user)** - to have a counterpart of Britta Simon in Teamwork that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="f5a36-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="f5a36-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f5a36-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="f5a36-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f5a36-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f5a36-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f5a36-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az alkották alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f5a36-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Teamwork application.</span></span>

<span data-ttu-id="f5a36-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés alkották, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f5a36-150">**To configure Azure AD single sign-on with Teamwork, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a36-151">Az Azure felügyeleti portálján a a **alkották** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-151">In the Azure Management portal, on the **Teamwork** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f5a36-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="f5a36-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_01.png)

3. <span data-ttu-id="f5a36-155">A a **alkották tartomány és az URL-címek** ebben a szakaszban a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-cím:`https://<company name>.teamwork.com`</span><span class="sxs-lookup"><span data-stu-id="f5a36-155">On the **Teamwork Domain and URLs** section, in the **Sign On URL** textbox, type a URL using the following pattern: `https://<company name>.teamwork.com`</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_02.png)

    > [!NOTE] 
    > <span data-ttu-id="f5a36-157">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="f5a36-157">Please note that this is not the real value.</span></span> <span data-ttu-id="f5a36-158">Ezt az értéket a tényleges bejelentkezési URL-cím frissíteni kell.</span><span class="sxs-lookup"><span data-stu-id="f5a36-158">You have to update this value with the actual Sign On URL.</span></span> <span data-ttu-id="f5a36-159">Ügyfél [alkották támogatási csoport](mailto:support@teamwork.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="f5a36-159">Contact [Teamwork support team](mailto:support@teamwork.com) to get this value.</span></span> 

4. <span data-ttu-id="f5a36-160">Az a **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-160">On the **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_03.png)   

5. <span data-ttu-id="f5a36-162">A a **új tanúsítvány létrehozása** párbeszédpanel, kattintson a naptár ikonra, és válasszon egy **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-162">On the **Create New Certificate** dialog, click the calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="f5a36-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-163">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="f5a36-165">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-165">On the **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_04.png)

7. <span data-ttu-id="f5a36-167">Az előugró **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-167">On the pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="f5a36-169">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f5a36-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_05.png) 

9. <span data-ttu-id="f5a36-171">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [alkották támogatási csoport](mailto:support@teamwork.com) és adja meg a letöltött **metaadatok**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-171">To get SSO configured for your application, contact [Teamwork support team](mailto:support@teamwork.com) and provide them with the downloaded **metadata**.</span></span>
  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f5a36-172">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5a36-172">Creating an Azure AD test user</span></span>
<span data-ttu-id="f5a36-173">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="f5a36-173">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f5a36-175">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f5a36-175">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a36-176">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-176">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f5a36-178">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f5a36-178">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f5a36-180">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5a36-180">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f5a36-182">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="f5a36-182">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamwork-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f5a36-184">a.</span><span class="sxs-lookup"><span data-stu-id="f5a36-184">a.</span></span> <span data-ttu-id="f5a36-185">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-185">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f5a36-186">b.</span><span class="sxs-lookup"><span data-stu-id="f5a36-186">b.</span></span> <span data-ttu-id="f5a36-187">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f5a36-187">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f5a36-188">c.</span><span class="sxs-lookup"><span data-stu-id="f5a36-188">c.</span></span> <span data-ttu-id="f5a36-189">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-189">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="f5a36-190">d.</span><span class="sxs-lookup"><span data-stu-id="f5a36-190">d.</span></span> <span data-ttu-id="f5a36-191">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-191">Click **Create**.</span></span> 



### <a name="creating-a-teamwork-test-user"></a><span data-ttu-id="f5a36-192">Alkották tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f5a36-192">Creating a Teamwork test user</span></span>

<span data-ttu-id="f5a36-193">Ebben a szakaszban egy alkották Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="f5a36-193">In this section, you create a user called Britta Simon in Teamwork.</span></span> <span data-ttu-id="f5a36-194">Adjon együttműködve [alkották támogatási csoport](mailto:support@teamwork.com) a felhasználók hozzáadása a alkották platform.</span><span class="sxs-lookup"><span data-stu-id="f5a36-194">Please work with [Teamwork support team](mailto:support@teamwork.com) to add the users in the Teamwork platform.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="f5a36-195">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f5a36-195">Assigning the Azure AD test user</span></span>

<span data-ttu-id="f5a36-196">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés alkották Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="f5a36-196">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Teamwork.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f5a36-198">**Britta Simon hozzárendelése alkották, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="f5a36-198">**To assign Britta Simon to Teamwork, perform the following steps:**</span></span>

1. <span data-ttu-id="f5a36-199">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-199">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f5a36-201">Az alkalmazások listában válassza ki a **alkották**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-201">In the applications list, select **Teamwork**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamwork-tutorial/tutorial_teamwork_50.png) 

3. <span data-ttu-id="f5a36-203">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f5a36-203">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f5a36-205">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f5a36-205">Click **Add** button.</span></span> <span data-ttu-id="f5a36-206">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5a36-206">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f5a36-208">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f5a36-208">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="f5a36-209">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5a36-209">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f5a36-210">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f5a36-210">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="f5a36-211">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f5a36-211">Testing single sign-on</span></span>

<span data-ttu-id="f5a36-212">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="f5a36-212">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="f5a36-213">Ha a hozzáférési panelen alkották csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az alkották alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="f5a36-213">When you click the Teamwork tile in the Access Panel, you should get automatically signed-on to your Teamwork application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f5a36-214">További források</span><span class="sxs-lookup"><span data-stu-id="f5a36-214">Additional resources</span></span>

* [<span data-ttu-id="f5a36-215">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="f5a36-215">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f5a36-216">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f5a36-216">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamwork-tutorial/tutorial_general_203.png