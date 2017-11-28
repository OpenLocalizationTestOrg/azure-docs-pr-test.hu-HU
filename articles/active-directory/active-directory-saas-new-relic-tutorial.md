---
title: "Oktatóanyag: Azure Active Directory-integráció a New Relic |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a New Relic között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 3186b9a8-f4d8-45e2-ad82-6275f95e7aa6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: jeedes
ms.openlocfilehash: 605e85c23a849f70fcc0237361d7a891f716ca3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-new-relic"></a><span data-ttu-id="e0fe9-103">Oktatóanyag: A New Relic Azure Active Directory-integráció</span><span class="sxs-lookup"><span data-stu-id="e0fe9-103">Tutorial: Azure Active Directory integration with New Relic</span></span>

<span data-ttu-id="e0fe9-104">Ebben az oktatóanyagban elsajátíthatja a New Relic integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e0fe9-104">In this tutorial, you learn how to integrate New Relic with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="e0fe9-105">New Relic integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-105">Integrating New Relic with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="e0fe9-106">Megadhatja a New Relic hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e0fe9-106">You can control in Azure AD who has access to New Relic</span></span>
- <span data-ttu-id="e0fe9-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a New Relic (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="e0fe9-107">You can enable your users to automatically get signed-on to New Relic (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="e0fe9-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e0fe9-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="e0fe9-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e0fe9-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e0fe9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e0fe9-110">Prerequisites</span></span>

<span data-ttu-id="e0fe9-111">Az Azure AD-integráció konfigurálása a New Relic, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-111">To configure Azure AD integration with New Relic, you need the following items:</span></span>

- <span data-ttu-id="e0fe9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e0fe9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="e0fe9-113">Egy új New Relic egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e0fe9-113">A New Relic single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="e0fe9-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="e0fe9-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="e0fe9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="e0fe9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e0fe9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="e0fe9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e0fe9-118">Scenario description</span></span>
<span data-ttu-id="e0fe9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="e0fe9-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e0fe9-121">New Relic hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e0fe9-121">Adding New Relic from the gallery</span></span>
2. <span data-ttu-id="e0fe9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e0fe9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-new-relic-from-the-gallery"></a><span data-ttu-id="e0fe9-123">New Relic hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e0fe9-123">Adding New Relic from the gallery</span></span>
<span data-ttu-id="e0fe9-124">Az Azure AD integrálása a New Relic konfigurálásához kell hozzáadnia New Relic a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-124">To configure the integration of New Relic into Azure AD, you need to add New Relic from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="e0fe9-125">**Adja hozzá a New Relic a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-125">**To add New Relic from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="e0fe9-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="e0fe9-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="e0fe9-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="e0fe9-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="e0fe9-133">Írja be a keresőmezőbe, **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-133">In the search box, type **New Relic**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_search.png)

5. <span data-ttu-id="e0fe9-135">Az eredmények panelen válassza ki a **New Relic**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-135">In the results panel, select **New Relic**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="e0fe9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="e0fe9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="e0fe9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapuló új New Relic.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-138">In this section, you configure and test Azure AD single sign-on with New Relic based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e0fe9-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a New Relic tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-139">For single sign-on to work, Azure AD needs to know what the counterpart user in New Relic is to a user in Azure AD.</span></span> <span data-ttu-id="e0fe9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a New Relic közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-140">In other words, a link relationship between an Azure AD user and the related user in New Relic needs to be established.</span></span>

<span data-ttu-id="e0fe9-141">A New Relic, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-141">In New Relic, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="e0fe9-142">Az Azure AD egyszeri bejelentkezést a New Relic tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-142">To configure and test Azure AD single sign-on with New Relic, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="e0fe9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="e0fe9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e0fe9-145">**[New Relic tesztfelhasználó létrehozása](#creating-a-new-relic-test-user)**  - való Britta Simon egy megfelelője a New Relic, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-145">**[Creating a New Relic test user](#creating-a-new-relic-test-user)** - to have a counterpart of Britta Simon in New Relic that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="e0fe9-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e0fe9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="e0fe9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e0fe9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="e0fe9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a New Relic-alkalmazás az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your New Relic application.</span></span>

<span data-ttu-id="e0fe9-150">**A New Relic konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-150">**To configure Azure AD single sign-on with New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="e0fe9-151">Az Azure portálon a a **New Relic** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-151">In the Azure portal, on the **New Relic** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="e0fe9-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_samlbase.png)

3. <span data-ttu-id="e0fe9-155">Az a **új New Relic-tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-155">On the **New Relic Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_url.png)

    <span data-ttu-id="e0fe9-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.newrelic.com`</span><span class="sxs-lookup"><span data-stu-id="e0fe9-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.newrelic.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="e0fe9-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-158">The value is not real.</span></span> <span data-ttu-id="e0fe9-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="e0fe9-160">Ügyfél [új New Relic-ügyfél-támogatási csoport](https://support.newrelic.com/) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-160">Contact [New Relic Client support team](https://support.newrelic.com/) to get the value.</span></span> 
 
4. <span data-ttu-id="e0fe9-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_certificate.png) 

5. <span data-ttu-id="e0fe9-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="e0fe9-165">A a **új New Relic-konfiguráció** kattintson **New Relic konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-165">On the **New Relic Configuration** section, click **Configure New Relic** to open **Configure sign-on** window.</span></span> <span data-ttu-id="e0fe9-166">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_configure.png) 

7. <span data-ttu-id="e0fe9-168">Egy másik webes böngészőablakban, jelentkezzen be a **New Relic** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-168">In a different web browser window, sign on to your **New Relic** company site as administrator.</span></span>

8. <span data-ttu-id="e0fe9-169">Kattintson a felső menüben **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-169">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="e0fe9-170">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-170">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797036.png "Account Settings")</span></span>

9. <span data-ttu-id="e0fe9-171">Kattintson a **biztonsági és hitelesítési** fülre, majd a **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-171">Click the **Security and authentication** tab, and then click the **Single sign on** tab.</span></span>
   
    <span data-ttu-id="e0fe9-172">![Egyszeri bejelentkezés](./media/active-directory-saas-new-relic-tutorial/ic797037.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-172">![Single Sign-On](./media/active-directory-saas-new-relic-tutorial/ic797037.png "Single Sign-On")</span></span>

10. <span data-ttu-id="e0fe9-173">A SAML párbeszédpanel lapon hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-173">On the SAML dialog page, perform the following steps:</span></span>
   
    <span data-ttu-id="e0fe9-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-174">![SAML](./media/active-directory-saas-new-relic-tutorial/ic797038.png "SAML")</span></span>
   
   <span data-ttu-id="e0fe9-175">a.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-175">a.</span></span> <span data-ttu-id="e0fe9-176">Kattintson a **Choose File** a letöltött Azure Active Directory-tanúsítványok feltöltéséről.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-176">Click **Choose File** to upload your downloaded Azure Active Directory certificate.</span></span>

   <span data-ttu-id="e0fe9-177">b.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-177">b.</span></span> <span data-ttu-id="e0fe9-178">A a **távoli bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-178">In the **Remote login URL** textbox,  paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="e0fe9-179">c.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-179">c.</span></span> <span data-ttu-id="e0fe9-180">A a **kijelentkezési URL-cím üzenetsorokra** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-180">In the **Logout landing URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

   <span data-ttu-id="e0fe9-181">d.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-181">d.</span></span> <span data-ttu-id="e0fe9-182">Kattintson a **menti a módosításokat**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-182">Click **Save my changes**.</span></span>

> [!TIP]
> <span data-ttu-id="e0fe9-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="e0fe9-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="e0fe9-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="e0fe9-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="e0fe9-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="e0fe9-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0fe9-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="e0fe9-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="e0fe9-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="e0fe9-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="e0fe9-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="e0fe9-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e0fe9-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-new-relic-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="e0fe9-198">a.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-198">a.</span></span> <span data-ttu-id="e0fe9-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="e0fe9-200">b.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-200">b.</span></span> <span data-ttu-id="e0fe9-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="e0fe9-202">c.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-202">c.</span></span> <span data-ttu-id="e0fe9-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="e0fe9-204">d.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-204">d.</span></span> <span data-ttu-id="e0fe9-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-205">Click **Create**.</span></span>
 
### <a name="creating-a-new-relic-test-user"></a><span data-ttu-id="e0fe9-206">New Relic tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e0fe9-206">Creating a New Relic test user</span></span>

<span data-ttu-id="e0fe9-207">Ahhoz, hogy az Azure Active Directory – felhasználók jelentkezzenek be az új New Relic, akkor ki kell építenie a New Relic.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-207">In order to enable Azure Active Directory users to log in to New Relic, they must be provisioned into New Relic.</span></span> <span data-ttu-id="e0fe9-208">New Relic esetén kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-208">In the case of New Relic, provisioning is a manual task.</span></span>

<span data-ttu-id="e0fe9-209">**Egy új New Relic fiókot kiépítéséhez, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-209">**To provision a user account to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="e0fe9-210">Jelentkezzen be a **New Relic** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-210">Log in to your **New Relic** company site as administrator.</span></span>

2. <span data-ttu-id="e0fe9-211">Kattintson a felső menüben **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-211">In the menu on the top, click **Account Settings**.</span></span>
   
    <span data-ttu-id="e0fe9-212">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-212">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797040.png "Account Settings")</span></span>

3. <span data-ttu-id="e0fe9-213">Az a **fiók** a bal oldali ablaktábláján kattintson **összegzés**, és kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-213">In the **Account** pane on the left side, click **Summary**, and then click **Add user**.</span></span>
   
    <span data-ttu-id="e0fe9-214">![Fiókbeállítások](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Fiókbeállítások")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-214">![Account Settings](./media/active-directory-saas-new-relic-tutorial/ic797041.png "Account Settings")</span></span>

4. <span data-ttu-id="e0fe9-215">Az a **aktív felhasználók** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="e0fe9-215">On the **Active users** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="e0fe9-216">![Aktív felhasználók](./media/active-directory-saas-new-relic-tutorial/ic797042.png "aktív felhasználók")</span><span class="sxs-lookup"><span data-stu-id="e0fe9-216">![Active Users](./media/active-directory-saas-new-relic-tutorial/ic797042.png "Active Users")</span></span>
   
    <span data-ttu-id="e0fe9-217">a.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-217">a.</span></span> <span data-ttu-id="e0fe9-218">Az a **E-mail** szövegmező, írja be a kívánt rendelkezés érvényes Azure Active Directory felhasználó e-mail címe.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-218">In the **Email** textbox, type the email address of a valid Azure Active Directory user you want to provision.</span></span>

    <span data-ttu-id="e0fe9-219">b.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-219">b.</span></span> <span data-ttu-id="e0fe9-220">Mint **szerepkör** válasszon **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-220">As **Role** select **User**.</span></span>

    <span data-ttu-id="e0fe9-221">c.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-221">c.</span></span> <span data-ttu-id="e0fe9-222">Kattintson a **adja hozzá a felhasználót**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-222">Click **Add this user**.</span></span>

>[!NOTE]
><span data-ttu-id="e0fe9-223">Bármely más New Relic felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz új New Relic által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-223">You can use any other New Relic user account creation tools or APIs provided by New Relic to provision AAD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="e0fe9-224">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e0fe9-224">Assigning the Azure AD test user</span></span>

<span data-ttu-id="e0fe9-225">Ebben a szakaszban Britta Simon hozzáférést biztosít a New Relic által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-225">In this section, you enable Britta Simon to use Azure single sign-on by granting access to New Relic.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e0fe9-227">**New Relic Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="e0fe9-227">**To assign Britta Simon to New Relic, perform the following steps:**</span></span>

1. <span data-ttu-id="e0fe9-228">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-228">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e0fe9-230">Az alkalmazások listában válassza ki a **New Relic**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-230">In the applications list, select **New Relic**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-new-relic-tutorial/tutorial_newrelic_app.png) 

3. <span data-ttu-id="e0fe9-232">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-232">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="e0fe9-234">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-234">Click **Add** button.</span></span> <span data-ttu-id="e0fe9-235">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-235">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="e0fe9-237">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-237">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="e0fe9-238">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-238">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="e0fe9-239">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-239">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="e0fe9-240">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e0fe9-240">Testing single sign-on</span></span>

<span data-ttu-id="e0fe9-241">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-241">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="e0fe9-242">Ha a hozzáférési panelen a New Relic csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a New Relic-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="e0fe9-242">When you click the New Relic tile in the Access Panel, you should get automatically signed-on to your New Relic application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e0fe9-243">További források</span><span class="sxs-lookup"><span data-stu-id="e0fe9-243">Additional resources</span></span>

* [<span data-ttu-id="e0fe9-244">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="e0fe9-244">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e0fe9-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e0fe9-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-new-relic-tutorial/tutorial_general_203.png

