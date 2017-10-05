---
title: "Oktatóanyag: Azure Active Directoryval integrált 23 videó |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a videó 23 között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 5e73dd1d-3995-4a73-b9cf-1b2318d49cb3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/26/2017
ms.author: jeedes
ms.openlocfilehash: ffcd665506c21e25c84367af5b6a3afb8e319af7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-23-video"></a><span data-ttu-id="de137-103">Oktatóanyag: Azure Active Directoryval integrált 23 videó</span><span class="sxs-lookup"><span data-stu-id="de137-103">Tutorial: Azure Active Directory integration with 23 Video</span></span>

<span data-ttu-id="de137-104">Ebben az oktatóanyagban elsajátíthatja 23 videó integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="de137-104">In this tutorial, you learn how to integrate 23 Video with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="de137-105">23 integrálása videó az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="de137-105">Integrating 23 Video with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="de137-106">Szabályozhatja az Azure AD, aki hozzáfér 23 videó</span><span class="sxs-lookup"><span data-stu-id="de137-106">You can control in Azure AD who has access to 23 Video</span></span>
- <span data-ttu-id="de137-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett 23 Video (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="de137-107">You can enable your users to automatically get signed-on to 23 Video (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="de137-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="de137-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="de137-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="de137-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="de137-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="de137-110">Prerequisites</span></span>

<span data-ttu-id="de137-111">23 videó az Azure AD-integráció konfigurálásához a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="de137-111">To configure Azure AD integration with 23 Video, you need the following items:</span></span>

- <span data-ttu-id="de137-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="de137-112">An Azure AD subscription</span></span>
- <span data-ttu-id="de137-113">A 23 videó egyszeri bejelentkezést előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="de137-113">A 23 Video single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="de137-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="de137-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="de137-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="de137-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="de137-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="de137-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="de137-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="de137-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="de137-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="de137-118">Scenario description</span></span>
<span data-ttu-id="de137-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="de137-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="de137-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="de137-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="de137-121">Hozzáadása 23 videót a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="de137-121">Adding 23 Video from the gallery</span></span>
2. <span data-ttu-id="de137-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de137-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-23-video-from-the-gallery"></a><span data-ttu-id="de137-123">Hozzáadása 23 videót a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="de137-123">Adding 23 Video from the gallery</span></span>
<span data-ttu-id="de137-124">23 videó az Azure AD integrálása konfigurálásához szüksége a gyűjteményből hozzáadása 23 videót a kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="de137-124">To configure the integration of 23 Video into Azure AD, you need to add 23 Video from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="de137-125">**Hozzáadása 23 videót a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de137-125">**To add 23 Video from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="de137-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de137-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="de137-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="de137-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="de137-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de137-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="de137-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="de137-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="de137-133">Írja be a keresőmezőbe, **23 videó**.</span><span class="sxs-lookup"><span data-stu-id="de137-133">In the search box, type **23 Video**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/tutorial_23video_search.png)

5. <span data-ttu-id="de137-135">Az eredmények panelen válassza ki a **23 videó**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="de137-135">In the results panel, select **23 Video**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/tutorial_23video_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="de137-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="de137-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="de137-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján 23 videó</span><span class="sxs-lookup"><span data-stu-id="de137-138">In this section, you configure and test Azure AD single sign-on with 23 Video based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="de137-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó 23 videóban a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="de137-139">For single sign-on to work, Azure AD needs to know what the counterpart user in 23 Video is to a user in Azure AD.</span></span> <span data-ttu-id="de137-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó 23 közötti kapcsolat kapcsolatot videó kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="de137-140">In other words, a link relationship between an Azure AD user and the related user in 23 Video needs to be established.</span></span>

<span data-ttu-id="de137-141">A videó 23, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="de137-141">In 23 Video, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="de137-142">Az Azure AD az egyszeri bejelentkezés 23 videó tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="de137-142">To configure and test Azure AD single sign-on with 23 Video, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="de137-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="de137-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="de137-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="de137-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="de137-145">**[Videó 23 tesztfelhasználó létrehozása](#creating-a-23-video-test-user)**  - való egy megfelelője a Britta Simon 23 videót, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="de137-145">**[Creating a 23 Video test user](#creating-a-23-video-test-user)** - to have a counterpart of Britta Simon in 23 Video that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="de137-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="de137-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="de137-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="de137-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="de137-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="de137-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="de137-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és 23 videó alkalmazásában egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="de137-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your 23 Video application.</span></span>

<span data-ttu-id="de137-150">**23 videó az Azure AD az egyszeri bejelentkezés konfigurálása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de137-150">**To configure Azure AD single sign-on with 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="de137-151">Az Azure portálon a a **23 videó** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="de137-151">In the Azure portal, on the **23 Video** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="de137-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="de137-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_samlbase.png)

3. <span data-ttu-id="de137-155">Az a **23 videó tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de137-155">On the **23 Video Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_url.png)

    <span data-ttu-id="de137-157">a.</span><span class="sxs-lookup"><span data-stu-id="de137-157">a.</span></span> <span data-ttu-id="de137-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.23video.com`</span><span class="sxs-lookup"><span data-stu-id="de137-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.23video.com`</span></span>

    <span data-ttu-id="de137-159">b.</span><span class="sxs-lookup"><span data-stu-id="de137-159">b.</span></span> <span data-ttu-id="de137-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.23video.com/saml/trust/<uniqueid>`</span><span class="sxs-lookup"><span data-stu-id="de137-160">In the **Identifier** textbox, type a URL using the following pattern: `https://www.23video.com/saml/trust/<uniqueid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="de137-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="de137-161">These values are not real.</span></span> <span data-ttu-id="de137-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="de137-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="de137-163">Ügyfél [23 videó ügyfél-támogatási csoport](mailto:support@23company.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="de137-163">Contact [23 Video Client support team](mailto:support@23company.com) to get these values.</span></span> 
 
4. <span data-ttu-id="de137-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="de137-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_certificate.png) 

5. <span data-ttu-id="de137-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="de137-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="de137-168">A a **23 videó konfiguráció** kattintson **23 videó konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="de137-168">On the **23 Video Configuration** section, click **Configure 23 Video** to open **Configure sign-on** window.</span></span> <span data-ttu-id="de137-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="de137-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_configure.png) 

7. <span data-ttu-id="de137-171">Egyszeri bejelentkezés konfigurálása **23 videó** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** való [23 videó támogatási csoport](mailto:support@23company.com).</span><span class="sxs-lookup"><span data-stu-id="de137-171">To configure single sign-on on **23 Video** side, you need to send the downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** to [23 Video support team](mailto:support@23company.com).</span></span> 


> [!TIP]
> <span data-ttu-id="de137-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="de137-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="de137-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="de137-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="de137-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="de137-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="de137-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de137-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="de137-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="de137-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="de137-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de137-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="de137-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="de137-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="de137-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="de137-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="de137-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="de137-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="de137-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="de137-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-23video-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="de137-187">a.</span><span class="sxs-lookup"><span data-stu-id="de137-187">a.</span></span> <span data-ttu-id="de137-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="de137-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="de137-189">b.</span><span class="sxs-lookup"><span data-stu-id="de137-189">b.</span></span> <span data-ttu-id="de137-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="de137-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="de137-191">c.</span><span class="sxs-lookup"><span data-stu-id="de137-191">c.</span></span> <span data-ttu-id="de137-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="de137-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="de137-193">d.</span><span class="sxs-lookup"><span data-stu-id="de137-193">d.</span></span> <span data-ttu-id="de137-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="de137-194">Click **Create**.</span></span>
 
### <a name="creating-a-23-video-test-user"></a><span data-ttu-id="de137-195">Videó 23 tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="de137-195">Creating a 23 Video test user</span></span>

<span data-ttu-id="de137-196">Ez a szakasz célja 23 videó Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="de137-196">The objective of this section is to create a user called Britta Simon in 23 Video.</span></span>

<span data-ttu-id="de137-197">**A felhasználó Britta Simon meghívta 23 videó létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de137-197">**To create a user called Britta Simon in 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="de137-198">Jelentkezzen be rendszergazdaként a 23 videó vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="de137-198">Sign on to your 23 Video company site as administrator.</span></span>

2. <span data-ttu-id="de137-199">Ugrás a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="de137-199">Go to **Settings**.</span></span>
 
3. <span data-ttu-id="de137-200">A **felhasználók** kattintson **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="de137-200">In **Users** section, click **Configure**.</span></span>
   
    ![Felhasználó hozzárendelése][400]

4. <span data-ttu-id="de137-202">Kattintson a **új felhasználó hozzáadásához**.</span><span class="sxs-lookup"><span data-stu-id="de137-202">Click **Add a new user**.</span></span> 
   
    ![Felhasználó hozzárendelése][401]

5. <span data-ttu-id="de137-204">Az a **meghívás a helyhez való csatlakozást** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="de137-204">In the **Invite someone to join this site** section, perform the following steps:</span></span>
   
    ![Felhasználó hozzárendelése][402]

    <span data-ttu-id="de137-206">a.</span><span class="sxs-lookup"><span data-stu-id="de137-206">a.</span></span> <span data-ttu-id="de137-207">Az a **E-mail címek** szövegmező, írja be a Britta Simon e-mail címét az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="de137-207">In the **E-mail addresses** textbox, type Britta Simon's email address in Azure AD.</span></span>  
 
    <span data-ttu-id="de137-208">b.</span><span class="sxs-lookup"><span data-stu-id="de137-208">b.</span></span> <span data-ttu-id="de137-209">Kattintson a **adja hozzá a felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="de137-209">Click **Add the user**.</span></span>   

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="de137-210">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="de137-210">Assigning the Azure AD test user</span></span>

<span data-ttu-id="de137-211">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés 23 videó az Azure egyszeri bejelentkezés használatára.</span><span class="sxs-lookup"><span data-stu-id="de137-211">In this section, you enable Britta Simon to use Azure single sign-on by granting access to 23 Video.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="de137-213">**Britta Simon hozzárendelése 23 videó, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="de137-213">**To assign Britta Simon to 23 Video, perform the following steps:**</span></span>

1. <span data-ttu-id="de137-214">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="de137-214">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="de137-216">Az alkalmazások listában válassza ki a **23 videó**.</span><span class="sxs-lookup"><span data-stu-id="de137-216">In the applications list, select **23 Video**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-23video-tutorial/tutorial_23video_app.png) 

3. <span data-ttu-id="de137-218">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="de137-218">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="de137-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="de137-220">Click **Add** button.</span></span> <span data-ttu-id="de137-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de137-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="de137-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="de137-223">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="de137-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de137-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="de137-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="de137-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="de137-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="de137-226">Testing single sign-on</span></span>

<span data-ttu-id="de137-227">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="de137-227">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="de137-228">Ha a hozzáférési panelen 23 videó csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az 23 videó alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="de137-228">When you click the 23 Video tile in the Access Panel, you should get automatically signed-on to your 23 Video application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="de137-229">További források</span><span class="sxs-lookup"><span data-stu-id="de137-229">Additional resources</span></span>

* [<span data-ttu-id="de137-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="de137-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="de137-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="de137-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-23video-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-23video-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-23video-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-23video-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-23video-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-23video-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-23video-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-23video-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-23video-tutorial/tutorial_general_203.png

[400]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_10.png
[401]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_11.png
[402]: ./media/active-directory-saas-23video-tutorial/tutorial_23video_12.png
