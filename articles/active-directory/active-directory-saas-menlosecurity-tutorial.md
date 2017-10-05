---
title: "Oktatóanyag: Azure Active Directoryval integrált Menlo biztonsági |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Menlo biztonsági között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9e63fe6b-0ad0-405d-9e41-6a1a40a41df8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: jeedes
ms.openlocfilehash: 75366abafa551d21630b0edddb65db23b9ea9d42
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-menlo-security"></a><span data-ttu-id="a0787-103">Oktatóanyag: Azure Active Directoryval integrált Menlo biztonsági</span><span class="sxs-lookup"><span data-stu-id="a0787-103">Tutorial: Azure Active Directory integration with Menlo Security</span></span>

<span data-ttu-id="a0787-104">Ebben az oktatóanyagban elsajátíthatja Menlo biztonsági integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a0787-104">In this tutorial, you learn how to integrate Menlo Security with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a0787-105">Menlo biztonsági integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a0787-105">Integrating Menlo Security with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a0787-106">Megadhatja a Menlo biztonsági hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a0787-106">You can control in Azure AD who has access to Menlo Security</span></span>
- <span data-ttu-id="a0787-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Menlo biztonsági (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="a0787-107">You can enable your users to automatically get signed-on to Menlo Security (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a0787-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a0787-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a0787-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="a0787-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="a0787-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a0787-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a0787-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a0787-111">Prerequisites</span></span>

<span data-ttu-id="a0787-112">Az Azure AD-integrációs Menlo biztonsági konfigurálni, kell a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="a0787-112">To configure Azure AD integration with Menlo Security, you need the following items:</span></span>

- <span data-ttu-id="a0787-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a0787-113">An Azure AD subscription</span></span>
- <span data-ttu-id="a0787-114">Egy Menlo biztonsági egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a0787-114">A Menlo Security single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a0787-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a0787-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a0787-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a0787-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a0787-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a0787-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a0787-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a0787-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a0787-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a0787-119">Scenario description</span></span>
<span data-ttu-id="a0787-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a0787-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a0787-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a0787-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a0787-122">A gyűjteményből Menlo adatvédelme</span><span class="sxs-lookup"><span data-stu-id="a0787-122">Adding Menlo Security from the gallery</span></span>
2. <span data-ttu-id="a0787-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a0787-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-menlo-security-from-the-gallery"></a><span data-ttu-id="a0787-124">A gyűjteményből Menlo adatvédelme</span><span class="sxs-lookup"><span data-stu-id="a0787-124">Adding Menlo Security from the gallery</span></span>
<span data-ttu-id="a0787-125">Az Azure AD integrálása a Menlo biztonsági konfigurálásához kell hozzáadnia Menlo biztonsági a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a0787-125">To configure the integration of Menlo Security into Azure AD, you need to add Menlo Security from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a0787-126">**A gyűjteményből Menlo biztonsági hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a0787-126">**To add Menlo Security from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a0787-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a0787-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a0787-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a0787-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a0787-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a0787-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a0787-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a0787-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a0787-134">Írja be a keresőmezőbe, **Menlo biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="a0787-134">In the search box, type **Menlo Security**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_search.png)

5. <span data-ttu-id="a0787-136">Az eredmények panelen válassza ki a **Menlo biztonsági**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a0787-136">In the results panel, select **Menlo Security**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a0787-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a0787-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a0787-139">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Menlo biztonság</span><span class="sxs-lookup"><span data-stu-id="a0787-139">In this section, you configure and test Azure AD single sign-on with Menlo Security based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a0787-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Menlo biztonsági a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a0787-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Menlo Security is to a user in Azure AD.</span></span> <span data-ttu-id="a0787-141">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Menlo biztonsági közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a0787-141">In other words, a link relationship between an Azure AD user and the related user in Menlo Security needs to be established.</span></span>

<span data-ttu-id="a0787-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Menlo biztonsági.</span><span class="sxs-lookup"><span data-stu-id="a0787-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Menlo Security.</span></span>

<span data-ttu-id="a0787-143">Az Azure AD az egyszeri bejelentkezés Menlo biztonsági tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a0787-143">To configure and test Azure AD single sign-on with Menlo Security, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a0787-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a0787-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a0787-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a0787-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a0787-146">**[Menlo biztonsági tesztfelhasználó létrehozása](#creating-a-menlo-security-test-user)**  - való egy megfelelője a Britta Simon Menlo biztonsági, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="a0787-146">**[Creating a Menlo Security test user](#creating-a-menlo-security-test-user)** - to have a counterpart of Britta Simon in Menlo Security that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a0787-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a0787-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a0787-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a0787-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a0787-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a0787-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a0787-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és az Menlo biztonsági alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a0787-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Menlo Security application.</span></span>

<span data-ttu-id="a0787-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Menlo biztonsági, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a0787-151">**To configure Azure AD single sign-on with Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="a0787-152">Az Azure portálon a a **Menlo biztonsági** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a0787-152">In the Azure portal, on the **Menlo Security** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a0787-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a0787-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_samlbase.png)

3. <span data-ttu-id="a0787-156">Az a **Menlo biztonsági tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a0787-156">On the **Menlo Security Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_url.png)

    <span data-ttu-id="a0787-158">a.</span><span class="sxs-lookup"><span data-stu-id="a0787-158">a.</span></span> <span data-ttu-id="a0787-159">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.menlosecurity.com/account/login`</span><span class="sxs-lookup"><span data-stu-id="a0787-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/account/login`</span></span>

    <span data-ttu-id="a0787-160">b.</span><span class="sxs-lookup"><span data-stu-id="a0787-160">b.</span></span> <span data-ttu-id="a0787-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="a0787-161">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.menlosecurity.com/safeview-auth-server/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a0787-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="a0787-162">These values are not the real.</span></span> <span data-ttu-id="a0787-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a0787-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="a0787-164">Ügyfél [Menlo biztonsági ügyfél-támogatási csoport](https://www.menlosecurity.com/menlo-contact) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a0787-164">Contact [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to get these values.</span></span> 
 
4. <span data-ttu-id="a0787-165">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a0787-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the Certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_certificate.png) 

5. <span data-ttu-id="a0787-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0787-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a0787-169">A a **Menlo biztonsági konfiguráció** kattintson **Menlo biztonságának konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a0787-169">On the **Menlo Security Configuration** section, click **Configure Menlo Security** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a0787-170">Másolás a **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a0787-170">Copy the **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_configure.png) 

7. <span data-ttu-id="a0787-172">Egyszeri bejelentkezés konfigurálása **Menlo biztonsági** ügyféloldali, jelentkezzen be a **Menlo biztonsági** webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a0787-172">To configure single sign-on on **Menlo Security** side, login to the **Menlo Security** website as an administrator.</span></span>

8. <span data-ttu-id="a0787-173">A **beállítások** lépjen **hitelesítési** , és végezze el a következő műveleteket:</span><span class="sxs-lookup"><span data-stu-id="a0787-173">Under **Settings** go to **Authentication** and perform following actions:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/menlo_user_setup.png)

    <span data-ttu-id="a0787-175">a.</span><span class="sxs-lookup"><span data-stu-id="a0787-175">a.</span></span> <span data-ttu-id="a0787-176">A jelölőnégyzet osztásjelek **használatával SAML-alapú hitelesítés engedélyezése felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="a0787-176">Tick the checkbox **Enable user authentication using SAML**.</span></span>

    <span data-ttu-id="a0787-177">b.</span><span class="sxs-lookup"><span data-stu-id="a0787-177">b.</span></span> <span data-ttu-id="a0787-178">Válassza ki **külső hozzáférés engedélyezése** való **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a0787-178">Select **Allow External Access** to **Yes**.</span></span>

    <span data-ttu-id="a0787-179">c.</span><span class="sxs-lookup"><span data-stu-id="a0787-179">c.</span></span> <span data-ttu-id="a0787-180">A **SAML-szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a0787-180">Under **SAML Provider**, select **Azure Active Directory**.</span></span>

    <span data-ttu-id="a0787-181">d.</span><span class="sxs-lookup"><span data-stu-id="a0787-181">d.</span></span> <span data-ttu-id="a0787-182">**SAML 2.0 végpont** : illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a0787-182">**SAML 2.0 Endpoint** : Paste the **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a0787-183">e.</span><span class="sxs-lookup"><span data-stu-id="a0787-183">e.</span></span> <span data-ttu-id="a0787-184">**Szolgáltatás azonosítója (kibocsátó)** : illessze be a **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a0787-184">**Service Identifier (Issuer)** : Paste the **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a0787-185">f.</span><span class="sxs-lookup"><span data-stu-id="a0787-185">f.</span></span> <span data-ttu-id="a0787-186">**X.509 tanúsítvány** : Nyissa meg a **tanúsítvány (Base64)** a Jegyzettömbben az Azure-portálról letöltött, és illessze be ezt a jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="a0787-186">**X.509 Certificate** : Open the **Certificate (Base64)** downloaded from the Azure Portal in notepad and paste it in this box.</span></span>

    <span data-ttu-id="a0787-187">g.</span><span class="sxs-lookup"><span data-stu-id="a0787-187">g.</span></span> <span data-ttu-id="a0787-188">Kattintson a **mentése** menti a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="a0787-188">Click **Save** to save the settings.</span></span>

> [!TIP]
> <span data-ttu-id="a0787-189">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a0787-189">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a0787-190">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a0787-190">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a0787-191">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a0787-191">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a0787-192">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0787-192">Creating an Azure AD test user</span></span>
<span data-ttu-id="a0787-193">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a0787-193">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a0787-195">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a0787-195">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a0787-196">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a0787-196">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a0787-198">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a0787-198">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a0787-200">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a0787-200">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a0787-202">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a0787-202">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-menlosecurity-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a0787-204">a.</span><span class="sxs-lookup"><span data-stu-id="a0787-204">a.</span></span> <span data-ttu-id="a0787-205">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a0787-205">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a0787-206">b.</span><span class="sxs-lookup"><span data-stu-id="a0787-206">b.</span></span> <span data-ttu-id="a0787-207">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a0787-207">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a0787-208">c.</span><span class="sxs-lookup"><span data-stu-id="a0787-208">c.</span></span> <span data-ttu-id="a0787-209">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a0787-209">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a0787-210">d.</span><span class="sxs-lookup"><span data-stu-id="a0787-210">d.</span></span> <span data-ttu-id="a0787-211">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a0787-211">Click **Create**.</span></span>
 
### <a name="creating-a-menlo-security-test-user"></a><span data-ttu-id="a0787-212">Menlo biztonsági tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a0787-212">Creating a Menlo Security test user</span></span>
 
<span data-ttu-id="a0787-213">Ebben a szakaszban egy Menlo biztonsági Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a0787-213">In this section, you create a user called Britta Simon in Menlo Security.</span></span> <span data-ttu-id="a0787-214">Együttműködve [Menlo biztonsági ügyfél-támogatási csoport](https://www.menlosecurity.com/menlo-contact) a felhasználók hozzáadása a Menlo biztonsági platform.</span><span class="sxs-lookup"><span data-stu-id="a0787-214">Work with [Menlo Security Client support team](https://www.menlosecurity.com/menlo-contact) to add the users in the Menlo Security platform.</span></span> <span data-ttu-id="a0787-215">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="a0787-215">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a0787-216">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a0787-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a0787-217">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Menlo biztonsági Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a0787-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Menlo Security.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a0787-219">**Britta Simon hozzárendelése Menlo biztonsági, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a0787-219">**To assign Britta Simon to Menlo Security, perform the following steps:**</span></span>

1. <span data-ttu-id="a0787-220">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a0787-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a0787-222">Az alkalmazások listában válassza ki a **Menlo biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="a0787-222">In the applications list, select **Menlo Security**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-menlosecurity-tutorial/tutorial_menlosecurity_app.png) 

3. <span data-ttu-id="a0787-224">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a0787-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a0787-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a0787-226">Click **Add** button.</span></span> <span data-ttu-id="a0787-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a0787-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a0787-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a0787-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a0787-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a0787-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a0787-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a0787-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a0787-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a0787-232">Testing single sign-on</span></span>

<span data-ttu-id="a0787-233">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a0787-233">In this section, you test your Azure AD single sign-on configuration.</span></span>

<span data-ttu-id="a0787-234">Nyisson meg egy böngészőablakot elindítható egy új hitelesítési "InPrivate" vagy "Incognito" módban.</span><span class="sxs-lookup"><span data-stu-id="a0787-234">Open a browser window in an "InPrivate" or "Incognito" mode to trigger a new authentication.</span></span>  <span data-ttu-id="a0787-235">Az Internet Explorerben használja a Ctrl + Shift + P.</span><span class="sxs-lookup"><span data-stu-id="a0787-235">In Internet Explorer, use Ctrl+Shift+P.</span></span>  <span data-ttu-id="a0787-236">A Chrome-ban használja a Ctrl + Shift + N.</span><span class="sxs-lookup"><span data-stu-id="a0787-236">In Chrome, use Ctrl+Shift+N.</span></span>  <span data-ttu-id="a0787-237">A titkos böngészési ablakban keresse meg a védett erőforrásokhoz, és hajtsa végre az Azure AD bejelentkezési.</span><span class="sxs-lookup"><span data-stu-id="a0787-237">In the private browsing window, browse to a protected resource and perform an Azure AD login.</span></span>  <span data-ttu-id="a0787-238">Sikeres bejelentkezés esetén megnyílik a kért webhely elkülönítési munkamenetekben.</span><span class="sxs-lookup"><span data-stu-id="a0787-238">Upon successful login, you will be taken to the requested site in an isolation session.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a0787-239">További források</span><span class="sxs-lookup"><span data-stu-id="a0787-239">Additional resources</span></span>

* [<span data-ttu-id="a0787-240">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a0787-240">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a0787-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a0787-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-menlosecurity-tutorial/tutorial_general_203.png

