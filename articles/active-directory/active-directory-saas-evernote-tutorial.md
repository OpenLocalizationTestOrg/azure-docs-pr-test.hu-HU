---
title: "Oktatóanyag: Azure Active Directoryval integrált Evernote |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Evernote között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: be94152a84bbbeacb623d7dd8b540e3981931a8e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-evernote"></a><span data-ttu-id="05d20-103">Oktatóanyag: Azure Active Directoryval integrált Evernote</span><span class="sxs-lookup"><span data-stu-id="05d20-103">Tutorial: Azure Active Directory integration with Evernote</span></span>

<span data-ttu-id="05d20-104">Ebben az oktatóanyagban elsajátíthatja Evernote integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="05d20-104">In this tutorial, you learn how to integrate Evernote with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="05d20-105">Evernote integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="05d20-105">Integrating Evernote with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="05d20-106">Az Azure AD, aki hozzáfér Evernote szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="05d20-106">You can control in Azure AD who has access to Evernote.</span></span>
- <span data-ttu-id="05d20-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Evernote (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="05d20-107">You can enable your users to automatically get signed-on to Evernote (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="05d20-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="05d20-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="05d20-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="05d20-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05d20-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05d20-110">Prerequisites</span></span>

<span data-ttu-id="05d20-111">Konfigurálása az Azure AD-integrációs Evernote, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="05d20-111">To configure Azure AD integration with Evernote, you need the following items:</span></span>

- <span data-ttu-id="05d20-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="05d20-112">An Azure AD subscription</span></span>
- <span data-ttu-id="05d20-113">Egy Evernote egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="05d20-113">An Evernote single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="05d20-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="05d20-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="05d20-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="05d20-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="05d20-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="05d20-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="05d20-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="05d20-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="05d20-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="05d20-118">Scenario description</span></span>
<span data-ttu-id="05d20-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="05d20-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="05d20-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="05d20-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="05d20-121">A gyűjteményből Evernote hozzáadása</span><span class="sxs-lookup"><span data-stu-id="05d20-121">Adding Evernote from the gallery</span></span>
2. <span data-ttu-id="05d20-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="05d20-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-evernote-from-the-gallery"></a><span data-ttu-id="05d20-123">A gyűjteményből Evernote hozzáadása</span><span class="sxs-lookup"><span data-stu-id="05d20-123">Adding Evernote from the gallery</span></span>
<span data-ttu-id="05d20-124">Az Azure AD integrálása a Evernote konfigurálásához kell hozzáadnia Evernote a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="05d20-124">To configure the integration of Evernote into Azure AD, you need to add Evernote from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="05d20-125">**A gyűjteményből Evernote hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="05d20-125">**To add Evernote from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="05d20-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="05d20-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="05d20-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="05d20-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="05d20-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="05d20-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="05d20-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="05d20-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="05d20-133">Írja be a keresőmezőbe, **Evernote**, jelölje be **Evernote** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="05d20-133">In the search box, type **Evernote**, select **Evernote** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Evernote](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="05d20-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="05d20-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="05d20-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Evernote.</span><span class="sxs-lookup"><span data-stu-id="05d20-136">In this section, you configure and test Azure AD single sign-on with Evernote based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="05d20-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Evernote a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="05d20-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Evernote is to a user in Azure AD.</span></span> <span data-ttu-id="05d20-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Evernote közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="05d20-138">In other words, a link relationship between an Azure AD user and the related user in Evernote needs to be established.</span></span>

<span data-ttu-id="05d20-139">Evernote, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="05d20-139">In Evernote, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="05d20-140">Az Azure AD egyszeri bejelentkezést a Evernote tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="05d20-140">To configure and test Azure AD single sign-on with Evernote, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="05d20-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="05d20-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="05d20-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="05d20-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="05d20-143">**[Hozzon létre egy Evernote tesztfelhasználó](#create-an-evernote-test-user)**  - való Britta Simon valami Evernote, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="05d20-143">**[Create an Evernote test user](#create-an-evernote-test-user)** - to have a counterpart of Britta Simon in Evernote that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="05d20-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="05d20-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="05d20-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="05d20-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="05d20-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="05d20-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="05d20-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Evernote alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="05d20-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Evernote application.</span></span>

<span data-ttu-id="05d20-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Evernote, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="05d20-148">**To configure Azure AD single sign-on with Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="05d20-149">Az Azure portálon a a **Evernote** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="05d20-149">In the Azure portal, on the **Evernote** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="05d20-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="05d20-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_samlbase.png)

3. <span data-ttu-id="05d20-153">Az a **Evernote tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás által kezdeményezett IDP módban, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="05d20-153">On the **Evernote Domain and URLs** section, perform the following steps if you wish to configure the application in IDP initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url.png)

    <span data-ttu-id="05d20-155">Az a **azonosító** szövegmező, írja be az URL-cím:`https://www.evernote.com/saml2`</span><span class="sxs-lookup"><span data-stu-id="05d20-155">In the **Identifier** textbox, type the URL: `https://www.evernote.com/saml2`</span></span>

4. <span data-ttu-id="05d20-156">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="05d20-156">Check **Show advanced URL settings** and perform the following step if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Az egyszeri bejelentkezés információk Evernote tartomány és az URL-címek](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_url1.png)

    <span data-ttu-id="05d20-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-cím:`https://www.evernote.com/Login.action`</span><span class="sxs-lookup"><span data-stu-id="05d20-158">In the **Sign on URL** textbox, type the URL: `https://www.evernote.com/Login.action`</span></span>   

5. <span data-ttu-id="05d20-159">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="05d20-159">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certificate.png) 

6. <span data-ttu-id="05d20-161">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="05d20-161">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-evernote-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="05d20-163">A a **Evernote konfigurációs** kattintson **konfigurálása Evernote** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="05d20-163">On the **Evernote Configuration** section, click **Configure Evernote** to open **Configure sign-on** window.</span></span> <span data-ttu-id="05d20-164">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="05d20-164">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Evernote konfiguráció](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_configure.png) 

8. <span data-ttu-id="05d20-166">Egy másik webes böngészőablakban jelentkezzen be a Evernote vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="05d20-166">In a different web browser window, log into your Evernote company site as an administrator.</span></span>

9. <span data-ttu-id="05d20-167">Ugrás a **"Felügyeleti konzol"**</span><span class="sxs-lookup"><span data-stu-id="05d20-167">Go to **'Admin Console'**</span></span>

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

10. <span data-ttu-id="05d20-169">Az a **"Felügyeleti konzol"**, és **"Security"** válassza **"egyszeri bejelentkezéshez"**</span><span class="sxs-lookup"><span data-stu-id="05d20-169">From the **'Admin Console'**, go to **‘Security’** and select **‘Single Sign-On’**</span></span>

    ![SSO-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_sso.png)

11. <span data-ttu-id="05d20-171">Adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="05d20-171">Configure the following values:</span></span>

    ![Tanúsítvány-beállítás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_certx.png)
    
    <span data-ttu-id="05d20-173">a.</span><span class="sxs-lookup"><span data-stu-id="05d20-173">a.</span></span>  <span data-ttu-id="05d20-174">**Egyszeri bejelentkezés engedélyezése:** egyszeri bejelentkezés alapértelmezés szerint engedélyezve van (kattintson **tiltsa le az egyszeri bejelentkezés** SSO engedélyezése)</span><span class="sxs-lookup"><span data-stu-id="05d20-174">**Enable SSO:** SSO is enabled by default (Click **Disable Single Sign-on** to remove the SSO requirement)</span></span>

    <span data-ttu-id="05d20-175">b.</span><span class="sxs-lookup"><span data-stu-id="05d20-175">b.</span></span> <span data-ttu-id="05d20-176">Beillesztés **SAML-alapú egyszeri bejelentkezést szolgáltatás URL-címe** értéket, amely az Azure-portálról másolta a **SAML HTTP-kérelmek URL** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="05d20-176">Paste **SAML Single sign-on Service URL** value, which you have copied from the Azure portal into the **SAML HTTP Request URL** textbox.</span></span>

    <span data-ttu-id="05d20-177">c.</span><span class="sxs-lookup"><span data-stu-id="05d20-177">c.</span></span> <span data-ttu-id="05d20-178">Nyissa meg a letöltött tanúsítvány az Azure AD egy fájlt, és másolja a tartalmat, beleértve a "BEGIN tanúsítvány" és "END CERTIFICATE", és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="05d20-178">Open the downloaded certificate from Azure AD in a notepad and copy the content including "BEGIN CERTIFICATE" and "END CERTIFICATE" and paste it into the **X.509 Certificate** textbox.</span></span> 

    <span data-ttu-id="05d20-179">d.Click **módosítások mentése**</span><span class="sxs-lookup"><span data-stu-id="05d20-179">d.Click **Save Changes**</span></span>

> [!TIP]
> <span data-ttu-id="05d20-180">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="05d20-180">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="05d20-181">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="05d20-181">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="05d20-182">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="05d20-182">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="05d20-183">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="05d20-183">Create an Azure AD test user</span></span>

<span data-ttu-id="05d20-184">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="05d20-184">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="05d20-186">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="05d20-186">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="05d20-187">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="05d20-187">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-evernote-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="05d20-189">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="05d20-189">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-evernote-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="05d20-191">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="05d20-191">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-evernote-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="05d20-193">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="05d20-193">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-evernote-tutorial/create_aaduser_04.png)

    <span data-ttu-id="05d20-195">a.</span><span class="sxs-lookup"><span data-stu-id="05d20-195">a.</span></span> <span data-ttu-id="05d20-196">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="05d20-196">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="05d20-197">b.</span><span class="sxs-lookup"><span data-stu-id="05d20-197">b.</span></span> <span data-ttu-id="05d20-198">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="05d20-198">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="05d20-199">c.</span><span class="sxs-lookup"><span data-stu-id="05d20-199">c.</span></span> <span data-ttu-id="05d20-200">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="05d20-200">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="05d20-201">d.</span><span class="sxs-lookup"><span data-stu-id="05d20-201">d.</span></span> <span data-ttu-id="05d20-202">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="05d20-202">Click **Create**.</span></span>
 
### <a name="create-an-evernote-test-user"></a><span data-ttu-id="05d20-203">Hozzon létre egy Evernote tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="05d20-203">Create an Evernote test user</span></span>

<span data-ttu-id="05d20-204">Ahhoz, hogy az Azure AD-felhasználók Evernote bejelentkezni, akkor ki kell építenie Evernote be.</span><span class="sxs-lookup"><span data-stu-id="05d20-204">In order to enable Azure AD users to log into Evernote, they must be provisioned into Evernote.</span></span>  
<span data-ttu-id="05d20-205">Evernote, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="05d20-205">In the case of Evernote, provisioning is a manual task.</span></span>

<span data-ttu-id="05d20-206">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="05d20-206">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="05d20-207">Jelentkezzen be rendszergazdaként a Evernote vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="05d20-207">Log in to your Evernote company site as an administrator.</span></span>

2. <span data-ttu-id="05d20-208">Kattintson a **"Felügyeleti konzol"**.</span><span class="sxs-lookup"><span data-stu-id="05d20-208">Click the **'Admin Console'**.</span></span>

    ![Rendszergazda-konzol](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_adminconsole.png)

3. <span data-ttu-id="05d20-210">Az a **"Felügyeleti konzol"**, és **"Felhasználók hozzáadása"**.</span><span class="sxs-lookup"><span data-stu-id="05d20-210">From the **'Admin Console'**, go to **‘Add users’**.</span></span>

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0001.png)

4. <span data-ttu-id="05d20-212">**Adja hozzá a csoport tagjai** a a **E-mail** szövegmező, írja be a felhasználói fiók e-mail címét, majd kattintson **hívhat meg.**</span><span class="sxs-lookup"><span data-stu-id="05d20-212">**Add team members** in the **Email** textbox, type the email address of user account and click **Invite.**</span></span>

    ![Tesztfelhasználó-nevet](./media/active-directory-saas-evernote-tutorial/create_aaduser_0002.png)
    
5. <span data-ttu-id="05d20-214">Meghívót küldött, miután az Azure Active Directory fióktulajdonos a meghívó elfogadásának e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="05d20-214">After invitation is sent, the Azure Active Directory account holder will receive an email to accept the invitation.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="05d20-215">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="05d20-215">Assign the Azure AD test user</span></span>

<span data-ttu-id="05d20-216">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Evernote Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="05d20-216">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Evernote.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="05d20-218">**Britta Simon hozzárendelése Evernote, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="05d20-218">**To assign Britta Simon to Evernote, perform the following steps:**</span></span>

1. <span data-ttu-id="05d20-219">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="05d20-219">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="05d20-221">Az alkalmazások listában válassza ki a **Evernote**.</span><span class="sxs-lookup"><span data-stu-id="05d20-221">In the applications list, select **Evernote**.</span></span>

    ![Az alkalmazások listáját a Evernote hivatkozás](./media/active-directory-saas-evernote-tutorial/tutorial_evernote_app.png)  

3. <span data-ttu-id="05d20-223">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="05d20-223">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="05d20-225">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="05d20-225">Click **Add** button.</span></span> <span data-ttu-id="05d20-226">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05d20-226">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="05d20-228">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="05d20-228">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="05d20-229">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05d20-229">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="05d20-230">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="05d20-230">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="05d20-231">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="05d20-231">Test single sign-on</span></span>

<span data-ttu-id="05d20-232">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="05d20-232">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="05d20-233">Ha a hozzáférési panelen Evernote csempére kattint, akkor kell beolvasása bejelentkezett az Evernote alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="05d20-233">When you click the Evernote tile in the Access Panel, you should get signed-on to your Evernote application.</span></span> <span data-ttu-id="05d20-234">Akkor lesz naplózása szervezeti fiók azonban akkor kell jelentkezzen be a személyes fiókjával.</span><span class="sxs-lookup"><span data-stu-id="05d20-234">You'll be logging in as an Organization account but then need to log in with your personal account.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="05d20-235">További források</span><span class="sxs-lookup"><span data-stu-id="05d20-235">Additional resources</span></span>

* [<span data-ttu-id="05d20-236">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="05d20-236">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="05d20-237">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="05d20-237">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-evernote-tutorial/tutorial_general_203.png

