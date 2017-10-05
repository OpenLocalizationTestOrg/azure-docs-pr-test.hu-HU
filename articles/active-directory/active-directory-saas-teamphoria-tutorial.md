---
title: "Oktatóanyag: Azure Active Directoryval integrált Teamphoria |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Teamphoria között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: 2a35efb04d7fe22abc6894c149caf090666ce016
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="c5a07-103">Oktatóanyag: Azure Active Directoryval integrált Teamphoria</span><span class="sxs-lookup"><span data-stu-id="c5a07-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="c5a07-104">Ebben az oktatóanyagban elsajátíthatja Teamphoria integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="c5a07-104">In this tutorial, you learn how to integrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="c5a07-105">Teamphoria integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="c5a07-105">Integrating Teamphoria with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="c5a07-106">Megadhatja a Teamphoria hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="c5a07-106">You can control in Azure AD who has access to Teamphoria</span></span>
- <span data-ttu-id="c5a07-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Teamphoria (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="c5a07-107">You can enable your users to automatically get signed-on to Teamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="c5a07-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="c5a07-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="c5a07-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="c5a07-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

To enable single sign-on with Teamphoria, it must be configured to use Azure Active Directory as an identity provider. This guide provides information and tips on how to perform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in the new Azure portal, and we’d love to hear your thoughts. Use the Feedback ? button at the top of the portal to provide feedback. The older guide for using the [Azure classic portal](https://manage.windowsazure.com) to configure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="c5a07-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c5a07-110">Prerequisites</span></span>

<span data-ttu-id="c5a07-111">Konfigurálása az Azure AD-integrációs Teamphoria, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="c5a07-111">To configure Azure AD integration with Teamphoria, you need the following items:</span></span>

- <span data-ttu-id="c5a07-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="c5a07-112">An Azure AD subscription</span></span>
- <span data-ttu-id="c5a07-113">Egy Teamphoria egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="c5a07-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="c5a07-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="c5a07-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="c5a07-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="c5a07-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="c5a07-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="c5a07-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="c5a07-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="c5a07-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="c5a07-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="c5a07-118">Scenario description</span></span>
<span data-ttu-id="c5a07-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="c5a07-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="c5a07-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="c5a07-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="c5a07-121">A gyűjteményből Teamphoria hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c5a07-121">Adding Teamphoria from the gallery</span></span>
2. <span data-ttu-id="c5a07-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c5a07-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-the-gallery"></a><span data-ttu-id="c5a07-123">A gyűjteményből Teamphoria hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c5a07-123">Adding Teamphoria from the gallery</span></span>
<span data-ttu-id="c5a07-124">Az Azure AD integrálása a Teamphoria konfigurálásához kell hozzáadnia Teamphoria a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="c5a07-124">To configure the integration of Teamphoria into Azure AD, you need to add Teamphoria from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="c5a07-125">**A gyűjteményből Teamphoria hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c5a07-125">**To add Teamphoria from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="c5a07-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c5a07-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="c5a07-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="c5a07-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="c5a07-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="c5a07-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="c5a07-133">Írja be a keresőmezőbe, **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-133">In the search box, type **Teamphoria**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="c5a07-135">Az eredmények panelen válassza ki a **Teamphoria**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c5a07-135">In the results panel, select **Teamphoria**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="c5a07-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="c5a07-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="c5a07-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="c5a07-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="c5a07-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Teamphoria a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="c5a07-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Teamphoria is to a user in Azure AD.</span></span> <span data-ttu-id="c5a07-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Teamphoria közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c5a07-140">In other words, a link relationship between an Azure AD user and the related user in Teamphoria needs to be established.</span></span>

<span data-ttu-id="c5a07-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Teamphoria a.</span><span class="sxs-lookup"><span data-stu-id="c5a07-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Teamphoria.</span></span>

<span data-ttu-id="c5a07-142">Az Azure AD egyszeri bejelentkezést a Teamphoria tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="c5a07-142">To configure and test Azure AD single sign-on with Teamphoria, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="c5a07-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="c5a07-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="c5a07-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="c5a07-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="c5a07-145">**[Teamphoria tesztfelhasználó létrehozása](#creating-a-teamphoria-test-user)**  - való Britta Simon valami Teamphoria, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="c5a07-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - to have a counterpart of Britta Simon in Teamphoria that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="c5a07-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c5a07-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="c5a07-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="c5a07-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="c5a07-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c5a07-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="c5a07-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Teamphoria alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c5a07-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="c5a07-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Teamphoria, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c5a07-150">**To configure Azure AD single sign-on with Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="c5a07-151">Az Azure felügyeleti portálján a a **Teamphoria** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-151">In the Azure Management portal, on the **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="c5a07-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="c5a07-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="c5a07-155">Az a **Teamphoria tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c5a07-155">On the **Teamphoria Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="c5a07-157">a.</span><span class="sxs-lookup"><span data-stu-id="c5a07-157">a.</span></span> <span data-ttu-id="c5a07-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az URL-CÍMÉT a következő mintát:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="c5a07-158">In the **Sign-on URL** textbox, type the URL using the following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>    

    > [!NOTE] 
    > <span data-ttu-id="c5a07-159">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="c5a07-159">Please note that these are not the real values.</span></span> <span data-ttu-id="c5a07-160">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c5a07-160">You have to update these values with the actual Sign-On URL.</span></span> <span data-ttu-id="c5a07-161">Ügyfél [Teamphoria ügyfél-támogatási csoport](https://www.teamphoria.com/) lekérni a bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c5a07-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) to get the Sign-on URL.</span></span> 

4. <span data-ttu-id="c5a07-162">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c5a07-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="c5a07-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a07-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="c5a07-166">A a **Teamphoria konfigurációs** kattintson **konfigurálása Teamphoria** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="c5a07-166">On the **Teamphoria Configuration** section, click **Configure Teamphoria** to open **Configure sign-on** window.</span></span> <span data-ttu-id="c5a07-167">Másolás a **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="c5a07-167">Copy the **SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="c5a07-169">Egyszeri bejelentkezés konfigurálása **Teamphoria** oldalán, jelentkezzen be rendszergazdaként a Teamphoria alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c5a07-169">To configure single sign-on on **Teamphoria** side, Login to your Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="c5a07-170">Ugrás a **rendszergazdai beállítások** lehetőséget a bal oldali eszköztáron és a a a konfigurálása lapon kattintson a **egyetlen SIGN-ON** az SSO konfigurációs ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c5a07-170">Go to **ADMIN SETTINGS** option in the left toolbar and under the the Configure Tab click on **SINGLE SIGN-ON** to open the SSO configuration window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="c5a07-172">Kattintson a **hozzáadása új IDENTITÁSSZOLGÁLTATÓ** az egyszeri bejelentkezés beállítások hozzáadásával a képernyő jobb felső sarokban beállítást.</span><span class="sxs-lookup"><span data-stu-id="c5a07-172">Click on **ADD NEW IDENTITY PROVIDER** option in the top right corner to open the form for adding the settings for SSO.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="c5a07-174">Adja meg a mezők a részleteket lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="c5a07-174">Enter the details in the fields as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="c5a07-176">a.</span><span class="sxs-lookup"><span data-stu-id="c5a07-176">a.</span></span> <span data-ttu-id="c5a07-177">**MEGJELENÍTENDŐ név** : a beépülő modul megjelenítendő nevét adja meg a felügyelet lapon.</span><span class="sxs-lookup"><span data-stu-id="c5a07-177">**DISPLAY NAME** : Enter the display name of the plugin on the admin page.</span></span>

    <span data-ttu-id="c5a07-178">b.</span><span class="sxs-lookup"><span data-stu-id="c5a07-178">b.</span></span> <span data-ttu-id="c5a07-179">**GOMB neve** : a lap használatával történő Egyszeri bejelentkezéshez a bejelentkezési oldal megjelenítő nevét.</span><span class="sxs-lookup"><span data-stu-id="c5a07-179">**BUTTON NAME** : The name of the tab which will display on the login page for logging in via SSO.</span></span>

    <span data-ttu-id="c5a07-180">c.</span><span class="sxs-lookup"><span data-stu-id="c5a07-180">c.</span></span> <span data-ttu-id="c5a07-181">**TANÚSÍTVÁNY** : Nyissa meg a tanúsítványt a Jegyzettömbben, Azure-portálról letöltött ugyanazt a tartalom másolása és illessze be ide a mezőbe.</span><span class="sxs-lookup"><span data-stu-id="c5a07-181">**CERTIFICATE** : Open the Certificate downloaded earlier from the Azure portal in notepad, copy the contents of the same and paste it here in the box.</span></span>

    <span data-ttu-id="c5a07-182">d.</span><span class="sxs-lookup"><span data-stu-id="c5a07-182">d.</span></span> <span data-ttu-id="c5a07-183">**A belépési pont** : illessze be a **SAML-alapú egyszeri bejelentkezési URL-címe** másolt korábbi űrlap az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="c5a07-183">**ENTRY POINT** : Paste the **SAML Single Sign-On Service URL** copied earlier form the Azure portal.</span></span>

    <span data-ttu-id="c5a07-184">e.</span><span class="sxs-lookup"><span data-stu-id="c5a07-184">e.</span></span> <span data-ttu-id="c5a07-185">Váltás is **ON** , majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-185">Switch the option to **ON** and click on **SAVE**.</span></span>   

<!--### Next steps

To ensure users can sign-in to Teamphoria after it has been configured to use Azure Active Directory, review the following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior to sign-in. To set this up, see Provisioning.
 
- Users must be assigned access to Teamphoria in Azure AD to sign-in. To assign users, see Users.
 
- To configure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on to users, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="c5a07-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5a07-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="c5a07-187">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="c5a07-187">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="c5a07-189">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c5a07-189">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="c5a07-190">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="c5a07-190">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="c5a07-192">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c5a07-192">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="c5a07-194">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c5a07-194">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="c5a07-196">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="c5a07-196">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="c5a07-198">a.</span><span class="sxs-lookup"><span data-stu-id="c5a07-198">a.</span></span> <span data-ttu-id="c5a07-199">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-199">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="c5a07-200">b.</span><span class="sxs-lookup"><span data-stu-id="c5a07-200">b.</span></span> <span data-ttu-id="c5a07-201">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c5a07-201">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c5a07-202">c.</span><span class="sxs-lookup"><span data-stu-id="c5a07-202">c.</span></span> <span data-ttu-id="c5a07-203">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-203">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="c5a07-204">d.</span><span class="sxs-lookup"><span data-stu-id="c5a07-204">d.</span></span> <span data-ttu-id="c5a07-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a07-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="c5a07-206">Teamphoria tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="c5a07-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="c5a07-207">Ahhoz, hogy az Azure AD-felhasználók Teamphoria bejelentkezni, akkor ki kell építenie Teamphoria be.</span><span class="sxs-lookup"><span data-stu-id="c5a07-207">In order to enable Azure AD users to log into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="c5a07-208">Teamphoria, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c5a07-208">In the case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="c5a07-209">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c5a07-209">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="c5a07-210">Jelentkezzen be rendszergazdaként a Teamphoria vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="c5a07-210">Log in to your Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="c5a07-211">Kattintson a **ADMIN** a bal oldali eszköztáron és a beállítások a **kezelése** lapon kattintson a **felhasználók** a felhasználók számára a rendszergazda lap megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c5a07-211">Click on **ADMIN** settings on the left toolbar and under the **MANAGE** tab Click on **USERS** to open the admin page for users.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="c5a07-213">Kattintson a **manuális MEGHÍVÁSA** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="c5a07-213">Click on the **MANUAL INVITE** option.</span></span>

    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="c5a07-215">Ezen a lapon hajtsa végre a következő művelet.</span><span class="sxs-lookup"><span data-stu-id="c5a07-215">On this page, perform following action.</span></span> 
    
    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="c5a07-217">a.</span><span class="sxs-lookup"><span data-stu-id="c5a07-217">a.</span></span> <span data-ttu-id="c5a07-218">Az a **E-mail cím** szövegmezőhöz a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="c5a07-218">In the **EMAIL ADDRESS** textbox, the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="c5a07-219">b.</span><span class="sxs-lookup"><span data-stu-id="c5a07-219">b.</span></span> <span data-ttu-id="c5a07-220">Az a **UTÓNÉV** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-220">In the **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="c5a07-221">c.</span><span class="sxs-lookup"><span data-stu-id="c5a07-221">c.</span></span> <span data-ttu-id="c5a07-222">Az a **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-222">In the **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="c5a07-223">d.</span><span class="sxs-lookup"><span data-stu-id="c5a07-223">d.</span></span> <span data-ttu-id="c5a07-224">Kattintson a **MEGHÍVÁSA 1 felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="c5a07-225">Fogadja el a rendszer létrehozása a meghívott felhasználó felhasználónak kell.</span><span class="sxs-lookup"><span data-stu-id="c5a07-225">User needs to accept the invite to get created in the system.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="c5a07-226">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="c5a07-226">Assigning the Azure AD test user</span></span>

<span data-ttu-id="c5a07-227">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Teamphoria Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="c5a07-227">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Teamphoria.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="c5a07-229">**Britta Simon hozzárendelése Teamphoria, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="c5a07-229">**To assign Britta Simon to Teamphoria, perform the following steps:**</span></span>

1. <span data-ttu-id="c5a07-230">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-230">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="c5a07-232">Az alkalmazások listában válassza ki a **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-232">In the applications list, select **Teamphoria**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="c5a07-234">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c5a07-234">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="c5a07-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="c5a07-236">Click **Add** button.</span></span> <span data-ttu-id="c5a07-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c5a07-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="c5a07-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="c5a07-239">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="c5a07-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c5a07-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="c5a07-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="c5a07-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="c5a07-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="c5a07-242">Testing single sign-on</span></span>

<span data-ttu-id="c5a07-243">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="c5a07-243">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="c5a07-244">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="c5a07-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="c5a07-245">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="c5a07-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="c5a07-246">További források</span><span class="sxs-lookup"><span data-stu-id="c5a07-246">Additional resources</span></span>

* [<span data-ttu-id="c5a07-247">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="c5a07-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="c5a07-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="c5a07-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

