---
title: "Oktatóanyag: Azure Active Directoryval integrált PostBeyond |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és PostBeyond között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 09992f08-ec50-4472-997f-ccbe719039e8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 80b920dc99619795cbc86e8cb2e3ac2a78a71932
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-postbeyond"></a><span data-ttu-id="d6f02-103">Oktatóanyag: Azure Active Directoryval integrált PostBeyond</span><span class="sxs-lookup"><span data-stu-id="d6f02-103">Tutorial: Azure Active Directory integration with PostBeyond</span></span>

<span data-ttu-id="d6f02-104">Ebben az oktatóanyagban elsajátíthatja PostBeyond integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d6f02-104">In this tutorial, you learn how to integrate PostBeyond with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d6f02-105">PostBeyond integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="d6f02-105">Integrating PostBeyond with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="d6f02-106">Megadhatja a PostBeyond hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d6f02-106">You can control in Azure AD who has access to PostBeyond</span></span>
- <span data-ttu-id="d6f02-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett PostBeyond (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d6f02-107">You can enable your users to automatically get signed-on to PostBeyond (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d6f02-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d6f02-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="d6f02-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d6f02-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6f02-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d6f02-110">Prerequisites</span></span>

<span data-ttu-id="d6f02-111">Konfigurálása az Azure AD-integrációs PostBeyond, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="d6f02-111">To configure Azure AD integration with PostBeyond, you need the following items:</span></span>

- <span data-ttu-id="d6f02-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d6f02-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d6f02-113">Egy PostBeyond egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d6f02-113">A PostBeyond single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d6f02-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="d6f02-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d6f02-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="d6f02-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d6f02-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d6f02-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d6f02-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d6f02-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d6f02-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d6f02-118">Scenario description</span></span>
<span data-ttu-id="d6f02-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d6f02-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d6f02-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d6f02-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d6f02-121">A gyűjteményből PostBeyond hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d6f02-121">Adding PostBeyond from the gallery</span></span>
2. <span data-ttu-id="d6f02-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6f02-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-postbeyond-from-the-gallery"></a><span data-ttu-id="d6f02-123">A gyűjteményből PostBeyond hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d6f02-123">Adding PostBeyond from the gallery</span></span>
<span data-ttu-id="d6f02-124">Az Azure AD-be PostBeyond integrálása konfigurálásához kell hozzáadnia PostBeyond a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="d6f02-124">To configure the integration of PostBeyond into Azure AD, you need to add PostBeyond from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="d6f02-125">**A gyűjteményből PostBeyond hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6f02-125">**To add PostBeyond from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="d6f02-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d6f02-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="d6f02-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d6f02-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d6f02-133">Írja be a keresőmezőbe, **PostBeyond**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-133">In the search box, type **PostBeyond**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_search.png)

5. <span data-ttu-id="d6f02-135">Az eredmények panelen válassza ki a **PostBeyond**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d6f02-135">In the results panel, select **PostBeyond**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d6f02-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d6f02-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d6f02-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PostBeyond.</span><span class="sxs-lookup"><span data-stu-id="d6f02-138">In this section, you configure and test Azure AD single sign-on with PostBeyond based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d6f02-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó PostBeyond a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="d6f02-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PostBeyond is to a user in Azure AD.</span></span> <span data-ttu-id="d6f02-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a PostBeyond közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d6f02-140">In other words, a link relationship between an Azure AD user and the related user in PostBeyond needs to be established.</span></span>

<span data-ttu-id="d6f02-141">PostBeyond, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="d6f02-141">In PostBeyond, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="d6f02-142">Az Azure AD egyszeri bejelentkezést a PostBeyond tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="d6f02-142">To configure and test Azure AD single sign-on with PostBeyond, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="d6f02-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="d6f02-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="d6f02-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="d6f02-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d6f02-145">**[PostBeyond tesztfelhasználó létrehozása](#creating-a-postbeyond-test-user)**  - való Britta Simon valami PostBeyond, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d6f02-145">**[Creating a PostBeyond test user](#creating-a-postbeyond-test-user)** - to have a counterpart of Britta Simon in PostBeyond that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="d6f02-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d6f02-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d6f02-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="d6f02-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d6f02-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d6f02-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d6f02-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az PostBeyond alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d6f02-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PostBeyond application.</span></span>

<span data-ttu-id="d6f02-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés PostBeyond, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6f02-150">**To configure Azure AD single sign-on with PostBeyond, perform the following steps:**</span></span>

1. <span data-ttu-id="d6f02-151">Az Azure portálon a a **PostBeyond** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-151">In the Azure portal, on the **PostBeyond** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d6f02-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d6f02-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_samlbase.png)

3. <span data-ttu-id="d6f02-155">Az a **PostBeyond tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d6f02-155">On the **PostBeyond Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_url.png)

    <span data-ttu-id="d6f02-157">a.</span><span class="sxs-lookup"><span data-stu-id="d6f02-157">a.</span></span> <span data-ttu-id="d6f02-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.postbeyond.com`</span><span class="sxs-lookup"><span data-stu-id="d6f02-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.postbeyond.com`</span></span>

    <span data-ttu-id="d6f02-159">b.</span><span class="sxs-lookup"><span data-stu-id="d6f02-159">b.</span></span> <span data-ttu-id="d6f02-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.postbeyond.com`</span><span class="sxs-lookup"><span data-stu-id="d6f02-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.postbeyond.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d6f02-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d6f02-161">These values are not real.</span></span> <span data-ttu-id="d6f02-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="d6f02-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d6f02-163">Ügyfél [PostBeyond ügyfél-támogatási csoport](mailto:sso@postbeyond.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d6f02-163">Contact [PostBeyond Client support team](mailto:sso@postbeyond.com) to get these values.</span></span> 
 
4. <span data-ttu-id="d6f02-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d6f02-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_certificate.png) 

5. <span data-ttu-id="d6f02-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d6f02-168">A a **PostBeyond konfigurációs** kattintson **konfigurálása PostBeyond** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d6f02-168">On the **PostBeyond Configuration** section, click **Configure PostBeyond** to open **Configure sign-on** window.</span></span> <span data-ttu-id="d6f02-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d6f02-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_configure.png) 

7. <span data-ttu-id="d6f02-171">Egyszeri bejelentkezés konfigurálása **PostBeyond** oldalon kell küldeniük a letöltött **Certificate(Base64)**, **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezési URL-címe** és **Sign-Out URL-cím** való [PostBeyond támogatási csoport](mailto:sso@postbeyond.com).</span><span class="sxs-lookup"><span data-stu-id="d6f02-171">To configure single sign-on on **PostBeyond** side, you need to send the downloaded **Certificate(Base64)**, **SAML Entity ID**, **SAML Single Sign-On Service URL** and **Sign-Out URL** to [PostBeyond support team](mailto:sso@postbeyond.com).</span></span> <span data-ttu-id="d6f02-172">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="d6f02-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="d6f02-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="d6f02-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="d6f02-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="d6f02-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="d6f02-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d6f02-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d6f02-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6f02-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="d6f02-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="d6f02-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d6f02-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6f02-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="d6f02-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d6f02-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d6f02-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="d6f02-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d6f02-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="d6f02-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-postbeyond-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d6f02-188">a.</span><span class="sxs-lookup"><span data-stu-id="d6f02-188">a.</span></span> <span data-ttu-id="d6f02-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d6f02-190">b.</span><span class="sxs-lookup"><span data-stu-id="d6f02-190">b.</span></span> <span data-ttu-id="d6f02-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d6f02-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d6f02-192">c.</span><span class="sxs-lookup"><span data-stu-id="d6f02-192">c.</span></span> <span data-ttu-id="d6f02-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="d6f02-194">d.</span><span class="sxs-lookup"><span data-stu-id="d6f02-194">d.</span></span> <span data-ttu-id="d6f02-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-195">Click **Create**.</span></span>
 
### <a name="creating-a-postbeyond-test-user"></a><span data-ttu-id="d6f02-196">PostBeyond tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d6f02-196">Creating a PostBeyond test user</span></span>

<span data-ttu-id="d6f02-197">Ebben a szakaszban egy PostBeyond Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d6f02-197">In this section, you create a user called Britta Simon in PostBeyond.</span></span> <span data-ttu-id="d6f02-198">Ha nem tudja Britta Simon hozzáadása a PostBeyond, adjon együttműködve [PostBeyond támogatási csoport](mailto:sso@postbeyond.com) adja hozzá a tesztfelhasználó számára, és engedélyezze az egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d6f02-198">If you don't know how to add Britta Simon in PostBeyond, please work with [PostBeyond support team](mailto:sso@postbeyond.com) to add the test user and enable SSO.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="d6f02-199">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d6f02-199">Assigning the Azure AD test user</span></span>

<span data-ttu-id="d6f02-200">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés PostBeyond Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="d6f02-200">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PostBeyond.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d6f02-202">**Britta Simon hozzárendelése PostBeyond, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="d6f02-202">**To assign Britta Simon to PostBeyond, perform the following steps:**</span></span>

1. <span data-ttu-id="d6f02-203">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-203">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d6f02-205">Az alkalmazások listában válassza ki a **PostBeyond**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-205">In the applications list, select **PostBeyond**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-postbeyond-tutorial/tutorial_postbeyond_app.png) 

3. <span data-ttu-id="d6f02-207">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d6f02-207">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d6f02-209">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d6f02-209">Click **Add** button.</span></span> <span data-ttu-id="d6f02-210">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6f02-210">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d6f02-212">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d6f02-212">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="d6f02-213">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6f02-213">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d6f02-214">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d6f02-214">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d6f02-215">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d6f02-215">Testing single sign-on</span></span>

<span data-ttu-id="d6f02-216">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d6f02-216">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="d6f02-217">Ha PostBeyond csempére kattint a hozzáférési panelen a PostBeyond kell eléréséhez jelentkezzen be a lapon.</span><span class="sxs-lookup"><span data-stu-id="d6f02-217">When you click the PostBeyond tile in the Access Panel, you should get to the PostBeyond sign in page.</span></span> <span data-ttu-id="d6f02-218">Kattintson a **jelentkezzen be Office 365**, az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="d6f02-218">Click on **Sign in with Office 365**, enter your Azure AD credentials.</span></span> <span data-ttu-id="d6f02-219">Ezt követően meg kell kell bejelentkeznie PostBeyond.</span><span class="sxs-lookup"><span data-stu-id="d6f02-219">Then, you should be logged in into PostBeyond.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d6f02-220">További források</span><span class="sxs-lookup"><span data-stu-id="d6f02-220">Additional resources</span></span>

* [<span data-ttu-id="d6f02-221">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="d6f02-221">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d6f02-222">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d6f02-222">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-postbeyond-tutorial/tutorial_general_203.png

