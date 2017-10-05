---
title: "Oktatóanyag: Azure Active Directoryval integrált Workstars |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Workstars között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 51a4a4e4-ff60-4971-b3f8-a0367b70d220
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: e17c85689fa3aebf00ebf559185032b90103b4a5
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/29/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workstars"></a><span data-ttu-id="dfeb6-103">Oktatóanyag: Azure Active Directoryval integrált Workstars</span><span class="sxs-lookup"><span data-stu-id="dfeb6-103">Tutorial: Azure Active Directory integration with Workstars</span></span>

<span data-ttu-id="dfeb6-104">Ebben az oktatóanyagban elsajátíthatja Workstars integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dfeb6-104">In this tutorial, you learn how to integrate Workstars with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dfeb6-105">Workstars integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-105">Integrating Workstars with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dfeb6-106">Az Azure AD, aki hozzáfér Workstars szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-106">You can control in Azure AD who has access to Workstars.</span></span>
- <span data-ttu-id="dfeb6-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Workstars (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-107">You can enable your users to automatically get signed-on to Workstars (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="dfeb6-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="dfeb6-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dfeb6-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfeb6-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dfeb6-110">Prerequisites</span></span>

<span data-ttu-id="dfeb6-111">Konfigurálása az Azure AD-integrációs Workstars, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-111">To configure Azure AD integration with Workstars, you need the following items:</span></span>

- <span data-ttu-id="dfeb6-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dfeb6-112">An Azure AD subscription</span></span>
- <span data-ttu-id="dfeb6-113">Egy Workstars egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="dfeb6-113">A Workstars single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dfeb6-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dfeb6-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dfeb6-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dfeb6-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dfeb6-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dfeb6-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-118">Scenario description</span></span>
<span data-ttu-id="dfeb6-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dfeb6-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dfeb6-121">A gyűjteményből Workstars hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-121">Adding Workstars from the gallery</span></span>
2. <span data-ttu-id="dfeb6-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dfeb6-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workstars-from-the-gallery"></a><span data-ttu-id="dfeb6-123">A gyűjteményből Workstars hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-123">Adding Workstars from the gallery</span></span>
<span data-ttu-id="dfeb6-124">Az Azure AD integrálása a Workstars konfigurálásához kell hozzáadnia Workstars a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-124">To configure the integration of Workstars into Azure AD, you need to add Workstars from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dfeb6-125">**A gyűjteményből Workstars hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfeb6-125">**To add Workstars from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dfeb6-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="dfeb6-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dfeb6-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="dfeb6-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="dfeb6-133">Írja be a keresőmezőbe, **Workstars**, jelölje be **Workstars** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-133">In the search box, type **Workstars**, select **Workstars** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában Workstars](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="dfeb6-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="dfeb6-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Workstars.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-136">In this section, you configure and test Azure AD single sign-on with Workstars based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="dfeb6-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Workstars a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Workstars is to a user in Azure AD.</span></span> <span data-ttu-id="dfeb6-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Workstars közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-138">In other words, a link relationship between an Azure AD user and the related user in Workstars needs to be established.</span></span>

<span data-ttu-id="dfeb6-139">Workstars, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-139">In Workstars, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="dfeb6-140">Az Azure AD egyszeri bejelentkezést a Workstars tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-140">To configure and test Azure AD single sign-on with Workstars, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dfeb6-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dfeb6-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dfeb6-143">**[Workstars tesztfelhasználó létrehozása](#create-a-workstars-test-user)**  - való Britta Simon valami Workstars, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-143">**[Create a Workstars test user](#create-a-workstars-test-user)** - to have a counterpart of Britta Simon in Workstars that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dfeb6-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dfeb6-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="dfeb6-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="dfeb6-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Workstars alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workstars application.</span></span>

<span data-ttu-id="dfeb6-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés Workstars, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfeb6-148">**To configure Azure AD single sign-on with Workstars, perform the following steps:**</span></span>

1. <span data-ttu-id="dfeb6-149">Az Azure portálon a a **Workstars** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-149">In the Azure portal, on the **Workstars** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="dfeb6-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_samlbase.png)

3. <span data-ttu-id="dfeb6-153">Az a **Workstars tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-153">On the **Workstars Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Workstars tartomány és az URL-címek](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_url.png)

    <span data-ttu-id="dfeb6-155">a.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-155">a.</span></span> <span data-ttu-id="dfeb6-156">Az a **azonosító** szövegmező, írja be az URL-cím:`https://workstars.com`</span><span class="sxs-lookup"><span data-stu-id="dfeb6-156">In the **Identifier** textbox, type the URL: `https://workstars.com`</span></span>

    <span data-ttu-id="dfeb6-157">b.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-157">b.</span></span> <span data-ttu-id="dfeb6-158">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.workstars.com/saml/login_check`</span><span class="sxs-lookup"><span data-stu-id="dfeb6-158">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.workstars.com/saml/login_check`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="dfeb6-159">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-159">The value is not real.</span></span> <span data-ttu-id="dfeb6-160">Frissítse az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-160">Update the value with the actual Reply URL.</span></span> <span data-ttu-id="dfeb6-161">Ügyfél [Workstars támogatási csoport](https://support.workstars.com) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-161">Contact [Workstars support team](https://support.workstars.com) to get the value.</span></span>
 
4. <span data-ttu-id="dfeb6-162">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-162">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_certificate.png) 

5. <span data-ttu-id="dfeb6-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-workstars-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="dfeb6-166">A a **Workstars konfigurációs** kattintson **konfigurálása Workstars** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-166">On the **Workstars Configuration** section, click **Configure Workstars** to open **Configure sign-on** window.</span></span> <span data-ttu-id="dfeb6-167">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="dfeb6-167">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Workstars konfiguráció](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_configure.png) 

7. <span data-ttu-id="dfeb6-169">Egy másik böngészőablakban jelentkezzen be a Workstars vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-169">In another browser window, sign on to your Workstars company site as an administrator.</span></span>

8. <span data-ttu-id="dfeb6-170">Kattintson az eszköztáron **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-170">In the main toolbar, click **Settings**.</span></span>

    ![Workstars Dokumentumb](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_sett.png)

9. <span data-ttu-id="dfeb6-172">Ugrás a **bejelentkezés** > **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-172">Go to **Sign On** > **Settings**.</span></span>

    ![Workstars bejelentkezés](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_signon.png)

    ![Workstars beállítások](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_settings.png)

10. <span data-ttu-id="dfeb6-175">Az a **egyszeri bejelentkezési a (SAML) - beállítások** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-175">On the **Single Sign On (SAML) - Settings** page, perform the following steps:</span></span>
    
    ![Workstars saml](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_saml.png)

    <span data-ttu-id="dfeb6-177">a.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-177">a.</span></span> <span data-ttu-id="dfeb6-178">A **identitás szolgáltatónevet** szövegmezőhöz típus **Office 365**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-178">In **Identity Provider Name** textbox, type **Office 365**.</span></span>

    <span data-ttu-id="dfeb6-179">b.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-179">b.</span></span> <span data-ttu-id="dfeb6-180">A a **Identity Provider Entitásazonosító** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-180">In the **Identity Provider Entity ID** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="dfeb6-181">c.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-181">c.</span></span> <span data-ttu-id="dfeb6-182">Másolja a letöltött fájlt a Jegyzettömbben, és illessze be azt a **x509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-182">Copy the content of the downloaded certificate file in notepad, and then paste it into the **x509 Certificate** textbox.</span></span> 

    <span data-ttu-id="dfeb6-183">d.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-183">d.</span></span> <span data-ttu-id="dfeb6-184">A a **SAML SSO URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-184">In the **SAML SSO URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="dfeb6-185">e.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-185">e.</span></span> <span data-ttu-id="dfeb6-186">A a **távoli kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-186">In the **Remote Logout URL** textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="dfeb6-187">f.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-187">f.</span></span> <span data-ttu-id="dfeb6-188">Válassza ki **Névazonosító** , **E-mail (alapértelmezett)**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-188">select **Name ID** as **Email (Default)**.</span></span>

    <span data-ttu-id="dfeb6-189">g.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-189">g.</span></span> <span data-ttu-id="dfeb6-190">Kattintson a **megerősítése**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-190">Click **Confirm**.</span></span>
    
> [!TIP]
> <span data-ttu-id="dfeb6-191">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dfeb6-191">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dfeb6-192">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-192">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dfeb6-193">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dfeb6-193">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="dfeb6-194">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="dfeb6-194">Create an Azure AD test user</span></span>

<span data-ttu-id="dfeb6-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-195">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="dfeb6-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfeb6-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dfeb6-198">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-198">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-workstars-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="dfeb6-200">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-200">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-workstars-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="dfeb6-202">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-202">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-workstars-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="dfeb6-204">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dfeb6-204">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-workstars-tutorial/create_aaduser_04.png)

    <span data-ttu-id="dfeb6-206">a.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-206">a.</span></span> <span data-ttu-id="dfeb6-207">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-207">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dfeb6-208">b.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-208">b.</span></span> <span data-ttu-id="dfeb6-209">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-209">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="dfeb6-210">c.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-210">c.</span></span> <span data-ttu-id="dfeb6-211">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-211">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="dfeb6-212">d.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-212">d.</span></span> <span data-ttu-id="dfeb6-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-213">Click **Create**.</span></span>
  
### <a name="create-a-workstars-test-user"></a><span data-ttu-id="dfeb6-214">Workstars tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-214">Create a Workstars test user</span></span>

<span data-ttu-id="dfeb6-215">Ebben a szakaszban egy Workstars Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-215">In this section, you create a user called Britta Simon in Workstars.</span></span> <span data-ttu-id="dfeb6-216">Együttműködve [Workstars támogatási csoport](https://support.workstars.com) a felhasználók hozzáadása a Workstars platform.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-216">Work with [Workstars support team](https://support.workstars.com) to add the users in the Workstars platform.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="dfeb6-217">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="dfeb6-217">Assign the Azure AD test user</span></span>

<span data-ttu-id="dfeb6-218">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Workstars Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-218">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workstars.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="dfeb6-220">**Britta Simon hozzárendelése Workstars, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfeb6-220">**To assign Britta Simon to Workstars, perform the following steps:**</span></span>

1. <span data-ttu-id="dfeb6-221">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-221">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dfeb6-223">Az alkalmazások listában válassza ki a **Workstars**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-223">In the applications list, select **Workstars**.</span></span>

    ![Az alkalmazások listáját a Workstars hivatkozás](./media/active-directory-saas-workstars-tutorial/tutorial_workstars_app.png)  

3. <span data-ttu-id="dfeb6-225">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-225">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="dfeb6-227">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-227">Click **Add** button.</span></span> <span data-ttu-id="dfeb6-228">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-228">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="dfeb6-230">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-230">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dfeb6-231">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-231">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dfeb6-232">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-232">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="dfeb6-233">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dfeb6-233">Test single sign-on</span></span>

<span data-ttu-id="dfeb6-234">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-234">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dfeb6-235">Ha a hozzáférési panelen Workstars csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Workstars alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="dfeb6-235">When you click the Workstars tile in the Access Panel, you should get automatically signed-on to your Workstars application.</span></span>
<span data-ttu-id="dfeb6-236">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dfeb6-236">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="dfeb6-237">További források</span><span class="sxs-lookup"><span data-stu-id="dfeb6-237">Additional resources</span></span>

* [<span data-ttu-id="dfeb6-238">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dfeb6-238">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dfeb6-239">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dfeb6-239">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workstars-tutorial/tutorial_general_203.png

