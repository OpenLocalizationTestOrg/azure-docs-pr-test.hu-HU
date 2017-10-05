---
title: "Oktatóanyag: Azure Active Directoryval integrált legfontosabb feladatai közé tartoznak OnDemand |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a legfontosabb feladatai közé tartoznak OnDemand között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 8bb32588579a0d40b9ae7e0f823c6daab21c856e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="0305b-103">Oktatóanyag: Azure Active Directoryval integrált legfontosabb feladatai közé tartoznak kötegmérete</span><span class="sxs-lookup"><span data-stu-id="0305b-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="0305b-104">Ebben az oktatóanyagban elsajátíthatja legfontosabb feladatai közé tartoznak OnDemand integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0305b-104">In this tutorial, you learn how to integrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0305b-105">Legfontosabb feladatai közé tartoznak OnDemand integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0305b-105">Integrating Cornerstone OnDemand with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0305b-106">Szabályozhatja az Azure AD, aki hozzáférhet a legfontosabb feladatai közé tartoznak kötegmérete</span><span class="sxs-lookup"><span data-stu-id="0305b-106">You can control in Azure AD who has access to Cornerstone OnDemand</span></span>
- <span data-ttu-id="0305b-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a legfontosabb feladatai közé tartoznak OnDemand (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="0305b-107">You can enable your users to automatically get signed-on to Cornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0305b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0305b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0305b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0305b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0305b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0305b-110">Prerequisites</span></span>

<span data-ttu-id="0305b-111">Konfigurálása az Azure AD-integrációs OnDemand legfontosabb feladatai közé tartoznak, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0305b-111">To configure Azure AD integration with Cornerstone OnDemand, you need the following items:</span></span>

- <span data-ttu-id="0305b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0305b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0305b-113">A legfontosabb feladatai közé tartoznak OnDemand egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0305b-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0305b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0305b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0305b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0305b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0305b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0305b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0305b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0305b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0305b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0305b-118">Scenario description</span></span>
<span data-ttu-id="0305b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0305b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0305b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0305b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0305b-121">Legfontosabb feladatai közé tartoznak OnDemand hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0305b-121">Adding Cornerstone OnDemand from the gallery</span></span>
2. <span data-ttu-id="0305b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0305b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-the-gallery"></a><span data-ttu-id="0305b-123">Legfontosabb feladatai közé tartoznak OnDemand hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0305b-123">Adding Cornerstone OnDemand from the gallery</span></span>
<span data-ttu-id="0305b-124">Az Azure AD integrálása a legfontosabb feladatai közé tartoznak OnDemand konfigurálásához kell hozzáadnia OnDemand legfontosabb feladatai közé tartoznak a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0305b-124">To configure the integration of Cornerstone OnDemand into Azure AD, you need to add Cornerstone OnDemand from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0305b-125">**Adja hozzá az OnDemand legfontosabb feladatai közé tartoznak a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0305b-125">**To add Cornerstone OnDemand from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0305b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0305b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0305b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0305b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0305b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0305b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0305b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0305b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0305b-133">Írja be a keresőmezőbe, **legfontosabb feladatai közé tartoznak OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="0305b-133">In the search box, type **Cornerstone OnDemand**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="0305b-135">Az eredmények panelen válassza ki a **legfontosabb feladatai közé tartoznak OnDemand**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0305b-135">In the results panel, select **Cornerstone OnDemand**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0305b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0305b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0305b-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a legfontosabb feladatai közé tartoznak OnDemand "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="0305b-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0305b-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó OnDemand legfontosabb feladatai közé tartoznak a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0305b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Cornerstone OnDemand is to a user in Azure AD.</span></span> <span data-ttu-id="0305b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a legfontosabb feladatai közé tartoznak OnDemand közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0305b-140">In other words, a link relationship between an Azure AD user and the related user in Cornerstone OnDemand needs to be established.</span></span>

<span data-ttu-id="0305b-141">A legfontosabb feladatai közé tartoznak OnDemand, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0305b-141">In Cornerstone OnDemand, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0305b-142">Az Azure AD egyszeri bejelentkezést a legfontosabb feladatai közé tartoznak OnDemand tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0305b-142">To configure and test Azure AD single sign-on with Cornerstone OnDemand, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0305b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0305b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0305b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0305b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0305b-145">**[A legfontosabb feladatai közé tartoznak OnDemand tesztfelhasználó létrehozása](#creating-a-cornerstone-ondemand-test-user)**  - való Britta Simon egy megfelelője a legfontosabb feladatai közé tartoznak OnDemand, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0305b-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - to have a counterpart of Britta Simon in Cornerstone OnDemand that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0305b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0305b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0305b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0305b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0305b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0305b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0305b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a legfontosabb feladatai közé tartoznak OnDemand alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0305b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="0305b-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés legfontosabb feladatai közé tartoznak OnDemand, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0305b-150">**To configure Azure AD single sign-on with Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="0305b-151">Az Azure portálon a a **legfontosabb feladatai közé tartoznak OnDemand** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0305b-151">In the Azure portal, on the **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0305b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0305b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="0305b-155">Az a **legfontosabb feladatai közé tartoznak OnDemand tartomány és az URL-címek** csoportjában hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="0305b-155">On the **Cornerstone OnDemand Domain and URLs** section, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="0305b-157">a.</span><span class="sxs-lookup"><span data-stu-id="0305b-157">a.</span></span> <span data-ttu-id="0305b-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="0305b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="0305b-159">b.</span><span class="sxs-lookup"><span data-stu-id="0305b-159">b.</span></span> <span data-ttu-id="0305b-160">A **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="0305b-160">In **Identifier** textbox, type a URL using the following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0305b-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0305b-161">These values are not real.</span></span> <span data-ttu-id="0305b-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="0305b-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="0305b-163">Ügyfél [legfontosabb feladatai közé tartoznak OnDemand ügyfél-támogatási csoport](mailTo:moreinfo@csod.com) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0305b-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) to get these values.</span></span> 
 
4. <span data-ttu-id="0305b-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0305b-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="0305b-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0305b-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0305b-168">Az a **legfontosabb feladatai közé tartoznak OnDemand konfigurációs** területen kattintson **konfigurálása legfontosabb feladatai közé tartoznak OnDemand** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0305b-168">On the **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0305b-169">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0305b-169">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="0305b-171">Egyszeri bejelentkezés konfigurálása **legfontosabb feladatai közé tartoznak OnDemand** oldalon kell küldeniük a letöltött **tanúsítvány**, **Sign-Out URL-cím** és **SAML-alapú egyszeri bejelentkezési URL-címe** való [legfontosabb feladatai közé tartoznak OnDemand támogatási csoport](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="0305b-171">To configure single sign-on on **Cornerstone OnDemand** side, you need to send the downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  to [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="0305b-172">Akkor állítsa be ezt a beállítást, hogy a SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="0305b-172">They set this setting to have the SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="0305b-173">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0305b-173">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0305b-174">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0305b-174">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0305b-175">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0305b-175">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0305b-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0305b-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="0305b-177">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0305b-177">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0305b-179">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0305b-179">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0305b-180">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0305b-180">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0305b-182">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0305b-182">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0305b-184">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0305b-184">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0305b-186">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0305b-186">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0305b-188">a.</span><span class="sxs-lookup"><span data-stu-id="0305b-188">a.</span></span> <span data-ttu-id="0305b-189">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0305b-189">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0305b-190">b.</span><span class="sxs-lookup"><span data-stu-id="0305b-190">b.</span></span> <span data-ttu-id="0305b-191">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0305b-191">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0305b-192">c.</span><span class="sxs-lookup"><span data-stu-id="0305b-192">c.</span></span> <span data-ttu-id="0305b-193">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0305b-193">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0305b-194">d.</span><span class="sxs-lookup"><span data-stu-id="0305b-194">d.</span></span> <span data-ttu-id="0305b-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0305b-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="0305b-196">A legfontosabb feladatai közé tartoznak OnDemand tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0305b-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="0305b-197">Ahhoz, hogy az Azure AD-felhasználók legfontosabb feladatai közé tartoznak OnDemand bejelentkezni, azok ki kell építenie az OnDemand legfontosabb feladatai közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="0305b-197">In order to enable Azure AD users to log into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="0305b-198">Legfontosabb feladatai közé tartoznak Ondemanddetection, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="0305b-198">In the case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="0305b-199">Konfigurálhatja a felhasználók átadása, küldje el a (pl.: név, E-mail) az Azure AD-felhasználó meg szeretné azokat a [legfontosabb feladatai közé tartoznak OnDemand támogatási csoport](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="0305b-199">To configure user provisioning, send the information (e.g.: Name, Email) about the Azure AD user you want to provision to the [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="0305b-200">Bármely más legfontosabb feladatai közé tartoznak OnDemand felhasználói fiók létrehozása eszközök, vagy rendelkezés AAD felhasználói fiókokhoz legfontosabb feladatai közé tartoznak OnDemand által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="0305b-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand to provision AAD user accounts.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0305b-201">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0305b-201">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0305b-202">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés legfontosabb feladatai közé tartoznak OnDemand Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0305b-202">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Cornerstone OnDemand.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0305b-204">**Britta Simon hozzárendelése legfontosabb feladatai közé tartoznak OnDemand, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0305b-204">**To assign Britta Simon to Cornerstone OnDemand, perform the following steps:**</span></span>

1. <span data-ttu-id="0305b-205">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0305b-205">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0305b-207">Az alkalmazások listában válassza ki a **legfontosabb feladatai közé tartoznak OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="0305b-207">In the applications list, select **Cornerstone OnDemand**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="0305b-209">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0305b-209">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0305b-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0305b-211">Click **Add** button.</span></span> <span data-ttu-id="0305b-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0305b-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0305b-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0305b-214">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0305b-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0305b-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0305b-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0305b-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0305b-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0305b-217">Testing single sign-on</span></span>

<span data-ttu-id="0305b-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="0305b-218">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0305b-219">Ha a hozzáférési panelen legfontosabb feladatai közé tartoznak OnDemand csempére kattint, meg kell beolvasása automatikusan bejelentkezett a legfontosabb feladatai közé tartoznak OnDemand-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="0305b-219">When you click the Cornerstone OnDemand tile in the Access Panel, you should get automatically signed-on to your Cornerstone OnDemand application.</span></span>
<span data-ttu-id="0305b-220">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="0305b-220">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0305b-221">További források</span><span class="sxs-lookup"><span data-stu-id="0305b-221">Additional resources</span></span>

* [<span data-ttu-id="0305b-222">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0305b-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0305b-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0305b-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

