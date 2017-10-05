---
title: "Oktatóanyag: Munkahelyi Learning Azure Active Directoryval integrált |} Microsoft Docs"
description: "Ismerje meg, az egyszeri bejelentkezés Azure Active Directory és a tanulási munkahelyi konfigurálása."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1d607174-bea1-4f40-8233-54cabe02c66a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: jeedes
ms.openlocfilehash: 941832740689c583a8e857d706c35f3076fa754f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-learning-at-work"></a><span data-ttu-id="a1641-103">Oktatóanyag: Munkahelyi Learning Azure Active Directory integrációja</span><span class="sxs-lookup"><span data-stu-id="a1641-103">Tutorial: Azure Active Directory integration with Learning at Work</span></span>

<span data-ttu-id="a1641-104">Ebben az oktatóanyagban elsajátíthatja a munkahelyi tanulási integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a1641-104">In this tutorial, you learn how to integrate Learning at Work with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a1641-105">Learning munkahelyi integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a1641-105">Integrating Learning at Work with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a1641-106">Szabályozhatja, aki hozzáférhet a munkahelyi Learning Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a1641-106">You can control in Azure AD who has access to Learning at Work</span></span>
- <span data-ttu-id="a1641-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a tanulási munkahelyi (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a1641-107">You can enable your users to automatically get signed-on to Learning at Work (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a1641-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a1641-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a1641-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a1641-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a1641-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a1641-110">Prerequisites</span></span>

<span data-ttu-id="a1641-111">Az Azure AD rendszerrel történő integráció konfigurálása a munkahelyi tanulási, szüksége van a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="a1641-111">To configure Azure AD integration with Learning at Work, you need the following items:</span></span>

- <span data-ttu-id="a1641-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a1641-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a1641-113">A Learning munkahelyi egyszeri bejelentkezés engedélyezve van az előfizetésben:</span><span class="sxs-lookup"><span data-stu-id="a1641-113">A Learning at Work single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a1641-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a1641-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a1641-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a1641-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a1641-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1641-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a1641-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a1641-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a1641-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a1641-118">Scenario description</span></span>
<span data-ttu-id="a1641-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a1641-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a1641-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a1641-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a1641-121">Learning hozzáadása a munkahelyi hálózatban a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a1641-121">Adding Learning at Work from the gallery</span></span>
2. <span data-ttu-id="a1641-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a1641-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-learning-at-work-from-the-gallery"></a><span data-ttu-id="a1641-123">Learning hozzáadása a munkahelyi hálózatban a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a1641-123">Adding Learning at Work from the gallery</span></span>
<span data-ttu-id="a1641-124">Learning integrálása az Azure AD-be munkahelyi konfigurálásához kell hozzáadnia tanulási munkahelyi a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a1641-124">To configure the integration of Learning at Work into Azure AD, you need to add Learning at Work from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a1641-125">**Adja hozzá a munkahelyi tanulási a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1641-125">**To add Learning at Work from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a1641-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a1641-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a1641-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a1641-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a1641-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a1641-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a1641-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a1641-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a1641-133">Írja be a keresőmezőbe, **munkahelyi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="a1641-133">In the search box, type **Learning at Work**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_search.png)

5. <span data-ttu-id="a1641-135">Az eredmények panelen válassza ki a **munkahelyi tanulási**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a1641-135">In the results panel, select **Learning at Work**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a1641-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a1641-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a1641-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján munkahelyi Learning segítségével</span><span class="sxs-lookup"><span data-stu-id="a1641-138">In this section, you configure and test Azure AD single sign-on with Learning at Work based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a1641-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó a munkahelyi tanulási a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a1641-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Learning at Work is to a user in Azure AD.</span></span> <span data-ttu-id="a1641-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a munkahelyi tanulási közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a1641-140">In other words, a link relationship between an Azure AD user and the related user in Learning at Work needs to be established.</span></span>

<span data-ttu-id="a1641-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** munkahelyi szeretnének.</span><span class="sxs-lookup"><span data-stu-id="a1641-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Learning at Work.</span></span>

<span data-ttu-id="a1641-142">Az Azure AD az egyszeri bejelentkezés a munkahelyi tanulási tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a1641-142">To configure and test Azure AD single sign-on with Learning at Work, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a1641-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a1641-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a1641-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a1641-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a1641-145">**[A tanulás következő munkahelyi tesztfelhasználó létrehozása](#creating-a-learning-at-work-test-user)**  - való Britta Simon valami, amely csatolva van a felhasználó az Azure AD-ábrázolását munkahelyi tanulás.</span><span class="sxs-lookup"><span data-stu-id="a1641-145">**[Creating a Learning at Work test user](#creating-a-learning-at-work-test-user)** - to have a counterpart of Britta Simon in Learning at Work that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a1641-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1641-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a1641-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a1641-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a1641-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a1641-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a1641-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez a tanulás következő munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a1641-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Learning at Work application.</span></span>

<span data-ttu-id="a1641-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés munkahelyi tanulási, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1641-150">**To configure Azure AD single sign-on with Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="a1641-151">Az Azure portálon a a **munkahelyi tanulási** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a1641-151">In the Azure portal, on the **Learning at Work** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a1641-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1641-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_samlbase.png)

3. <span data-ttu-id="a1641-155">Az a **munkahelyi tartomány és az URL-címek tanulási** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a1641-155">On the **Learning at Work Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_url.png)

    <span data-ttu-id="a1641-157">a.</span><span class="sxs-lookup"><span data-stu-id="a1641-157">a.</span></span> <span data-ttu-id="a1641-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span><span class="sxs-lookup"><span data-stu-id="a1641-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/Web/<company code>`</span></span>

    <span data-ttu-id="a1641-159">b.</span><span class="sxs-lookup"><span data-stu-id="a1641-159">b.</span></span> <span data-ttu-id="a1641-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span><span class="sxs-lookup"><span data-stu-id="a1641-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.sabacloud.com/Saba/saml/SSO/alias/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a1641-161">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="a1641-161">These values are not the real.</span></span> <span data-ttu-id="a1641-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a1641-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a1641-163">Ügyfél [munkahelyi ügyfél-támogatási csapatával: tanulási](https://www.learninga-z.com/site/contact/support) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a1641-163">Contact [Learning at Work Client support team](https://www.learninga-z.com/site/contact/support) to get these values.</span></span> 
 
4. <span data-ttu-id="a1641-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a1641-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_certificate.png) 

5. <span data-ttu-id="a1641-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1641-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a1641-168">Az a **munkahelyi konfigurálás tanulási** területen kattintson **konfigurálása tanulási munkahelyi** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a1641-168">On the **Learning at Work Configuration** section, click **Configure Learning at Work** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a1641-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a1641-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_configure.png) 

7. <span data-ttu-id="a1641-171">Egyszeri bejelentkezés konfigurálása **munkahelyi tanulási** oldalon kell küldeniük a letöltött **metaadatainak XML-kódja**, **SAML Entitásazonosító**, **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-cím**, és **Sign-Out URL-cím** való [munkahelyi terméktámogatási tanulási](https://www.learninga-z.com/site/contact/support).</span><span class="sxs-lookup"><span data-stu-id="a1641-171">To configure single sign-on on **Learning at Work** side, you need to send the downloaded **Metadata XML**, **SAML Entity ID**, **SAML Single Sign-On Service URL**, and **Sign-Out URL** to [Learning at Work support](https://www.learninga-z.com/site/contact/support).</span></span>

> [!TIP]
> <span data-ttu-id="a1641-172">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a1641-172">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a1641-173">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a1641-173">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a1641-174">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a1641-174">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a1641-175">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1641-175">Creating an Azure AD test user</span></span>
<span data-ttu-id="a1641-176">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a1641-176">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a1641-178">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1641-178">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a1641-179">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a1641-179">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a1641-181">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a1641-181">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a1641-183">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a1641-183">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a1641-185">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a1641-185">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-learning-at-work-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a1641-187">a.</span><span class="sxs-lookup"><span data-stu-id="a1641-187">a.</span></span> <span data-ttu-id="a1641-188">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a1641-188">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a1641-189">b.</span><span class="sxs-lookup"><span data-stu-id="a1641-189">b.</span></span> <span data-ttu-id="a1641-190">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a1641-190">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a1641-191">c.</span><span class="sxs-lookup"><span data-stu-id="a1641-191">c.</span></span> <span data-ttu-id="a1641-192">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a1641-192">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a1641-193">d.</span><span class="sxs-lookup"><span data-stu-id="a1641-193">d.</span></span> <span data-ttu-id="a1641-194">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a1641-194">Click **Create**.</span></span>
 
### <a name="creating-a-learning-at-work-test-user"></a><span data-ttu-id="a1641-195">A tanulás következő munkahelyi tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a1641-195">Creating a Learning at Work test user</span></span>

<span data-ttu-id="a1641-196">Ebben a szakaszban a felhasználó munkahelyi tanulási Britta Simon meghívta hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a1641-196">In this section, you create a user called Britta Simon in Learning at Work.</span></span> <span data-ttu-id="a1641-197">Együttműködve [munkahelyi terméktámogatási tanulási](https://www.learninga-z.com/site/contact/support) felhasználót is hozzáadhat a munkahelyi platform: tanulási.</span><span class="sxs-lookup"><span data-stu-id="a1641-197">Work with [Learning at Work support](https://www.learninga-z.com/site/contact/support) to add the users in the Learning at Work platform.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a1641-198">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a1641-198">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a1641-199">Ebben a szakaszban Britta Simon hozzáférést biztosít a munkahelyi tanulás által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a1641-199">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Learning at Work.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a1641-201">**Learning munkahelyi Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a1641-201">**To assign Britta Simon to Learning at Work, perform the following steps:**</span></span>

1. <span data-ttu-id="a1641-202">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a1641-202">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a1641-204">Az alkalmazások listában válassza ki a **munkahelyi tanulási**.</span><span class="sxs-lookup"><span data-stu-id="a1641-204">In the applications list, select **Learning at Work**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-learning-at-work-tutorial/tutorial_learningatwork_app.png) 

3. <span data-ttu-id="a1641-206">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a1641-206">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a1641-208">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a1641-208">Click **Add** button.</span></span> <span data-ttu-id="a1641-209">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1641-209">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a1641-211">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a1641-211">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a1641-212">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1641-212">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a1641-213">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a1641-213">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a1641-214">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a1641-214">Testing single sign-on</span></span>

<span data-ttu-id="a1641-215">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a1641-215">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a1641-216">Ha a tanulás munkahelyi csempe a hozzáférési panelen, gombra kattint, akkor kell beolvasása automatikusan aláírt a a tanulás következő munkahelyi alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a1641-216">When you click the Learning at Work tile in the Access Panel, you should get automatically signed-on to your Learning at Work application.</span></span>
<span data-ttu-id="a1641-217">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a1641-217">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a1641-218">További források</span><span class="sxs-lookup"><span data-stu-id="a1641-218">Additional resources</span></span>

* [<span data-ttu-id="a1641-219">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a1641-219">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a1641-220">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a1641-220">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-learning-at-work-tutorial/tutorial_general_203.png

