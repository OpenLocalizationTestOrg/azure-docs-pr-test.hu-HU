---
title: "Oktatóanyag: Azure Active Directoryval integrált Novatus |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Novatus között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2f13779-bdb7-4408-9738-be67ed3de4e5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/02/2017
ms.author: jeedes
ms.openlocfilehash: ec67e96309a8877e6fb65b30da1501e4f34a9ee4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-novatus"></a><span data-ttu-id="8b765-103">Oktatóanyag: Azure Active Directoryval integrált Novatus</span><span class="sxs-lookup"><span data-stu-id="8b765-103">Tutorial: Azure Active Directory integration with Novatus</span></span>

<span data-ttu-id="8b765-104">Ebben az oktatóanyagban elsajátíthatja Novatus integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8b765-104">In this tutorial, you learn how to integrate Novatus with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8b765-105">Novatus integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8b765-105">Integrating Novatus with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8b765-106">Megadhatja a Novatus hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8b765-106">You can control in Azure AD who has access to Novatus</span></span>
- <span data-ttu-id="8b765-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Novatus (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8b765-107">You can enable your users to automatically get signed-on to Novatus (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8b765-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8b765-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8b765-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8b765-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b765-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8b765-110">Prerequisites</span></span>

<span data-ttu-id="8b765-111">Konfigurálása az Azure AD-integrációs Novatus, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8b765-111">To configure Azure AD integration with Novatus, you need the following items:</span></span>

- <span data-ttu-id="8b765-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8b765-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8b765-113">Egy Novatus egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8b765-113">A Novatus single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8b765-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8b765-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8b765-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8b765-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8b765-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8b765-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8b765-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8b765-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8b765-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8b765-118">Scenario description</span></span>
<span data-ttu-id="8b765-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8b765-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8b765-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8b765-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8b765-121">A gyűjteményből Novatus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8b765-121">Adding Novatus from the gallery</span></span>
2. <span data-ttu-id="8b765-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8b765-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-novatus-from-the-gallery"></a><span data-ttu-id="8b765-123">A gyűjteményből Novatus hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8b765-123">Adding Novatus from the gallery</span></span>
<span data-ttu-id="8b765-124">Az Azure AD integrálása a Novatus konfigurálásához kell hozzáadnia Novatus a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="8b765-124">To configure the integration of Novatus into Azure AD, you need to add Novatus from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8b765-125">**A gyűjteményből Novatus hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8b765-125">**To add Novatus from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8b765-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8b765-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8b765-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8b765-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8b765-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8b765-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8b765-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8b765-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8b765-133">Írja be a keresőmezőbe, **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="8b765-133">In the search box, type **Novatus**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_search.png)

5. <span data-ttu-id="8b765-135">Az eredmények panelen válassza ki a **Novatus**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8b765-135">In the results panel, select **Novatus**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8b765-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8b765-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8b765-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Novatus.</span><span class="sxs-lookup"><span data-stu-id="8b765-138">In this section, you configure and test Azure AD single sign-on with Novatus based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8b765-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Novatus a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8b765-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Novatus is to a user in Azure AD.</span></span> <span data-ttu-id="8b765-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Novatus közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8b765-140">In other words, a link relationship between an Azure AD user and the related user in Novatus needs to be established.</span></span>

<span data-ttu-id="8b765-141">Novatus, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8b765-141">In Novatus, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8b765-142">Az Azure AD egyszeri bejelentkezést a Novatus tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8b765-142">To configure and test Azure AD single sign-on with Novatus, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8b765-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8b765-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8b765-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8b765-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8b765-145">**[Novatus tesztfelhasználó létrehozása](#creating-a-novatus-test-user)**  - való Britta Simon valami Novatus, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8b765-145">**[Creating a Novatus test user](#creating-a-novatus-test-user)** - to have a counterpart of Britta Simon in Novatus that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8b765-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8b765-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8b765-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8b765-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8b765-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8b765-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8b765-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Novatus alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8b765-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Novatus application.</span></span>

<span data-ttu-id="8b765-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Novatus, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8b765-150">**To configure Azure AD single sign-on with Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="8b765-151">Az Azure portálon a a **Novatus** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8b765-151">In the Azure portal, on the **Novatus** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8b765-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8b765-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_samlbase.png)

3. <span data-ttu-id="8b765-155">Az a **Novatus tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8b765-155">On the **Novatus Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_url.png)

     <span data-ttu-id="8b765-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://sso.novatuscontracts.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="8b765-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://sso.novatuscontracts.com/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8b765-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="8b765-158">This value is not real.</span></span> <span data-ttu-id="8b765-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="8b765-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="8b765-160">Ügyfél [Novatus ügyfél-támogatási csoport](mailto:jvinci@novatusinc.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8b765-160">Contact [Novatus Client support team](mailto:jvinci@novatusinc.com) to get this value.</span></span> 
 


4. <span data-ttu-id="8b765-161">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8b765-161">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_certificate.png) 

5. <span data-ttu-id="8b765-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b765-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8b765-165">A a **Novatus konfigurációs** kattintson **konfigurálása Novatus** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8b765-165">On the **Novatus Configuration** section, click **Configure Novatus** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8b765-166">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8b765-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_configure.png) 

7. <span data-ttu-id="8b765-168">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba a [Novatus támogatási csoport](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="8b765-168">To get SSO configured for your application, contact your [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> <span data-ttu-id="8b765-169">Csatlakoztassa a **letöltött tanúsítvány** fájl a levelezés és a megosztáshoz a **metadata URL-címek** (**Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**) Novatus csapattal beállítania egyszeri Bejelentkezést az oldalon.</span><span class="sxs-lookup"><span data-stu-id="8b765-169">Attach the **downloaded certificate** file to your mail and share the **metadata urls** (**Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL**) with Novatus team to set up SSO on their side.</span></span>

> [!TIP]
> <span data-ttu-id="8b765-170">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8b765-170">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8b765-171">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8b765-171">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8b765-172">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8b765-172">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8b765-173">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b765-173">Creating an Azure AD test user</span></span>
<span data-ttu-id="8b765-174">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8b765-174">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8b765-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8b765-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8b765-177">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8b765-177">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8b765-179">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8b765-179">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8b765-181">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8b765-181">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8b765-183">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8b765-183">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-novatus-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8b765-185">a.</span><span class="sxs-lookup"><span data-stu-id="8b765-185">a.</span></span> <span data-ttu-id="8b765-186">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8b765-186">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8b765-187">b.</span><span class="sxs-lookup"><span data-stu-id="8b765-187">b.</span></span> <span data-ttu-id="8b765-188">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8b765-188">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8b765-189">c.</span><span class="sxs-lookup"><span data-stu-id="8b765-189">c.</span></span> <span data-ttu-id="8b765-190">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8b765-190">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8b765-191">d.</span><span class="sxs-lookup"><span data-stu-id="8b765-191">d.</span></span> <span data-ttu-id="8b765-192">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8b765-192">Click **Create**.</span></span>
 
### <a name="creating-a-novatus-test-user"></a><span data-ttu-id="8b765-193">Novatus tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b765-193">Creating a Novatus test user</span></span>

<span data-ttu-id="8b765-194">Ez a szakasz célja Novatus Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8b765-194">The objective of this section is to create a user called Britta Simon in Novatus.</span></span> <span data-ttu-id="8b765-195">Novatus támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="8b765-195">Novatus supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="8b765-196">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="8b765-196">There is no action item for you in this section.</span></span> <span data-ttu-id="8b765-197">Új felhasználó jön létre az Novatus elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="8b765-197">A new user will be created during an attempt to access Novatus if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="8b765-198">Ha hozzon létre manuálisan egy felhasználó van szüksége, forduljon a kell a [Novatus támogatási csoport](mailto:jvinci@novatusinc.com).</span><span class="sxs-lookup"><span data-stu-id="8b765-198">If you need to create an user manually, you need to contact the [Novatus support team](mailto:jvinci@novatusinc.com).</span></span> 
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8b765-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8b765-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8b765-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Novatus Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="8b765-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Novatus.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8b765-202">**Britta Simon hozzárendelése Novatus, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8b765-202">**To assign Britta Simon to Novatus, perform the following steps:**</span></span>

1. <span data-ttu-id="8b765-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8b765-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8b765-205">Az alkalmazások listában válassza ki a **Novatus**.</span><span class="sxs-lookup"><span data-stu-id="8b765-205">In the applications list, select **Novatus**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-novatus-tutorial/tutorial_novatus_app.png) 

3. <span data-ttu-id="8b765-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8b765-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8b765-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b765-209">Click **Add** button.</span></span> <span data-ttu-id="8b765-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8b765-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8b765-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8b765-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8b765-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8b765-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8b765-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8b765-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8b765-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8b765-215">Testing single sign-on</span></span>

<span data-ttu-id="8b765-216">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="8b765-216">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8b765-217">Ha a hozzáférési panelen Novatus csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Novatus alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="8b765-217">When you click the Novatus tile in the Access Panel, you should get automatically signed-on to your Novatus application.</span></span> <span data-ttu-id="8b765-218">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8b765-218">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8b765-219">További források</span><span class="sxs-lookup"><span data-stu-id="8b765-219">Additional resources</span></span>

* [<span data-ttu-id="8b765-220">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8b765-220">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8b765-221">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8b765-221">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-novatus-tutorial/tutorial_general_203.png

