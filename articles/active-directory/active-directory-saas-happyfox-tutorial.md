---
title: "Oktatóanyag: Azure Active Directoryval integrált HappyFox |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és HappyFox között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 8204ee77-f64b-4fac-b64a-25ea534feac0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: jeedes
ms.openlocfilehash: 8b5ad750d7849e4037ed7ee93c48b5645c68e799
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-happyfox"></a><span data-ttu-id="2ee0b-103">Oktatóanyag: Azure Active Directoryval integrált HappyFox</span><span class="sxs-lookup"><span data-stu-id="2ee0b-103">Tutorial: Azure Active Directory integration with HappyFox</span></span>

<span data-ttu-id="2ee0b-104">Ebben az oktatóanyagban elsajátíthatja HappyFox integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2ee0b-104">In this tutorial, you learn how to integrate HappyFox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2ee0b-105">HappyFox integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-105">Integrating HappyFox with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="2ee0b-106">Megadhatja a HappyFox hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2ee0b-106">You can control in Azure AD who has access to HappyFox</span></span>
- <span data-ttu-id="2ee0b-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett HappyFox (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2ee0b-107">You can enable your users to automatically get signed-on to HappyFox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2ee0b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2ee0b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="2ee0b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2ee0b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2ee0b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2ee0b-110">Prerequisites</span></span>

<span data-ttu-id="2ee0b-111">Konfigurálása az Azure AD-integrációs HappyFox, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-111">To configure Azure AD integration with HappyFox, you need the following items:</span></span>

- <span data-ttu-id="2ee0b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2ee0b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2ee0b-113">Egy HappyFox egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="2ee0b-113">A HappyFox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2ee0b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2ee0b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2ee0b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2ee0b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2ee0b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2ee0b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-118">Scenario description</span></span>
<span data-ttu-id="2ee0b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2ee0b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2ee0b-121">A gyűjteményből HappyFox hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-121">Adding HappyFox from the gallery</span></span>
2. <span data-ttu-id="2ee0b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2ee0b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-happyfox-from-the-gallery"></a><span data-ttu-id="2ee0b-123">A gyűjteményből HappyFox hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-123">Adding HappyFox from the gallery</span></span>
<span data-ttu-id="2ee0b-124">Az Azure AD integrálása a HappyFox konfigurálásához kell hozzáadnia HappyFox a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-124">To configure the integration of HappyFox into Azure AD, you need to add HappyFox from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="2ee0b-125">**A gyűjteményből HappyFox hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2ee0b-125">**To add HappyFox from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="2ee0b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2ee0b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="2ee0b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2ee0b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2ee0b-133">Írja be a keresőmezőbe, **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-133">In the search box, type **HappyFox**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_search.png)

5. <span data-ttu-id="2ee0b-135">Az eredmények panelen válassza ki a **HappyFox**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-135">In the results panel, select **HappyFox**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2ee0b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2ee0b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2ee0b-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján HappyFox</span><span class="sxs-lookup"><span data-stu-id="2ee0b-138">In this section, you configure and test Azure AD single sign-on with HappyFox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2ee0b-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó HappyFox a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in HappyFox is to a user in Azure AD.</span></span> <span data-ttu-id="2ee0b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a HappyFox közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-140">In other words, a link relationship between an Azure AD user and the related user in HappyFox needs to be established.</span></span>

<span data-ttu-id="2ee0b-141">HappyFox, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-141">In HappyFox, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="2ee0b-142">Az Azure AD egyszeri bejelentkezést a HappyFox tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-142">To configure and test Azure AD single sign-on with HappyFox, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="2ee0b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="2ee0b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2ee0b-145">**[HappyFox tesztfelhasználó létrehozása](#creating-a-happyfox-test-user)**  - való Britta Simon valami HappyFox, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-145">**[Creating a HappyFox test user](#creating-a-happyfox-test-user)** - to have a counterpart of Britta Simon in HappyFox that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="2ee0b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2ee0b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2ee0b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2ee0b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az HappyFox alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HappyFox application.</span></span>

<span data-ttu-id="2ee0b-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés HappyFox, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2ee0b-150">**To configure Azure AD single sign-on with HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="2ee0b-151">Az Azure portálon a a **HappyFox** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-151">In the Azure portal, on the **HappyFox** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2ee0b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_samlbase.png)

3. <span data-ttu-id="2ee0b-155">Az a **HappyFox tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-155">On the **HappyFox Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_url.png)

    <span data-ttu-id="2ee0b-157">a.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-157">a.</span></span> <span data-ttu-id="2ee0b-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.happyfox.com/`</span><span class="sxs-lookup"><span data-stu-id="2ee0b-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/`</span></span>

    <span data-ttu-id="2ee0b-159">b.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-159">b.</span></span> <span data-ttu-id="2ee0b-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.happyfox.com/saml/metadata/`</span><span class="sxs-lookup"><span data-stu-id="2ee0b-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.happyfox.com/saml/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2ee0b-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-161">These values are not real.</span></span> <span data-ttu-id="2ee0b-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="2ee0b-163">Ügyfél [HappyFox ügyfél-támogatási csoport](https://support.happyfox.com/home) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-163">Contact [HappyFox Client support team](https://support.happyfox.com/home) to get these values.</span></span> 
 
4. <span data-ttu-id="2ee0b-164">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-164">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_certificate.png) 

5. <span data-ttu-id="2ee0b-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="2ee0b-168">A a **HappyFox konfigurációs** kattintson **konfigurálása HappyFox** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-168">On the **HappyFox Configuration** section, click **Configure HappyFox** to open **Configure sign-on** window.</span></span> <span data-ttu-id="2ee0b-169">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-169">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_configure.png) 

7. <span data-ttu-id="2ee0b-171">Jelentkezzen be a HappyFox személyzet portálon, és keresse meg **kezelése**, kattintson a **integrációja** fülre.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-171">Sign on to your HappyFox staff portal and navigate to **Manage**, Click on **Integrations** tab.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/header.png) 

8. <span data-ttu-id="2ee0b-173">Az integráció lapon kattintson a **konfigurálása** alatt **SAML-integráció** az egyszeri bejelentkezési a beállításainak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-173">In the Integrations tab, Click **Configure** under **SAML Integration** to open the Single Sign On Settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/configure.png) 

9. <span data-ttu-id="2ee0b-175">SAML-alapú konfigurációs szakaszon belül illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure portálról másolt **SSO célként megadott URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-175">Inside SAML configuration section, paste the **SAML Single Sign-On Service URL** that you have copied from Azure portal into **SSO Target URL** textbox.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/targeturl.png)

10. <span data-ttu-id="2ee0b-177">Nyissa meg a Jegyzettömbben az Azure portálról letöltött a tanúsítványt, és illessze be a benne lévő tartalom **IdP aláírás** szakasz.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-177">Open the certificate downloaded from Azure portal in notepad and paste its content in **IdP Signature** section.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/cert.png)

11. <span data-ttu-id="2ee0b-179">Kattintson a **beállítások mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-179">Click **Save Settings** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/savesettings.png)

> [!TIP]
> <span data-ttu-id="2ee0b-181">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="2ee0b-181">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="2ee0b-182">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-182">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="2ee0b-183">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2ee0b-183">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2ee0b-184">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-184">Creating an Azure AD test user</span></span>
<span data-ttu-id="2ee0b-185">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-185">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2ee0b-187">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2ee0b-187">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="2ee0b-188">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-188">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2ee0b-190">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-190">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2ee0b-192">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-192">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2ee0b-194">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="2ee0b-194">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-happyfox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2ee0b-196">a.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-196">a.</span></span> <span data-ttu-id="2ee0b-197">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-197">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2ee0b-198">b.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-198">b.</span></span> <span data-ttu-id="2ee0b-199">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-199">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2ee0b-200">c.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-200">c.</span></span> <span data-ttu-id="2ee0b-201">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-201">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="2ee0b-202">d.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-202">d.</span></span> <span data-ttu-id="2ee0b-203">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-203">Click **Create**.</span></span>
 
### <a name="creating-a-happyfox-test-user"></a><span data-ttu-id="2ee0b-204">HappyFox tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-204">Creating a HappyFox test user</span></span>

<span data-ttu-id="2ee0b-205">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-205">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span>
        
### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="2ee0b-206">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2ee0b-206">Assigning the Azure AD test user</span></span>

<span data-ttu-id="2ee0b-207">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés HappyFox Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-207">In this section, you enable Britta Simon to use Azure single sign-on by granting access to HappyFox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2ee0b-209">**Britta Simon hozzárendelése HappyFox, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="2ee0b-209">**To assign Britta Simon to HappyFox, perform the following steps:**</span></span>

1. <span data-ttu-id="2ee0b-210">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-210">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2ee0b-212">Az alkalmazások listában válassza ki a **HappyFox**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-212">In the applications list, select **HappyFox**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-happyfox-tutorial/tutorial_happyfox_app.png) 

3. <span data-ttu-id="2ee0b-214">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-214">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2ee0b-216">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-216">Click **Add** button.</span></span> <span data-ttu-id="2ee0b-217">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-217">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2ee0b-219">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-219">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="2ee0b-220">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-220">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2ee0b-221">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-221">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2ee0b-222">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2ee0b-222">Testing single sign-on</span></span>

<span data-ttu-id="2ee0b-223">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-223">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="2ee0b-224">Ha a hozzáférési panelen HappyFox csempére kattint, HappyFox alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-224">When you click the HappyFox tile in the Access Panel, you should get login page of HappyFox application.</span></span> <span data-ttu-id="2ee0b-225">Megjelenik a **"SAML"** a bejelentkezési lapon gombra.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-225">You should see the **‘SAML’** button on the sign-in page.</span></span>

    ![Beépülő modul](./media/active-directory-saas-happyfox-tutorial/saml.png) 

2. <span data-ttu-id="2ee0b-227">Kattintson a **"SAML"** gombra kattintva HappyFox az Azure AD-fiókkal bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="2ee0b-227">Click the **‘SAML’** button to log in to HappyFox using your Azure AD account.</span></span>

<span data-ttu-id="2ee0b-228">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2ee0b-228">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2ee0b-229">További források</span><span class="sxs-lookup"><span data-stu-id="2ee0b-229">Additional resources</span></span>

* [<span data-ttu-id="2ee0b-230">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="2ee0b-230">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2ee0b-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2ee0b-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-happyfox-tutorial/tutorial_general_203.png

