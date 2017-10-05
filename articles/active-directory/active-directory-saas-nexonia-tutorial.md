---
title: "Oktatóanyag: Azure Active Directoryval integrált Nexonia |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Nexonia között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a93b771a-9bc3-444a-bdc0-457f8bb7e780
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/1/2017
ms.author: jeedes
ms.openlocfilehash: 1370fa64c2ddc25d3121c567ceea4828b1e50921
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-nexonia"></a><span data-ttu-id="0a0f2-103">Oktatóanyag: Azure Active Directoryval integrált Nexonia</span><span class="sxs-lookup"><span data-stu-id="0a0f2-103">Tutorial: Azure Active Directory integration with Nexonia</span></span>

<span data-ttu-id="0a0f2-104">Ebben az oktatóanyagban elsajátíthatja Nexonia integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0a0f2-104">In this tutorial, you learn how to integrate Nexonia with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0a0f2-105">Nexonia integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-105">Integrating Nexonia with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="0a0f2-106">Megadhatja a Nexonia hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="0a0f2-106">You can control in Azure AD who has access to Nexonia</span></span>
- <span data-ttu-id="0a0f2-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Nexonia (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="0a0f2-107">You can enable your users to automatically get signed-on to Nexonia (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="0a0f2-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0a0f2-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="0a0f2-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="0a0f2-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0a0f2-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0a0f2-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0a0f2-111">Prerequisites</span></span>

<span data-ttu-id="0a0f2-112">Konfigurálása az Azure AD-integrációs Nexonia, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-112">To configure Azure AD integration with Nexonia, you need the following items:</span></span>

- <span data-ttu-id="0a0f2-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0a0f2-113">An Azure AD subscription</span></span>
- <span data-ttu-id="0a0f2-114">Egy Nexonia egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="0a0f2-114">A Nexonia single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0a0f2-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0a0f2-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0a0f2-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0a0f2-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0a0f2-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0a0f2-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-119">Scenario description</span></span>
<span data-ttu-id="0a0f2-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0a0f2-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0a0f2-122">A gyűjteményből Nexonia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-122">Adding Nexonia from the gallery</span></span>
2. <span data-ttu-id="0a0f2-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a0f2-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-nexonia-from-the-gallery"></a><span data-ttu-id="0a0f2-124">A gyűjteményből Nexonia hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-124">Adding Nexonia from the gallery</span></span>
<span data-ttu-id="0a0f2-125">Az Azure AD integrálása a Nexonia konfigurálásához kell hozzáadnia Nexonia a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-125">To configure the integration of Nexonia into Azure AD, you need to add Nexonia from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="0a0f2-126">**A gyűjteményből Nexonia hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-126">**To add Nexonia from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="0a0f2-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="0a0f2-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="0a0f2-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="0a0f2-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="0a0f2-134">Írja be a keresőmezőbe, **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-134">In the search box, type **Nexonia**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_search.png)

5. <span data-ttu-id="0a0f2-136">Az eredmények panelen válassza ki a **Nexonia**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-136">In the results panel, select **Nexonia**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0a0f2-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0a0f2-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0a0f2-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Nexonia</span><span class="sxs-lookup"><span data-stu-id="0a0f2-139">In this section, you configure and test Azure AD single sign-on with Nexonia based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="0a0f2-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Nexonia a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Nexonia is to a user in Azure AD.</span></span> <span data-ttu-id="0a0f2-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Nexonia közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-141">In other words, a link relationship between an Azure AD user and the related user in Nexonia needs to be established.</span></span>

<span data-ttu-id="0a0f2-142">Nexonia, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-142">In Nexonia, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="0a0f2-143">Az Azure AD egyszeri bejelentkezést a Nexonia tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-143">To configure and test Azure AD single sign-on with Nexonia, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="0a0f2-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="0a0f2-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0a0f2-146">**[Nexonia tesztfelhasználó létrehozása](#creating-a-nexonia-test-user)**  - való Britta Simon valami Nexonia, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-146">**[Creating a Nexonia test user](#creating-a-nexonia-test-user)** - to have a counterpart of Britta Simon in Nexonia that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="0a0f2-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0a0f2-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="0a0f2-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="0a0f2-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Nexonia alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Nexonia application.</span></span>

>[!Note]
><span data-ttu-id="0a0f2-151">Ha problémák vannak az integrációt, akkor tekintse meg a [hivatkozás](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) hibaelhárítási útmutató.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-151">If you have issues in the integration then refer this [link](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?WT.mc_id=UI_AAD_Enterprise_Apps_SupportOrTroubleshooting) for troubleshooting guide.</span></span> <span data-ttu-id="0a0f2-152">Ha még nem talált a megoldást, majd indítson a támogatási kérelem Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-152">If you still have not found the solution, then raise the support request from Azure portal.</span></span>

<span data-ttu-id="0a0f2-153">**Konfigurálása az Azure AD az egyszeri bejelentkezés Nexonia, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-153">**To configure Azure AD single sign-on with Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="0a0f2-154">Az Azure portálon a a **Nexonia** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-154">In the Azure portal, on the **Nexonia** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="0a0f2-156">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-156">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_samlbase.png)

3. <span data-ttu-id="0a0f2-158">Az a **Nexonia tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-158">On the **Nexonia Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_url.png)

    <span data-ttu-id="0a0f2-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span><span class="sxs-lookup"><span data-stu-id="0a0f2-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://system.nexonia.com/assistant/saml.do?orgCode=<organizationcode>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0a0f2-161">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-161">This value is not real.</span></span> <span data-ttu-id="0a0f2-162">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-162">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="0a0f2-163">Ügyfél [Nexonia támogatási csoport](https://nexonia.zendesk.com/hc/requests/new) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-163">Contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to get this value.</span></span> 


4. <span data-ttu-id="0a0f2-164">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-164">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_certificate.png) 

5. <span data-ttu-id="0a0f2-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0a0f2-168">A a **Nexonia konfigurációs** kattintson **konfigurálása Nexonia** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-168">On the **Nexonia Configuration** section, click **Configure Nexonia** to open **Configure sign-on** window.</span></span> <span data-ttu-id="0a0f2-169">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-169">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_configure.png) 

7. <span data-ttu-id="0a0f2-171">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [Nexonia támogatási csoport](https://nexonia.zendesk.com/hc/requests/new) és adja meg a következőket:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-171">To get SSO configured for your application, contact [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) and provide them with the following:</span></span>

    <span data-ttu-id="0a0f2-172">• A letöltött **tanúsítvány**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-172">• The downloaded **certificate**</span></span>

    <span data-ttu-id="0a0f2-173">• A **SAML entitás azonosítója**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-173">• The **SAML Entity ID**</span></span>

    <span data-ttu-id="0a0f2-174">• A **SAML-alapú egyszeri bejelentkezési szolgáltatás URL-címe**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-174">• The **SAML Single Sign-On Service URL**</span></span>

    <span data-ttu-id="0a0f2-175">• A **kijelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-175">• The **Sign-Out URL**</span></span>

> [!TIP]
> <span data-ttu-id="0a0f2-176">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="0a0f2-176">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="0a0f2-177">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-177">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="0a0f2-178">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0a0f2-178">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0a0f2-179">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-179">Creating an Azure AD test user</span></span>
<span data-ttu-id="0a0f2-180">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-180">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="0a0f2-182">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-182">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="0a0f2-183">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-183">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="0a0f2-185">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-185">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="0a0f2-187">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-187">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0a0f2-189">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="0a0f2-189">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-nexonia-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="0a0f2-191">a.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-191">a.</span></span> <span data-ttu-id="0a0f2-192">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-192">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="0a0f2-193">b.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-193">b.</span></span> <span data-ttu-id="0a0f2-194">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-194">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="0a0f2-195">c.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-195">c.</span></span> <span data-ttu-id="0a0f2-196">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-196">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="0a0f2-197">d.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-197">d.</span></span> <span data-ttu-id="0a0f2-198">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-198">Click **Create**.</span></span>
 
### <a name="creating-a-nexonia-test-user"></a><span data-ttu-id="0a0f2-199">Nexonia tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-199">Creating a Nexonia test user</span></span>

<span data-ttu-id="0a0f2-200">Ebben a szakaszban egy Nexonia Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-200">In this section, you create a user called Britta Simon in Nexonia.</span></span> <span data-ttu-id="0a0f2-201">Együttműködve [Nexonia támogatási csoport](https://nexonia.zendesk.com/hc/requests/new) a felhasználók hozzáadása a Nexonia platform.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-201">Work with [Nexonia support team](https://nexonia.zendesk.com/hc/requests/new) to add the users in the Nexonia platform.</span></span> <span data-ttu-id="0a0f2-202">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-202">Users must be created and activated before you use single sign-on.</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="0a0f2-203">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0a0f2-203">Assigning the Azure AD test user</span></span>

<span data-ttu-id="0a0f2-204">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Nexonia Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-204">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Nexonia.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0a0f2-206">**Britta Simon hozzárendelése Nexonia, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="0a0f2-206">**To assign Britta Simon to Nexonia, perform the following steps:**</span></span>

1. <span data-ttu-id="0a0f2-207">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-207">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0a0f2-209">Az alkalmazások listában válassza ki a **Nexonia**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-209">In the applications list, select **Nexonia**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-nexonia-tutorial/tutorial_nexonia_app.png) 

3. <span data-ttu-id="0a0f2-211">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-211">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="0a0f2-213">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-213">Click **Add** button.</span></span> <span data-ttu-id="0a0f2-214">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-214">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="0a0f2-216">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-216">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="0a0f2-217">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-217">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0a0f2-218">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-218">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="0a0f2-219">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0a0f2-219">Testing single sign-on</span></span>

<span data-ttu-id="0a0f2-220">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-220">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="0a0f2-221">Ha a hozzáférési panelen Nexonia csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Nexonia alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="0a0f2-221">When you click the Nexonia tile in the Access Panel, you should get automatically signed-on to your Nexonia application.</span></span>
<span data-ttu-id="0a0f2-222">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="0a0f2-222">For more information about the Access Panel, see [introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0a0f2-223">További források</span><span class="sxs-lookup"><span data-stu-id="0a0f2-223">Additional resources</span></span>

* [<span data-ttu-id="0a0f2-224">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="0a0f2-224">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0a0f2-225">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0a0f2-225">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-nexonia-tutorial/tutorial_general_203.png

