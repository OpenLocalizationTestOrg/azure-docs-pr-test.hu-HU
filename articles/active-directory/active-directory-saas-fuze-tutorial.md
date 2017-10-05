---
title: "Oktatóanyag: Azure Active Directoryval integrált Fuze |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Fuze között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9780b4bf-1fd1-48c1-9ceb-f750225ae08a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: jeedes
ms.openlocfilehash: c7f7b095aac6202a7ec5248ee2bbb109615287a7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-fuze"></a><span data-ttu-id="36e6e-103">Oktatóanyag: Azure Active Directoryval integrált Fuze</span><span class="sxs-lookup"><span data-stu-id="36e6e-103">Tutorial: Azure Active Directory integration with Fuze</span></span>

<span data-ttu-id="36e6e-104">Ebben az oktatóanyagban elsajátíthatja Fuze integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="36e6e-104">In this tutorial, you learn how to integrate Fuze with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="36e6e-105">Fuze integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="36e6e-105">Integrating Fuze with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="36e6e-106">Megadhatja a Fuze hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="36e6e-106">You can control in Azure AD who has access to Fuze</span></span>
- <span data-ttu-id="36e6e-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Fuze (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="36e6e-107">You can enable your users to automatically get signed-on to Fuze (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="36e6e-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="36e6e-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="36e6e-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="36e6e-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36e6e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="36e6e-110">Prerequisites</span></span>

<span data-ttu-id="36e6e-111">Konfigurálása az Azure AD-integrációs Fuze, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="36e6e-111">To configure Azure AD integration with Fuze, you need the following items:</span></span>

- <span data-ttu-id="36e6e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="36e6e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="36e6e-113">Egy Fuze egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="36e6e-113">A Fuze single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="36e6e-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="36e6e-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="36e6e-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="36e6e-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="36e6e-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="36e6e-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="36e6e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="36e6e-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="36e6e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="36e6e-118">Scenario description</span></span>
<span data-ttu-id="36e6e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="36e6e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="36e6e-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="36e6e-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="36e6e-121">A gyűjteményből Fuze hozzáadása</span><span class="sxs-lookup"><span data-stu-id="36e6e-121">Adding Fuze from the gallery</span></span>
2. <span data-ttu-id="36e6e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36e6e-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-fuze-from-the-gallery"></a><span data-ttu-id="36e6e-123">A gyűjteményből Fuze hozzáadása</span><span class="sxs-lookup"><span data-stu-id="36e6e-123">Adding Fuze from the gallery</span></span>
<span data-ttu-id="36e6e-124">Az Azure AD integrálása a Fuze konfigurálásához kell hozzáadnia Fuze a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="36e6e-124">To configure the integration of Fuze into Azure AD, you need to add Fuze from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="36e6e-125">**A gyűjteményből Fuze hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36e6e-125">**To add Fuze from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="36e6e-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36e6e-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="36e6e-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="36e6e-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="36e6e-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="36e6e-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="36e6e-133">Írja be a keresőmezőbe, **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-133">In the search box, type **Fuze**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_000.png)

5. <span data-ttu-id="36e6e-135">Az eredmények panelen válassza ki a **Fuze**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="36e6e-135">In the results panel, select **Fuze**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="36e6e-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="36e6e-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="36e6e-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Fuze.</span><span class="sxs-lookup"><span data-stu-id="36e6e-138">In this section, you configure and test Azure AD single sign-on with Fuze based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="36e6e-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Fuze a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="36e6e-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Fuze is to a user in Azure AD.</span></span> <span data-ttu-id="36e6e-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Fuze közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="36e6e-140">In other words, a link relationship between an Azure AD user and the related user in Fuze needs to be established.</span></span>

<span data-ttu-id="36e6e-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Fuze a.</span><span class="sxs-lookup"><span data-stu-id="36e6e-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Fuze.</span></span>

<span data-ttu-id="36e6e-142">Az Azure AD egyszeri bejelentkezést a Fuze tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="36e6e-142">To configure and test Azure AD single sign-on with Fuze, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="36e6e-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="36e6e-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="36e6e-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="36e6e-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="36e6e-145">**[Fuze tesztfelhasználó létrehozása](#creating-a-fuze-test-user)**  - való Britta Simon valami Fuze, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="36e6e-145">**[Creating a Fuze test user](#creating-a-fuze-test-user)** - to have a counterpart of Britta Simon in Fuze that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="36e6e-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="36e6e-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="36e6e-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="36e6e-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="36e6e-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="36e6e-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="36e6e-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Fuze alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="36e6e-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Fuze application.</span></span>

<span data-ttu-id="36e6e-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Fuze, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36e6e-150">**To configure Azure AD single sign-on with Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="36e6e-151">Az Azure felügyeleti portálján a a **Fuze** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-151">In the Azure Management portal, on the **Fuze** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="36e6e-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="36e6e-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_01.png)

3. <span data-ttu-id="36e6e-155">Az a **Fuze tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="36e6e-155">On the **Fuze Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_020.png)
    
    <span data-ttu-id="36e6e-157">Az a **bejelentkezési URL-cím** szövegmező, a bejelentkezés típusát az URL-címet:`https://www.thinkingphones.com/jetspeed/portal/`</span><span class="sxs-lookup"><span data-stu-id="36e6e-157">In the **Sign on URL** textbox, type the Sign-on URL as: `https://www.thinkingphones.com/jetspeed/portal/`</span></span>

4.  <span data-ttu-id="36e6e-158">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="36e6e-158">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_general_400.png)

5. <span data-ttu-id="36e6e-160">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="36e6e-160">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the xml file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_05.png) 

6. <span data-ttu-id="36e6e-162">Egyszeri bejelentkezés konfigurálása **Fuze** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja** való [Fuze támogatási csoport](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="36e6e-162">To configure single sign-on on **Fuze** side, you need to send the downloaded **Metadata XML** to [Fuze support team](https://www.fuze.com/support).</span></span> <span data-ttu-id="36e6e-163">Akkor lesz beállítására kell biztosítani a SAML SSO-kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="36e6e-163">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="36e6e-164">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36e6e-164">Creating an Azure AD test user</span></span>
<span data-ttu-id="36e6e-165">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="36e6e-165">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="36e6e-167">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36e6e-167">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="36e6e-168">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="36e6e-168">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="36e6e-170">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="36e6e-170">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="36e6e-172">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36e6e-172">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="36e6e-174">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="36e6e-174">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-fuze-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="36e6e-176">a.</span><span class="sxs-lookup"><span data-stu-id="36e6e-176">a.</span></span> <span data-ttu-id="36e6e-177">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-177">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="36e6e-178">b.</span><span class="sxs-lookup"><span data-stu-id="36e6e-178">b.</span></span> <span data-ttu-id="36e6e-179">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="36e6e-179">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="36e6e-180">c.</span><span class="sxs-lookup"><span data-stu-id="36e6e-180">c.</span></span> <span data-ttu-id="36e6e-181">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-181">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="36e6e-182">d.</span><span class="sxs-lookup"><span data-stu-id="36e6e-182">d.</span></span> <span data-ttu-id="36e6e-183">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="36e6e-183">Click **Create**.</span></span> 


### <a name="creating-a-fuze-test-user"></a><span data-ttu-id="36e6e-184">Fuze tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="36e6e-184">Creating a Fuze test user</span></span>

<span data-ttu-id="36e6e-185">Fuze alkalmazás teljes csak támogatja idő felhasználói kiépítését, így a felhasználókat a rendszer létrehozza automatikusan, ha azok bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="36e6e-185">Fuze application supports full Just in time user provision, so users will get created automatically when they sign-in.</span></span> <span data-ttu-id="36e6e-186">A további tisztázása, lépjen kapcsolatba az Fuze [támogatja](https://www.fuze.com/support).</span><span class="sxs-lookup"><span data-stu-id="36e6e-186">For any other clarification, please contact Fuze [support](https://www.fuze.com/support).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="36e6e-187">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="36e6e-187">Assigning the Azure AD test user</span></span>

<span data-ttu-id="36e6e-188">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Fuze Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="36e6e-188">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Fuze.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="36e6e-190">**Britta Simon hozzárendelése Fuze, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="36e6e-190">**To assign Britta Simon to Fuze, perform the following steps:**</span></span>

1. <span data-ttu-id="36e6e-191">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-191">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="36e6e-193">Az alkalmazások listában válassza ki a **Fuze**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-193">In the applications list, select **Fuze**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-fuze-tutorial/tutorial_fuze_50.png) 

3. <span data-ttu-id="36e6e-195">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="36e6e-195">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="36e6e-197">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="36e6e-197">Click **Add** button.</span></span> <span data-ttu-id="36e6e-198">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36e6e-198">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="36e6e-200">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="36e6e-200">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="36e6e-201">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36e6e-201">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="36e6e-202">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="36e6e-202">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="testing-single-sign-on"></a><span data-ttu-id="36e6e-203">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="36e6e-203">Testing single sign-on</span></span>

<span data-ttu-id="36e6e-204">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="36e6e-204">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="36e6e-205">Ha a hozzáférési panelen Fuze csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Fuze alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="36e6e-205">When you click the Fuze tile in the Access Panel, you should get automatically signed-on to your Fuze application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="36e6e-206">További források</span><span class="sxs-lookup"><span data-stu-id="36e6e-206">Additional resources</span></span>

* [<span data-ttu-id="36e6e-207">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="36e6e-207">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="36e6e-208">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="36e6e-208">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-fuze-tutorial/tutorial_general_203.png