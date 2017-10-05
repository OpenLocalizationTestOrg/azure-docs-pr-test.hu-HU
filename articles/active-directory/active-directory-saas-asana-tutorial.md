---
title: "Oktatóanyag: Azure Active Directoryval integrált Asana |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Asana között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 837e38fe-8f55-475c-87f4-6394dc1fee2b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/01/2017
ms.author: jeedes
ms.openlocfilehash: a2f0cecb93cc382bcfd710c44eb70f80ae67f9b6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-asana"></a><span data-ttu-id="a5261-103">Oktatóanyag: Azure Active Directoryval integrált Asana</span><span class="sxs-lookup"><span data-stu-id="a5261-103">Tutorial: Azure Active Directory integration with Asana</span></span>

<span data-ttu-id="a5261-104">Ebben az oktatóanyagban elsajátíthatja Asana integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a5261-104">In this tutorial, you learn how to integrate Asana with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a5261-105">Asana integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a5261-105">Integrating Asana with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a5261-106">Megadhatja a Asana hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a5261-106">You can control in Azure AD who has access to Asana</span></span>
- <span data-ttu-id="a5261-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Asana (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a5261-107">You can enable your users to automatically get signed-on to Asana (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a5261-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a5261-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a5261-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a5261-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a5261-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a5261-110">Prerequisites</span></span>

<span data-ttu-id="a5261-111">Konfigurálása az Azure AD-integrációs Asana, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a5261-111">To configure Azure AD integration with Asana, you need the following items:</span></span>

- <span data-ttu-id="a5261-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a5261-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a5261-113">Egy Asana egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a5261-113">An Asana single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a5261-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a5261-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a5261-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a5261-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a5261-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a5261-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a5261-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a5261-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a5261-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a5261-118">Scenario description</span></span>
<span data-ttu-id="a5261-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a5261-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a5261-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a5261-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a5261-121">A gyűjteményből Asana hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a5261-121">Adding Asana from the gallery</span></span>
2. <span data-ttu-id="a5261-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a5261-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-asana-from-the-gallery"></a><span data-ttu-id="a5261-123">A gyűjteményből Asana hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a5261-123">Adding Asana from the gallery</span></span>
<span data-ttu-id="a5261-124">Az Azure AD integrálása a Asana konfigurálásához kell hozzáadnia Asana a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a5261-124">To configure the integration of Asana into Azure AD, you need to add Asana from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a5261-125">**A gyűjteményből Asana hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a5261-125">**To add Asana from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a5261-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a5261-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="a5261-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a5261-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a5261-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a5261-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="a5261-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="a5261-133">Írja be a keresőmezőbe, **Asana**, jelölje be **Asana** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a5261-133">In the search box, type **Asana**, select **Asana** from result panel then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a5261-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a5261-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a5261-136">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Asana</span><span class="sxs-lookup"><span data-stu-id="a5261-136">In this section, you configure and test Azure AD single sign-on with Asana based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a5261-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Asana a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a5261-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Asana is to a user in Azure AD.</span></span> <span data-ttu-id="a5261-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Asana közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a5261-138">In other words, a link relationship between an Azure AD user and the related user in Asana needs to be established.</span></span>

<span data-ttu-id="a5261-139">Asana, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a5261-139">In Asana, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a5261-140">Az Azure AD egyszeri bejelentkezést a Asana tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a5261-140">To configure and test Azure AD single sign-on with Asana, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a5261-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a5261-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a5261-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a5261-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a5261-143">**[Hozzon létre egy Asana tesztfelhasználó](#create-an-asana-test-user)**  - való Britta Simon valami Asana, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a5261-143">**[Create an Asana test user](#create-an-asana-test-user)** - to have a counterpart of Britta Simon in Asana that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a5261-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a5261-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a5261-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a5261-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a5261-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a5261-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a5261-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Asana alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a5261-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Asana application.</span></span>

<span data-ttu-id="a5261-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Asana, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a5261-148">**To configure Azure AD single sign-on with Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="a5261-149">Az Azure portálon a a **Asana** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a5261-149">In the Azure portal, on the **Asana** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a5261-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a5261-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-asana-tutorial/tutorial_asana_samlbase.png)

3. <span data-ttu-id="a5261-153">Az a **Asana tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a5261-153">On the **Asana Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Asana tartomány és az URL-címek](./media/active-directory-saas-asana-tutorial/tutorial_asana_url.png)

    <span data-ttu-id="a5261-155">a.</span><span class="sxs-lookup"><span data-stu-id="a5261-155">a.</span></span> <span data-ttu-id="a5261-156">Az a **bejelentkezési URL-cím** szövegmezőhöz típus URL-címe:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="a5261-156">In the **Sign-on URL** textbox, type URL: `https://app.asana.com/`</span></span>

    <span data-ttu-id="a5261-157">b.</span><span class="sxs-lookup"><span data-stu-id="a5261-157">b.</span></span> <span data-ttu-id="a5261-158">Az a **azonosító** szövegmezőhöz objektumtípus-érték:`https://app.asana.com/`</span><span class="sxs-lookup"><span data-stu-id="a5261-158">In the **Identifier** textbox, type value: `https://app.asana.com/`</span></span>
 
4. <span data-ttu-id="a5261-159">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a5261-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-asana-tutorial/tutorial_asana_certificate.png)
    
5. <span data-ttu-id="a5261-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a5261-163">A a **Asana konfigurációs** kattintson **konfigurálása Asana** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a5261-163">On the **Asana Configuration** section, click **Configure Asana** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a5261-164">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a5261-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Asana konfiguráció](./media/active-directory-saas-asana-tutorial/tutorial_asana_configure.png) 

7. <span data-ttu-id="a5261-166">Egy másik böngészőablakban bejelentkezés az Asana alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="a5261-166">In a different browser window, sign-on to your Asana application.</span></span> <span data-ttu-id="a5261-167">Asana SSO konfigurálásához a munkaterület beállításához kattintson a képernyő jobb felső sarkában a munkaterület neve való hozzáféréshez.</span><span class="sxs-lookup"><span data-stu-id="a5261-167">To configure SSO in Asana, access the workspace settings by clicking the workspace name on the top right corner of the screen.</span></span> <span data-ttu-id="a5261-168">Kattintson a  **\<a munkaterület neve\> beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a5261-168">Then, click on **\<your workspace name\> Settings**.</span></span> 
   
    ![Asana egyszeri bejelentkezési beállítások](./media/active-directory-saas-asana-tutorial/tutorial_asana_09.png)

8. <span data-ttu-id="a5261-170">Az a **szervezeti beállítások** ablak, kattintson a **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="a5261-170">On the **Organization settings** window, click **Administration**.</span></span> <span data-ttu-id="a5261-171">Kattintson a **tagok SAML keresztül kell bejelentkezniük** engedélyezése az egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a5261-171">Then, click **Members must log in via SAML** to enable the SSO configuration.</span></span> <span data-ttu-id="a5261-172">Hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a5261-172">The perform the following steps:</span></span>
   
    ![Egyszeri bejelentkezés szervezeti beállítások konfigurálása](./media/active-directory-saas-asana-tutorial/tutorial_asana_10.png)  

     <span data-ttu-id="a5261-174">a.</span><span class="sxs-lookup"><span data-stu-id="a5261-174">a.</span></span> <span data-ttu-id="a5261-175">Az a **bejelentkezési URL-címe** szövegmező, illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe**.</span><span class="sxs-lookup"><span data-stu-id="a5261-175">In the **Sign-in page URL** textbox, paste the **SAML Single Sign-On Service URL**.</span></span>

     <span data-ttu-id="a5261-176">b.</span><span class="sxs-lookup"><span data-stu-id="a5261-176">b.</span></span> <span data-ttu-id="a5261-177">Az Azure portálról letöltött tanúsítvány kattintson a jobb gombbal, majd nyissa meg a tanúsítványfájlt, a Jegyzettömb vagy az előnyben részesített szövegszerkesztőben.</span><span class="sxs-lookup"><span data-stu-id="a5261-177">Right click the certificate downloaded from Azure portal, then open the certificate file using Notepad or your preferred text editor.</span></span> <span data-ttu-id="a5261-178">Másolja a tartalmat a kezdő és záró tanúsítvány címe között, és illessze be a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a5261-178">Copy the content between the begin and the end certificate title and paste it in the **X.509 Certificate** textbox.</span></span>

9. <span data-ttu-id="a5261-179">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-179">Click **Save**.</span></span> <span data-ttu-id="a5261-180">Ugrás a [Asana útmutató egyszeri bejelentkezés beállításával kapcsolatos](https://asana.com/guide/help/premium/authentication#gl-saml) Ha segítségre van szüksége további.</span><span class="sxs-lookup"><span data-stu-id="a5261-180">Go to [Asana guide for setting up SSO](https://asana.com/guide/help/premium/authentication#gl-saml) if you need further assistance.</span></span>

> [!TIP]
> <span data-ttu-id="a5261-181">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a5261-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a5261-182">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a5261-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a5261-183">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a5261-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a5261-184">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="a5261-184">Create an Azure AD test user</span></span>

<span data-ttu-id="a5261-185">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a5261-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="a5261-187">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a5261-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a5261-188">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a5261-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-asana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a5261-190">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a5261-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-asana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a5261-192">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a5261-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a5261-194">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a5261-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A Hozzáadás gombra.](./media/active-directory-saas-asana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a5261-196">a.</span><span class="sxs-lookup"><span data-stu-id="a5261-196">a.</span></span> <span data-ttu-id="a5261-197">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a5261-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a5261-198">b.</span><span class="sxs-lookup"><span data-stu-id="a5261-198">b.</span></span> <span data-ttu-id="a5261-199">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a5261-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a5261-200">c.</span><span class="sxs-lookup"><span data-stu-id="a5261-200">c.</span></span> <span data-ttu-id="a5261-201">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a5261-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a5261-202">d.</span><span class="sxs-lookup"><span data-stu-id="a5261-202">d.</span></span> <span data-ttu-id="a5261-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-203">Click **Create**.</span></span>
 
### <a name="create-an-asana-test-user"></a><span data-ttu-id="a5261-204">Hozzon létre egy Asana tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="a5261-204">Create an Asana test user</span></span>

<span data-ttu-id="a5261-205">Ebben a szakaszban egy Asana Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a5261-205">In this section, you create a user called Britta Simon in Asana.</span></span>

1. <span data-ttu-id="a5261-206">A **Asana**, navigáljon a **csapatok** szakaszt, a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="a5261-206">On **Asana**, go to the **Teams** section on the left panel.</span></span> <span data-ttu-id="a5261-207">Kattintson a plusz jelre gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-207">Click the plus sign button.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-asana-tutorial/tutorial_asana_12.png) 

2. <span data-ttu-id="a5261-209">Írja be az e-mailt britta.simon@contoso.com a szövegmezőbe, és válassza ki a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="a5261-209">Type the email britta.simon@contoso.com in the text box and then select **Invite**.</span></span>

3. <span data-ttu-id="a5261-210">Kattintson a **küldése a meghívás**.</span><span class="sxs-lookup"><span data-stu-id="a5261-210">Click **Send Invite**.</span></span> <span data-ttu-id="a5261-211">Az új felhasználó kap egy e-mail az az e-mail fiókjába.</span><span class="sxs-lookup"><span data-stu-id="a5261-211">The new user will receive an email into her email account.</span></span> <span data-ttu-id="a5261-212">Egyenként kell létrehozni, és ellenőrizze a fiók.</span><span class="sxs-lookup"><span data-stu-id="a5261-212">She will need to create and validate the account.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="a5261-213">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="a5261-213">Assign the Azure AD test user</span></span>

<span data-ttu-id="a5261-214">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Asana Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a5261-214">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Asana.</span></span>

![A felhasználói szerepkör hozzárendelése][200]

<span data-ttu-id="a5261-216">**Britta Simon hozzárendelése Asana, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a5261-216">**To assign Britta Simon to Asana, perform the following steps:**</span></span>

1. <span data-ttu-id="a5261-217">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a5261-217">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a5261-219">Az alkalmazások listában válassza ki a **Asana**.</span><span class="sxs-lookup"><span data-stu-id="a5261-219">In the applications list, select **Asana**.</span></span>

    ![Az alkalmazások listáját a Asana hivatkozás](./media/active-directory-saas-asana-tutorial/tutorial_asana_app.png) 

3. <span data-ttu-id="a5261-221">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a5261-221">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="a5261-223">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a5261-223">Click **Add** button.</span></span> <span data-ttu-id="a5261-224">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a5261-224">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="a5261-226">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a5261-226">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a5261-227">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a5261-227">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a5261-228">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a5261-228">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a5261-229">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a5261-229">Test single sign-on</span></span>

<span data-ttu-id="a5261-230">Ez a szakasz célja az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a5261-230">The objective of this section is to test your Azure AD single sign-on.</span></span>

<span data-ttu-id="a5261-231">Ugrás a Asana bejelentkezési lapot.</span><span class="sxs-lookup"><span data-stu-id="a5261-231">Go to Asana login page.</span></span> <span data-ttu-id="a5261-232">Az E-mail cím szövegmezőjének helyezze be az e-mail cím britta.simon@contoso.com. A jelszó szövegmezőjét hagyja üresen, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a5261-232">In the Email address textbox, insert the email address britta.simon@contoso.com. Leave the password textbox in blank and then click **Log In**.</span></span> <span data-ttu-id="a5261-233">Az Azure AD bejelentkezési oldalára irányítja.</span><span class="sxs-lookup"><span data-stu-id="a5261-233">You will be redirected to Azure AD login page.</span></span> <span data-ttu-id="a5261-234">Végezze el az Azure AD hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="a5261-234">Complete your Azure AD credentials.</span></span> <span data-ttu-id="a5261-235">Most jelentkezett Asana.</span><span class="sxs-lookup"><span data-stu-id="a5261-235">Now, you are logged in on Asana.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a5261-236">További források</span><span class="sxs-lookup"><span data-stu-id="a5261-236">Additional resources</span></span>

* [<span data-ttu-id="a5261-237">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a5261-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a5261-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a5261-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-asana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-asana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-asana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-asana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-asana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-asana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-asana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-asana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-asana-tutorial/tutorial_general_203.png
[10]: ./media/active-directory-saas-asana-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-asana-tutorial/tutorial_general_070.png
