---
title: "Oktatóanyag: Azure Active Directoryval integrált Velpic SAML |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Velpic SAML között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 5325f3cca00167e6b7b687509ce43435447ad2f4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a><span data-ttu-id="48c0d-103">Oktatóanyag: Azure Active Directoryval integrált Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="48c0d-103">Tutorial: Azure Active Directory integration with Velpic SAML</span></span>

<span data-ttu-id="48c0d-104">Ebben az oktatóanyagban elsajátíthatja Velpic SAML integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="48c0d-104">In this tutorial, you learn how to integrate Velpic SAML with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="48c0d-105">Velpic SAML integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="48c0d-105">Integrating Velpic SAML with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="48c0d-106">Megadhatja a Velpic SAML hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="48c0d-106">You can control in Azure AD who has access to Velpic SAML</span></span>
- <span data-ttu-id="48c0d-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Velpic SAML (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="48c0d-107">You can enable your users to automatically get signed-on to Velpic SAML (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="48c0d-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="48c0d-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="48c0d-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="48c0d-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="48c0d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="48c0d-110">Prerequisites</span></span>

<span data-ttu-id="48c0d-111">Velpic SAML konfigurálása az Azure AD-integrációra, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="48c0d-111">To configure Azure AD integration with Velpic SAML, you need the following items:</span></span>

- <span data-ttu-id="48c0d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="48c0d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="48c0d-113">Egy Velpic SAML-alapú egyszeri bejelentkezést engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="48c0d-113">A Velpic SAML single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="48c0d-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="48c0d-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="48c0d-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="48c0d-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="48c0d-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="48c0d-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="48c0d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="48c0d-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="48c0d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="48c0d-118">Scenario description</span></span>
<span data-ttu-id="48c0d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="48c0d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="48c0d-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="48c0d-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="48c0d-121">A gyűjteményből Velpic SAML hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48c0d-121">Adding Velpic SAML from the gallery</span></span>
2. <span data-ttu-id="48c0d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48c0d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-velpic-saml-from-the-gallery"></a><span data-ttu-id="48c0d-123">A gyűjteményből Velpic SAML hozzáadása</span><span class="sxs-lookup"><span data-stu-id="48c0d-123">Adding Velpic SAML from the gallery</span></span>
<span data-ttu-id="48c0d-124">Az Azure AD integrálása a Velpic SAML konfigurálásához kell hozzáadnia Velpic SAML a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="48c0d-124">To configure the integration of Velpic SAML into Azure AD, you need to add Velpic SAML from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="48c0d-125">**A gyűjteményből Velpic SAML hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="48c0d-125">**To add Velpic SAML from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="48c0d-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="48c0d-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="48c0d-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="48c0d-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="48c0d-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="48c0d-133">Írja be a keresőmezőbe, **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-133">In the search box, type **Velpic SAML**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. <span data-ttu-id="48c0d-135">Az eredmények panelen válassza ki a **Velpic SAML**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="48c0d-135">In the results panel, select **Velpic SAML**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="48c0d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="48c0d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="48c0d-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Velpic SAML-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="48c0d-138">In this section, you configure and test Azure AD single sign-on with Velpic SAML based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="48c0d-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Velpic SAML a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="48c0d-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Velpic SAML is to a user in Azure AD.</span></span> <span data-ttu-id="48c0d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Velpic SAML közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="48c0d-140">In other words, a link relationship between an Azure AD user and the related user in Velpic SAML needs to be established.</span></span>

<span data-ttu-id="48c0d-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="48c0d-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Velpic SAML.</span></span>

<span data-ttu-id="48c0d-142">Az Azure AD egyszeri bejelentkezéshez a Velpic SAML tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="48c0d-142">To configure and test Azure AD single sign-on with Velpic SAML, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="48c0d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="48c0d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="48c0d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="48c0d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="48c0d-145">**[Velpic SAML tesztfelhasználó létrehozása](#creating-a-velpic-saml-test-user)**  - való egy megfelelője a Britta Simon Velpic SAML, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="48c0d-145">**[Creating a Velpic SAML test user](#creating-a-velpic-saml-test-user)** - to have a counterpart of Britta Simon in Velpic SAML that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="48c0d-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="48c0d-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="48c0d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="48c0d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="48c0d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48c0d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="48c0d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Velpic SAML-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="48c0d-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your Velpic SAML application.</span></span>

<span data-ttu-id="48c0d-150">**Velpic SAML konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="48c0d-150">**To configure Azure AD single sign-on with Velpic SAML, perform the following steps:**</span></span>

1. <span data-ttu-id="48c0d-151">Az Azure felügyeleti portálján a a **Velpic SAML** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-151">In the Azure Management portal, on the **Velpic SAML** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="48c0d-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="48c0d-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. <span data-ttu-id="48c0d-155">Adja meg a részleteket a **Velpic SAML-tartomány és az URL-címek** szakasz -</span><span class="sxs-lookup"><span data-stu-id="48c0d-155">Enter the details in the **Velpic SAML Domain and URLs** section-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    <span data-ttu-id="48c0d-157">a.</span><span class="sxs-lookup"><span data-stu-id="48c0d-157">a.</span></span> <span data-ttu-id="48c0d-158">Az a **bejelentkezési URL-cím** szövegmező, írja be az értéket, mint:`https://<sub-domain>.velpicsaml.net`</span><span class="sxs-lookup"><span data-stu-id="48c0d-158">In the **Sign-on URL** textbox, type the value as: `https://<sub-domain>.velpicsaml.net`</span></span>

    <span data-ttu-id="48c0d-159">b.</span><span class="sxs-lookup"><span data-stu-id="48c0d-159">b.</span></span> <span data-ttu-id="48c0d-160">Az a **azonosító** szövegmező, illessze be a **"Egyszeri bejelentkezési URL-címe"** érték`https://auth.velpic.com/saml/v2/<entity-id>/login`</span><span class="sxs-lookup"><span data-stu-id="48c0d-160">In the **Identifier** textbox, paste the **‘Single sign on URL’** value `https://auth.velpic.com/saml/v2/<entity-id>/login`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="48c0d-161">Vegye figyelembe, hogy a Velpic SAML team biztosítja a bejelentkezési URL-címen, és azonosító értéket az SSO beépülő modul Velpic SAML oldalon konfigurálásakor lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="48c0d-161">Please note that the Sign on URL will be provided by the Velpic SAML team and Identifier value will be available when you configure the SSO Plugin on Velpic SAML side.</span></span> <span data-ttu-id="48c0d-162">Ezt az értéket Velpic SAML alkalmazáslap másolja és illessze be ide kell.</span><span class="sxs-lookup"><span data-stu-id="48c0d-162">You need to copy that value from Velpic SAML application  page and paste it here.</span></span>

4. <span data-ttu-id="48c0d-163">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="48c0d-163">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. <span data-ttu-id="48c0d-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="48c0d-167">Kattintson a Velpic SAML-alapú konfigurációs szakasz Velpic SAML konfigurálása konfigurálása bejelentkezés ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="48c0d-167">On the Velpic SAML Configuration section, click Configure Velpic SAML to open Configure sign-on window.</span></span> <span data-ttu-id="48c0d-168">Másolja a SAML Entitásazonosító rövid összefoglaló szakaszából.</span><span class="sxs-lookup"><span data-stu-id="48c0d-168">Copy the SAML Entity ID from the Quick Reference section.</span></span>

7. <span data-ttu-id="48c0d-169">Egy másik webes böngészőablakban jelentkezzen be a Velpic SAML-alapú vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="48c0d-169">In a different web browser window, log into your Velpic SAML company site as an administrator.</span></span>

8. <span data-ttu-id="48c0d-170">Kattintson a **kezelése** lapra, és navigáljon a **integrációs** szakaszban kattintson a kell **beépülő modulok** gombra kattintva hozzon létre új beépülő modul a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="48c0d-170">Click on **Manage** tab and go to **Integration** section where you need to click on **Plugins** button to create new plugin for Sign-In.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. <span data-ttu-id="48c0d-172">Kattintson a **"Beépülő modul hozzáadása"** gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-172">Click on the **‘Add plugin’** button.</span></span>
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. <span data-ttu-id="48c0d-174">Kattintson a **SAML** a beépülő modul hozzáadása oldalon csempére.</span><span class="sxs-lookup"><span data-stu-id="48c0d-174">Click on the **SAML** tile in the Add Plugin page.</span></span>
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. <span data-ttu-id="48c0d-176">Adja meg az új SAML-alapú beépülő modul nevét, majd kattintson a **"Add"** gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-176">Enter the name of the new SAML plugin and click the **‘Add’** button.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. <span data-ttu-id="48c0d-178">Adja meg a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="48c0d-178">Enter the details as follows:</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    <span data-ttu-id="48c0d-180">a.</span><span class="sxs-lookup"><span data-stu-id="48c0d-180">a.</span></span> <span data-ttu-id="48c0d-181">Az a **neve** szövegmező, írja be a SAML-alapú beépülő modul nevét.</span><span class="sxs-lookup"><span data-stu-id="48c0d-181">In the **Name** textbox, type the name of SAML plugin.</span></span>

    <span data-ttu-id="48c0d-182">b.</span><span class="sxs-lookup"><span data-stu-id="48c0d-182">b.</span></span> <span data-ttu-id="48c0d-183">A a **kiállítójának URL-címe** szövegmező, illessze be a **SAML Entitásazonosító** , átmásolva a **bejelentkezés konfigurálása** ablakot, az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="48c0d-183">In the **Issuer URL** textbox, paste the **SAML Entity ID** you copied from the **Configure sign-on** window of the Azure portal.</span></span>

    <span data-ttu-id="48c0d-184">c.</span><span class="sxs-lookup"><span data-stu-id="48c0d-184">c.</span></span> <span data-ttu-id="48c0d-185">Az a **szolgáltató metaadatok Config** a metaadatok XML-fájl, amely az Azure-portálról letöltött feltöltése.</span><span class="sxs-lookup"><span data-stu-id="48c0d-185">In the **Provider Metadata Config** upload the Metadata XML file which you downloaded from Azure portal.</span></span>

    <span data-ttu-id="48c0d-186">d.</span><span class="sxs-lookup"><span data-stu-id="48c0d-186">d.</span></span> <span data-ttu-id="48c0d-187">SAML engedélyezése csak az idő-kiépítés engedélyezése is beállíthatja a **"Automatikus hozzon létre új felhasználók"** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="48c0d-187">You can also choose to enable SAML just in time provisioning by enabling the **‘Auto create new users’** checkbox.</span></span> <span data-ttu-id="48c0d-188">Ha egy felhasználó Velpic nem létezik, és ez a jelző nincs engedélyezve, az Azure-ból a bejelentkezés sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="48c0d-188">If a user doesn’t exist in Velpic and this flag is not enabled, the login from Azure will fail.</span></span> <span data-ttu-id="48c0d-189">Ha a jelző engedélyezve van a felhasználó automatikusan megkapják a Velpic bejelentkezés alkalmával.</span><span class="sxs-lookup"><span data-stu-id="48c0d-189">If the flag is enabled the user will automatically be provisioned into Velpic at the time of login.</span></span> 

    <span data-ttu-id="48c0d-190">e.</span><span class="sxs-lookup"><span data-stu-id="48c0d-190">e.</span></span> <span data-ttu-id="48c0d-191">Másolás a **egyszeri bejelentkezés URL-címen** szöveg mezőbe, majd illessze be az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="48c0d-191">Copy the **Single sign on URL** from the text box and paste it in the Azure portal.</span></span>
    
    <span data-ttu-id="48c0d-192">f.</span><span class="sxs-lookup"><span data-stu-id="48c0d-192">f.</span></span> <span data-ttu-id="48c0d-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-193">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="48c0d-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48c0d-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="48c0d-195">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="48c0d-195">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="48c0d-197">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="48c0d-197">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="48c0d-198">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-198">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="48c0d-200">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="48c0d-200">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="48c0d-202">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48c0d-202">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="48c0d-204">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="48c0d-204">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="48c0d-206">a.</span><span class="sxs-lookup"><span data-stu-id="48c0d-206">a.</span></span> <span data-ttu-id="48c0d-207">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-207">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="48c0d-208">b.</span><span class="sxs-lookup"><span data-stu-id="48c0d-208">b.</span></span> <span data-ttu-id="48c0d-209">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="48c0d-209">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="48c0d-210">c.</span><span class="sxs-lookup"><span data-stu-id="48c0d-210">c.</span></span> <span data-ttu-id="48c0d-211">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-211">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="48c0d-212">d.</span><span class="sxs-lookup"><span data-stu-id="48c0d-212">d.</span></span> <span data-ttu-id="48c0d-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-213">Click **Create**.</span></span>
 
### <a name="creating-a-velpic-saml-test-user"></a><span data-ttu-id="48c0d-214">Velpic SAML tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="48c0d-214">Creating a Velpic SAML test user</span></span>

<span data-ttu-id="48c0d-215">Ebben a lépésben esetén általában nincs szükség, ha az alkalmazás csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="48c0d-215">This step is usually not required as the application supports just in time user provisioning.</span></span> <span data-ttu-id="48c0d-216">Ha nincs engedélyezve az automatikus felhasználó-átadási majd kézi felhasználó létrehozása teheti az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="48c0d-216">If the automatic user provisioning is not enabled then manual user creation can be done as described below.</span></span>

<span data-ttu-id="48c0d-217">Jelentkezzen be rendszergazdaként egy vállalati Velpic SAML-alapú webhelyekhez, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="48c0d-217">Log into your Velpic SAML company site as an administrator and perform following steps:</span></span>
    
1. <span data-ttu-id="48c0d-218">Kattintson a kezelés lapon, majd nyissa meg a felhasználók szakaszban kattintson a felhasználók hozzáadása az új gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-218">Click on Manage tab and go to Users section, then click on New button to add users.</span></span>

    ![felhasználó hozzáadása](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. <span data-ttu-id="48c0d-220">Az a **"Az új felhasználó létrehozása"** párbeszédpanel lapon, a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="48c0d-220">On the **“Create New User”** dialog page, perform the following steps.</span></span>

    ![Felhasználó](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    <span data-ttu-id="48c0d-222">a.</span><span class="sxs-lookup"><span data-stu-id="48c0d-222">a.</span></span> <span data-ttu-id="48c0d-223">Az a **Keresztnév** szövegmezőhöz Britta Simon az első nevét.</span><span class="sxs-lookup"><span data-stu-id="48c0d-223">In the **First Name** textbox, type the first name of Britta Simon.</span></span>

    <span data-ttu-id="48c0d-224">b.</span><span class="sxs-lookup"><span data-stu-id="48c0d-224">b.</span></span> <span data-ttu-id="48c0d-225">Az a **Vezetéknév** szövegmezőhöz Britta Simon utolsó neve.</span><span class="sxs-lookup"><span data-stu-id="48c0d-225">In the **Last Name** textbox, type the last name of Britta Simon.</span></span>

    <span data-ttu-id="48c0d-226">c.</span><span class="sxs-lookup"><span data-stu-id="48c0d-226">c.</span></span> <span data-ttu-id="48c0d-227">Az a **felhasználónév** szövegmező, írja be a felhasználónevet Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="48c0d-227">In the **User Name** textbox, type the user name of Britta Simon.</span></span>

    <span data-ttu-id="48c0d-228">d.</span><span class="sxs-lookup"><span data-stu-id="48c0d-228">d.</span></span> <span data-ttu-id="48c0d-229">Az a **E-mail** szövegmezőhöz Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="48c0d-229">In the **Email** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="48c0d-230">e.</span><span class="sxs-lookup"><span data-stu-id="48c0d-230">e.</span></span> <span data-ttu-id="48c0d-231">További információk megadása nem kötelező, megadhatja azt, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="48c0d-231">Rest of the information is optional, you can fill it if needed.</span></span>
    
    <span data-ttu-id="48c0d-232">f.</span><span class="sxs-lookup"><span data-stu-id="48c0d-232">f.</span></span> <span data-ttu-id="48c0d-233">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-233">Click **SAVE**.</span></span>  

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="48c0d-234">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="48c0d-234">Assigning the Azure AD test user</span></span>

<span data-ttu-id="48c0d-235">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés Velpic SAML Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="48c0d-235">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Velpic SAML.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="48c0d-237">**Britta Simon hozzárendelése Velpic SAML, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="48c0d-237">**To assign Britta Simon to Velpic SAML, perform the following steps:**</span></span>

1. <span data-ttu-id="48c0d-238">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-238">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="48c0d-240">Az alkalmazások listában válassza ki a **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-240">In the applications list, select **Velpic SAML**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. <span data-ttu-id="48c0d-242">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="48c0d-242">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="48c0d-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="48c0d-244">Click **Add** button.</span></span> <span data-ttu-id="48c0d-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48c0d-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="48c0d-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="48c0d-247">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="48c0d-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48c0d-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="48c0d-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="48c0d-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="48c0d-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="48c0d-250">Testing single sign-on</span></span>

<span data-ttu-id="48c0d-251">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="48c0d-251">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

1. <span data-ttu-id="48c0d-252">Ha a hozzáférési panelen Velpic SAML csempére kattint, Velpic SAML alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="48c0d-252">When you click the Velpic SAML tile in the Access Panel, you should get login page of Velpic SAML application.</span></span> <span data-ttu-id="48c0d-253">Megjelenik a **"Jelentkezzen be az Azure AD"** a bejelentkezési oldal gombjára.</span><span class="sxs-lookup"><span data-stu-id="48c0d-253">You should see the **‘Log In With Azure AD’** button on the sign in page.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. <span data-ttu-id="48c0d-255">Kattintson a **"Az Azure AD bejelentkezés"** gombra kattintva Velpic az Azure AD-fiókkal bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="48c0d-255">Click on the **‘Log In With Azure AD’** button to log in to Velpic using your Azure AD account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="48c0d-256">További források</span><span class="sxs-lookup"><span data-stu-id="48c0d-256">Additional resources</span></span>

* [<span data-ttu-id="48c0d-257">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="48c0d-257">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="48c0d-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="48c0d-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

