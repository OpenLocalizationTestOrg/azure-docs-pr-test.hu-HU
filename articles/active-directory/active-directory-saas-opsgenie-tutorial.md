---
title: "Oktatóanyag: Azure Active Directoryval integrált OpsGenie |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és OpsGenie között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 41b59b22-a61d-4fe6-ab0d-6c3991d1375f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: ce63726d2406d2f1415d29786f0ef92ca95b9b90
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-opsgenie"></a><span data-ttu-id="9d0e8-103">Oktatóanyag: Azure Active Directoryval integrált OpsGenie</span><span class="sxs-lookup"><span data-stu-id="9d0e8-103">Tutorial: Azure Active Directory integration with OpsGenie</span></span>

<span data-ttu-id="9d0e8-104">Ebben az oktatóanyagban elsajátíthatja OpsGenie integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9d0e8-104">In this tutorial, you learn how to integrate OpsGenie with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9d0e8-105">OpsGenie integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-105">Integrating OpsGenie with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9d0e8-106">Megadhatja a OpsGenie hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9d0e8-106">You can control in Azure AD who has access to OpsGenie</span></span>
- <span data-ttu-id="9d0e8-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett OpsGenie (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9d0e8-107">You can enable your users to automatically get signed-on to OpsGenie (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9d0e8-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9d0e8-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9d0e8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9d0e8-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9d0e8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9d0e8-110">Prerequisites</span></span>

<span data-ttu-id="9d0e8-111">Konfigurálása az Azure AD-integrációs OpsGenie, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-111">To configure Azure AD integration with OpsGenie, you need the following items:</span></span>

- <span data-ttu-id="9d0e8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9d0e8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9d0e8-113">Egy OpsGenie egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9d0e8-113">A OpsGenie single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9d0e8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9d0e8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9d0e8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9d0e8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9d0e8-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9d0e8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-118">Scenario description</span></span>
<span data-ttu-id="9d0e8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9d0e8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9d0e8-121">A gyűjteményből OpsGenie hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-121">Adding OpsGenie from the gallery</span></span>
2. <span data-ttu-id="9d0e8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9d0e8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-opsgenie-from-the-gallery"></a><span data-ttu-id="9d0e8-123">A gyűjteményből OpsGenie hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-123">Adding OpsGenie from the gallery</span></span>
<span data-ttu-id="9d0e8-124">Az Azure AD integrálása a OpsGenie konfigurálásához kell hozzáadnia OpsGenie a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-124">To configure the integration of OpsGenie into Azure AD, you need to add OpsGenie from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9d0e8-125">**A gyűjteményből OpsGenie hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d0e8-125">**To add OpsGenie from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9d0e8-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9d0e8-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9d0e8-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9d0e8-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9d0e8-133">Írja be a keresőmezőbe, **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-133">In the search box, type **OpsGenie**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_search.png)

5. <span data-ttu-id="9d0e8-135">Az eredmények panelen válassza ki a **OpsGenie**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-135">In the results panel, select **OpsGenie**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9d0e8-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9d0e8-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9d0e8-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-138">In this section, you configure and test Azure AD single sign-on with OpsGenie based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9d0e8-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó OpsGenie a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-139">For single sign-on to work, Azure AD needs to know what the counterpart user in OpsGenie is to a user in Azure AD.</span></span> <span data-ttu-id="9d0e8-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a OpsGenie közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-140">In other words, a link relationship between an Azure AD user and the related user in OpsGenie needs to be established.</span></span>

<span data-ttu-id="9d0e8-141">OpsGenie, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-141">In OpsGenie, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9d0e8-142">Az Azure AD egyszeri bejelentkezést a OpsGenie tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-142">To configure and test Azure AD single sign-on with OpsGenie, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9d0e8-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9d0e8-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9d0e8-145">**[OpsGenie tesztfelhasználó létrehozása](#creating-a-opsgenie-test-user)**  - való Britta Simon valami OpsGenie, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-145">**[Creating a OpsGenie test user](#creating-a-opsgenie-test-user)** - to have a counterpart of Britta Simon in OpsGenie that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9d0e8-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9d0e8-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="9d0e8-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="9d0e8-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az OpsGenie alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your OpsGenie application.</span></span>

<span data-ttu-id="9d0e8-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés OpsGenie, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d0e8-150">**To configure Azure AD single sign-on with OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="9d0e8-151">Az Azure portálon a a **OpsGenie** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-151">In the Azure portal, on the **OpsGenie** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9d0e8-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_samlbase.png)

3. <span data-ttu-id="9d0e8-155">Az a **OpsGenie tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-155">On the **OpsGenie Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_url.png)

    <span data-ttu-id="9d0e8-157">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://app.opsgenie.com/auth/login`</span><span class="sxs-lookup"><span data-stu-id="9d0e8-157">In the **Sign-on URL** textbox, type the URL: `https://app.opsgenie.com/auth/login`</span></span>

4. <span data-ttu-id="9d0e8-158">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_certificate.png) 

5. <span data-ttu-id="9d0e8-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="9d0e8-162">A a **OpsGenie konfigurációs** kattintson **konfigurálása OpsGenie** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-162">On the **OpsGenie Configuration** section, click **Configure OpsGenie** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9d0e8-163">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9d0e8-163">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_configure.png) 

7. <span data-ttu-id="9d0e8-165">Nyissa meg egy másik böngészőben példánya, és ezután jelentkezzen be rendszergazdaként OpsGenie.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-165">Open another browser instance, and then log-in to OpsGenie as an administrator.</span></span>

8. <span data-ttu-id="9d0e8-166">Kattintson a **beállítások**, majd kattintson a **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-166">Click **Settings**, and then click the **Single Sign On** tab.</span></span>
   
    ![OpsGenie egyszeri bejelentkezés](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_06.png)

9. <span data-ttu-id="9d0e8-168">Egyszeri bejelentkezés engedélyezéséhez jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-168">To enable SSO, select **Enabled**.</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_07.png) 

10. <span data-ttu-id="9d0e8-170">Az a **szolgáltató** területen kattintson a **Azure Active Directory** fülre.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-170">In the **Provider** section, click the **Azure Active Directory** tab.</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_08.png) 

11. <span data-ttu-id="9d0e8-172">Az Azure Active Directory párbeszédpanel oldalon hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-172">On the Azure Active Directory dialog page, perform the following steps:</span></span>
   
    ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_09.png)
    
    <span data-ttu-id="9d0e8-174">a.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-174">a.</span></span> <span data-ttu-id="9d0e8-175">Beillesztés **egyszeri bejelentkezési szolgáltatás URL-cím**, amely az Azure-portálról másolta a **SAML 2.0 végpont** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-175">Paste **Single Sign On Service URL**, which you have copied from the Azure portal into the **SAML 2.0 Endpoint** textbox.</span></span>
    
    <span data-ttu-id="9d0e8-176">b.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-176">b.</span></span> <span data-ttu-id="9d0e8-177">Nyissa meg a letöltött base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **X.500 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-177">Open your downloaded base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it into the **X.500 Certificate** textbox.</span></span>
    
    <span data-ttu-id="9d0e8-178">c.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-178">c.</span></span> <span data-ttu-id="9d0e8-179">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-179">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="9d0e8-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9d0e8-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9d0e8-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9d0e8-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9d0e8-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9d0e8-183">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-183">Creating an Azure AD test user</span></span>
<span data-ttu-id="9d0e8-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9d0e8-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d0e8-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9d0e8-187">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-187">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9d0e8-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-189">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9d0e8-191">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-191">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9d0e8-193">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-193">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-opsgenie-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9d0e8-195">a.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-195">a.</span></span> <span data-ttu-id="9d0e8-196">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-196">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9d0e8-197">b.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-197">b.</span></span> <span data-ttu-id="9d0e8-198">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-198">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9d0e8-199">c.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-199">c.</span></span> <span data-ttu-id="9d0e8-200">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-200">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9d0e8-201">d.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-201">d.</span></span> <span data-ttu-id="9d0e8-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-202">Click **Create**.</span></span>
 
### <a name="creating-a-opsgenie-test-user"></a><span data-ttu-id="9d0e8-203">OpsGenie tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-203">Creating a OpsGenie test user</span></span>

<span data-ttu-id="9d0e8-204">Ez a szakasz célja OpsGenie Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-204">The objective of this section is to create a user called Britta Simon in OpsGenie.</span></span> 

1. <span data-ttu-id="9d0e8-205">A webböngésző ablakának bejelentkezni a OpsGenie Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-205">In a web browser window, log into your OpsGenie tenant as an administrator.</span></span>

2. <span data-ttu-id="9d0e8-206">Keresse meg a felhasználók listában kattintva **felhasználói** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-206">Navigate to Users list by clicking **User** in left panel.</span></span>
   
   ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_10.png) 

3. <span data-ttu-id="9d0e8-208">Kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-208">Click **Add User**.</span></span>

4. <span data-ttu-id="9d0e8-209">Az a **felhasználó hozzáadása** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="9d0e8-209">On the **Add User** dialog, perform the following steps:</span></span>
   
   ![OpsGenie beállítások](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_11.png)
   
   <span data-ttu-id="9d0e8-211">a.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-211">a.</span></span> <span data-ttu-id="9d0e8-212">Az a **E-mail** szövegmezőhöz BrittaSimon az e-mail cím típusú venni az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-212">In the **Email** textbox, type the email address of BrittaSimon addressed in Azure Active Directory.</span></span>
   
   <span data-ttu-id="9d0e8-213">b.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-213">b.</span></span> <span data-ttu-id="9d0e8-214">Az a **teljes nevét** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-214">In the **Full Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="9d0e8-215">c.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-215">c.</span></span> <span data-ttu-id="9d0e8-216">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-216">Click **Save**.</span></span> 

>[!NOTE]
><span data-ttu-id="9d0e8-217">Britta saját profil beállításával kapcsolatos utasításokat tartalmazó e-mailt kap.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-217">Britta gets an email with instructions for setting up her profile.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9d0e8-218">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9d0e8-218">Assigning the Azure AD test user</span></span>

<span data-ttu-id="9d0e8-219">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés OpsGenie Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-219">In this section, you enable Britta Simon to use Azure single sign-on by granting access to OpsGenie.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9d0e8-221">**Britta Simon hozzárendelése OpsGenie, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9d0e8-221">**To assign Britta Simon to OpsGenie, perform the following steps:**</span></span>

1. <span data-ttu-id="9d0e8-222">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-222">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9d0e8-224">Az alkalmazások listában válassza ki a **OpsGenie**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-224">In the applications list, select **OpsGenie**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-opsgenie-tutorial/tutorial_opsgenie_app.png) 

3. <span data-ttu-id="9d0e8-226">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-226">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9d0e8-228">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-228">Click **Add** button.</span></span> <span data-ttu-id="9d0e8-229">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-229">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9d0e8-231">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-231">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9d0e8-232">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-232">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9d0e8-233">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-233">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="9d0e8-234">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9d0e8-234">Testing single sign-on</span></span>

<span data-ttu-id="9d0e8-235">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-235">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="9d0e8-236">Ha a hozzáférési panelen OpsGenie csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az OpsGenie alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9d0e8-236">When you click the OpsGenie tile in the Access Panel, you should get automatically signed-on to your OpsGenie application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d0e8-237">További források</span><span class="sxs-lookup"><span data-stu-id="9d0e8-237">Additional resources</span></span>

* [<span data-ttu-id="9d0e8-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9d0e8-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9d0e8-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9d0e8-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-opsgenie-tutorial/tutorial_general_203.png

