---
title: "Oktatóanyag: Azure Active Directoryval integrált LogicMonitor |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és LogicMonitor között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 496156c3-0e22-4492-b36f-2c29c055e087
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: jeedes
ms.openlocfilehash: e49960cac868f80af3e9165a9f75e49be87515f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-logicmonitor"></a><span data-ttu-id="bca42-103">Oktatóanyag: Azure Active Directoryval integrált LogicMonitor</span><span class="sxs-lookup"><span data-stu-id="bca42-103">Tutorial: Azure Active Directory integration with LogicMonitor</span></span>

<span data-ttu-id="bca42-104">Ebben az oktatóanyagban elsajátíthatja LogicMonitor integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="bca42-104">In this tutorial, you learn how to integrate LogicMonitor with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="bca42-105">LogicMonitor integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="bca42-105">Integrating LogicMonitor with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="bca42-106">Megadhatja a LogicMonitor hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="bca42-106">You can control in Azure AD who has access to LogicMonitor</span></span>
- <span data-ttu-id="bca42-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett LogicMonitor (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="bca42-107">You can enable your users to automatically get signed-on to LogicMonitor (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="bca42-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bca42-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="bca42-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="bca42-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bca42-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="bca42-110">Prerequisites</span></span>

<span data-ttu-id="bca42-111">Konfigurálása az Azure AD-integrációs LogicMonitor, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="bca42-111">To configure Azure AD integration with LogicMonitor, you need the following items:</span></span>

- <span data-ttu-id="bca42-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="bca42-112">An Azure AD subscription</span></span>
- <span data-ttu-id="bca42-113">Egy LogicMonitor egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="bca42-113">A LogicMonitor single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="bca42-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="bca42-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="bca42-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="bca42-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="bca42-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="bca42-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="bca42-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="bca42-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="bca42-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="bca42-118">Scenario description</span></span>
<span data-ttu-id="bca42-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="bca42-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="bca42-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="bca42-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="bca42-121">A gyűjteményből LogicMonitor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bca42-121">Adding LogicMonitor from the gallery</span></span>
2. <span data-ttu-id="bca42-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bca42-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-logicmonitor-from-the-gallery"></a><span data-ttu-id="bca42-123">A gyűjteményből LogicMonitor hozzáadása</span><span class="sxs-lookup"><span data-stu-id="bca42-123">Adding LogicMonitor from the gallery</span></span>
<span data-ttu-id="bca42-124">Az Azure AD integrálása a LogicMonitor konfigurálásához kell hozzáadnia LogicMonitor a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="bca42-124">To configure the integration of LogicMonitor into Azure AD, you need to add LogicMonitor from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="bca42-125">**A gyűjteményből LogicMonitor hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bca42-125">**To add LogicMonitor from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="bca42-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bca42-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="bca42-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="bca42-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="bca42-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bca42-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="bca42-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="bca42-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="bca42-133">Írja be a keresőmezőbe, **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="bca42-133">In the search box, type **LogicMonitor**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_search.png)

5. <span data-ttu-id="bca42-135">Az eredmények panelen válassza ki a **LogicMonitor**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bca42-135">In the results panel, select **LogicMonitor**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="bca42-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="bca42-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="bca42-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján LogicMonitor.</span><span class="sxs-lookup"><span data-stu-id="bca42-138">In this section, you configure and test Azure AD single sign-on with LogicMonitor based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="bca42-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó LogicMonitor a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="bca42-139">For single sign-on to work, Azure AD needs to know what the counterpart user in LogicMonitor is to a user in Azure AD.</span></span> <span data-ttu-id="bca42-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a LogicMonitor közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="bca42-140">In other words, a link relationship between an Azure AD user and the related user in LogicMonitor needs to be established.</span></span>

<span data-ttu-id="bca42-141">LogicMonitor, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="bca42-141">In LogicMonitor, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="bca42-142">Az Azure AD egyszeri bejelentkezést a LogicMonitor tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="bca42-142">To configure and test Azure AD single sign-on with LogicMonitor, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="bca42-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="bca42-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="bca42-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="bca42-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="bca42-145">**[LogicMonitor tesztfelhasználó létrehozása](#creating-a-logicmonitor-test-user)**  - való Britta Simon valami LogicMonitor, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="bca42-145">**[Creating a LogicMonitor test user](#creating-a-logicmonitor-test-user)** - to have a counterpart of Britta Simon in LogicMonitor that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="bca42-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bca42-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="bca42-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="bca42-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="bca42-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bca42-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="bca42-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az LogicMonitor alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="bca42-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your LogicMonitor application.</span></span>

<span data-ttu-id="bca42-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés LogicMonitor, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bca42-150">**To configure Azure AD single sign-on with LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="bca42-151">Az Azure portálon a a **LogicMonitor** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="bca42-151">In the Azure portal, on the **LogicMonitor** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="bca42-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="bca42-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_samlbase.png)

3. <span data-ttu-id="bca42-155">Az a **LogicMonitor tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bca42-155">On the **LogicMonitor Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_url.png)

    <span data-ttu-id="bca42-157">a.</span><span class="sxs-lookup"><span data-stu-id="bca42-157">a.</span></span> <span data-ttu-id="bca42-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="bca42-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    <span data-ttu-id="bca42-159">b.</span><span class="sxs-lookup"><span data-stu-id="bca42-159">b.</span></span> <span data-ttu-id="bca42-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.logicmonitor.com`</span><span class="sxs-lookup"><span data-stu-id="bca42-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.logicmonitor.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="bca42-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="bca42-161">These values are not real.</span></span> <span data-ttu-id="bca42-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="bca42-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="bca42-163">Ügyfél [LogicMonitor ügyfél-támogatási csoport](https://www.logicmonitor.com/contact/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="bca42-163">Contact [LogicMonitor Client support team](https://www.logicmonitor.com/contact/) to get these values.</span></span> 
 


4. <span data-ttu-id="bca42-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="bca42-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_certificate.png) 

5. <span data-ttu-id="bca42-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="bca42-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="bca42-168">Jelentkezzen be a **LogicMonitor** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="bca42-168">Log in to your **LogicMonitor** company site as an administrator.</span></span>

7. <span data-ttu-id="bca42-169">Kattintson a felső menüben **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="bca42-169">In the menu on the top, click **Settings**.</span></span>
   
   <span data-ttu-id="bca42-170">![Beállítások](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="bca42-170">![Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790052.png "Settings")</span></span>

8. <span data-ttu-id="bca42-171">Kattintson a bal oldali navigációs bat, **egyszeri bejelentkezés**</span><span class="sxs-lookup"><span data-stu-id="bca42-171">In the navigation bat on the left side, click **Single Sign On**</span></span>
   
   <span data-ttu-id="bca42-172">![Egyszeri bejelentkezés](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="bca42-172">![Single Sign-On](./media/active-directory-saas-logicmonitor-tutorial/ic790053.png "Single Sign-On")</span></span>

9. <span data-ttu-id="bca42-173">Az a **egyszeri bejelentkezés (SSO) beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bca42-173">In the **Single Sign-on (SSO) settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="bca42-174">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="bca42-174">![Single Sign-On Settings](./media/active-directory-saas-logicmonitor-tutorial/ic790054.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="bca42-175">a.</span><span class="sxs-lookup"><span data-stu-id="bca42-175">a.</span></span> <span data-ttu-id="bca42-176">Válassza ki **egyszeri bejelentkezés engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="bca42-176">Select **Enable Single Sign-on**.</span></span>

   <span data-ttu-id="bca42-177">b.</span><span class="sxs-lookup"><span data-stu-id="bca42-177">b.</span></span> <span data-ttu-id="bca42-178">Mint **alapértelmezett szerepkör-hozzárendelés**, jelölje be **readonly**.</span><span class="sxs-lookup"><span data-stu-id="bca42-178">As **Default Role Assignment**, select **readonly**.</span></span>
   
   <span data-ttu-id="bca42-179">c.</span><span class="sxs-lookup"><span data-stu-id="bca42-179">c.</span></span> <span data-ttu-id="bca42-180">Nyissa meg a letöltött metaadat-fájlt a Jegyzettömbben, és illessze be a fájl tartalma a **Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="bca42-180">Open the downloaded metadata file in notepad, and then paste content of the file into the **Identity Provider Metadata** textbox.</span></span>
   
   <span data-ttu-id="bca42-181">d.</span><span class="sxs-lookup"><span data-stu-id="bca42-181">d.</span></span> <span data-ttu-id="bca42-182">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="bca42-182">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="bca42-183">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="bca42-183">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="bca42-184">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="bca42-184">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="bca42-185">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="bca42-185">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="bca42-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bca42-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="bca42-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="bca42-187">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="bca42-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bca42-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="bca42-190">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="bca42-190">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="bca42-192">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="bca42-192">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="bca42-194">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="bca42-194">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="bca42-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="bca42-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-logicmonitor-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="bca42-198">a.</span><span class="sxs-lookup"><span data-stu-id="bca42-198">a.</span></span> <span data-ttu-id="bca42-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="bca42-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="bca42-200">b.</span><span class="sxs-lookup"><span data-stu-id="bca42-200">b.</span></span> <span data-ttu-id="bca42-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="bca42-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="bca42-202">c.</span><span class="sxs-lookup"><span data-stu-id="bca42-202">c.</span></span> <span data-ttu-id="bca42-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bca42-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="bca42-204">d.</span><span class="sxs-lookup"><span data-stu-id="bca42-204">d.</span></span> <span data-ttu-id="bca42-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="bca42-205">Click **Create**.</span></span>
 
### <a name="creating-a-logicmonitor-test-user"></a><span data-ttu-id="bca42-206">LogicMonitor tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="bca42-206">Creating a LogicMonitor test user</span></span>

<span data-ttu-id="bca42-207">Az AAD-felhasználókat kell jelentkezhetnek be akkor ki kell építenie a LogicMonitor alkalmazást az Azure Active Directory felhasználói neveket.</span><span class="sxs-lookup"><span data-stu-id="bca42-207">For AAD users to be able to sign in, they must be provisioned to the LogicMonitor application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="bca42-208">**Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bca42-208">**To configure user provisioning, perform the following steps:**</span></span>

1. <span data-ttu-id="bca42-209">Jelentkezzen be rendszergazdaként a LogicMonitor vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="bca42-209">Log in to your LogicMonitor company site as an administrator.</span></span>

2. <span data-ttu-id="bca42-210">Kattintson a felső menüben **beállítások**, és kattintson a **szerepkörök és a felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="bca42-210">In the menu on the top, click **Settings**, and then click **Roles and Users**.</span></span>
   
   <span data-ttu-id="bca42-211">![Szerepkörök és a felhasználók](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "szerepkörök és a felhasználók")</span><span class="sxs-lookup"><span data-stu-id="bca42-211">![Roles and Users](./media/active-directory-saas-logicmonitor-tutorial/ic790056.png "Roles and Users")</span></span>

3. <span data-ttu-id="bca42-212">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="bca42-212">Click **Add**.</span></span>

4. <span data-ttu-id="bca42-213">Az a **vegyen fel egy fiókot** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="bca42-213">In the **Add an account** section, perform the following steps:</span></span>
   
   <span data-ttu-id="bca42-214">![Vegyen fel egy fiókot](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "fiók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="bca42-214">![Add an account](./media/active-directory-saas-logicmonitor-tutorial/ic790057.png "Add an account")</span></span>
   
   <span data-ttu-id="bca42-215">a.</span><span class="sxs-lookup"><span data-stu-id="bca42-215">a.</span></span> <span data-ttu-id="bca42-216">Típus a **felhasználónév**, **E-mail**, **jelszó**, és **írja be újra jelszó** szeretné azokat a kapcsolódó szövegmezők rendelkezés Azure Active Directory felhasználó értékek.</span><span class="sxs-lookup"><span data-stu-id="bca42-216">Type the **Username**, **Email**, **Password**, and **Retype password** values of the Azure Active Directory user you want to provision into the related textboxes.</span></span>
   
   <span data-ttu-id="bca42-217">b.</span><span class="sxs-lookup"><span data-stu-id="bca42-217">b.</span></span> <span data-ttu-id="bca42-218">Válassza ki **szerepkörök**, **engedélyek megtekintése**, és a **állapot**.</span><span class="sxs-lookup"><span data-stu-id="bca42-218">Select **Roles**, **View Permissions**, and the **Status**.</span></span>
   
   <span data-ttu-id="bca42-219">c.</span><span class="sxs-lookup"><span data-stu-id="bca42-219">c.</span></span> <span data-ttu-id="bca42-220">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="bca42-220">Click **Submit**.</span></span>

>[!NOTE]
><span data-ttu-id="bca42-221">Bármely más LogicMonitor felhasználói fiók létrehozása eszközök vagy API-k által biztosított LogicMonitor kiépítését Azure Active Directory felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="bca42-221">You can use any other LogicMonitor user account creation tools or APIs provided by LogicMonitor to provision Azure Active Directory user accounts.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="bca42-222">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="bca42-222">Assigning the Azure AD test user</span></span>

<span data-ttu-id="bca42-223">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés LogicMonitor Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="bca42-223">In this section, you enable Britta Simon to use Azure single sign-on by granting access to LogicMonitor.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="bca42-225">**Britta Simon hozzárendelése LogicMonitor, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="bca42-225">**To assign Britta Simon to LogicMonitor, perform the following steps:**</span></span>

1. <span data-ttu-id="bca42-226">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="bca42-226">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="bca42-228">Az alkalmazások listában válassza ki a **LogicMonitor**.</span><span class="sxs-lookup"><span data-stu-id="bca42-228">In the applications list, select **LogicMonitor**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-logicmonitor-tutorial/tutorial_logicmonitor_app.png) 

3. <span data-ttu-id="bca42-230">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="bca42-230">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="bca42-232">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="bca42-232">Click **Add** button.</span></span> <span data-ttu-id="bca42-233">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bca42-233">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="bca42-235">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="bca42-235">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="bca42-236">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bca42-236">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="bca42-237">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="bca42-237">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="bca42-238">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="bca42-238">Testing single sign-on</span></span>

<span data-ttu-id="bca42-239">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="bca42-239">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>
 
<span data-ttu-id="bca42-240">Ha a hozzáférési panelen LogicMonitor csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az LogicMonitor alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="bca42-240">When you click the LogicMonitor tile in the Access Panel, you should get automatically signed-on to your LogicMonitor application.</span></span>
<span data-ttu-id="bca42-241">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="bca42-241">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="bca42-242">További források</span><span class="sxs-lookup"><span data-stu-id="bca42-242">Additional resources</span></span>

* [<span data-ttu-id="bca42-243">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="bca42-243">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="bca42-244">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="bca42-244">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-logicmonitor-tutorial/tutorial_general_203.png

