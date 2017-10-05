---
title: "Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SkyDesk E-mail között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 0ffcca4161fc836192fc9c9871a905f36ea76b32
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a><span data-ttu-id="fbff4-103">Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail</span><span class="sxs-lookup"><span data-stu-id="fbff4-103">Tutorial: Azure Active Directory integration with SkyDesk Email</span></span>

<span data-ttu-id="fbff4-104">Ebben az oktatóanyagban elsajátíthatja SkyDesk E-mail integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fbff4-104">In this tutorial, you learn how to integrate SkyDesk Email with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fbff4-105">SkyDesk E-mail integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fbff4-105">Integrating SkyDesk Email with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fbff4-106">Megadhatja a SkyDesk e-mailek elérését, aki az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fbff4-106">You can control in Azure AD who has access to SkyDesk Email</span></span>
- <span data-ttu-id="fbff4-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SkyDesk e-mailhez (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="fbff4-107">You can enable your users to automatically get signed-on to SkyDesk Email (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fbff4-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fbff4-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fbff4-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fbff4-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fbff4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fbff4-110">Prerequisites</span></span>

<span data-ttu-id="fbff4-111">Az Azure AD-integrációs konfigurálásához SkyDesk e-mail, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fbff4-111">To configure Azure AD integration with SkyDesk Email, you need the following items:</span></span>

- <span data-ttu-id="fbff4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fbff4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fbff4-113">Egy e-mailek SkyDesk egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fbff4-113">A SkyDesk Email single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fbff4-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fbff4-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fbff4-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fbff4-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fbff4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fbff4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fbff4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fbff4-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fbff4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fbff4-118">Scenario description</span></span>
<span data-ttu-id="fbff4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fbff4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fbff4-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fbff4-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fbff4-121">A gyűjteményből SkyDesk E-mail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbff4-121">Adding SkyDesk Email from the gallery</span></span>
2. <span data-ttu-id="fbff4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fbff4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skydesk-email-from-the-gallery"></a><span data-ttu-id="fbff4-123">A gyűjteményből SkyDesk E-mail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fbff4-123">Adding SkyDesk Email from the gallery</span></span>
<span data-ttu-id="fbff4-124">Az Azure AD integrálása a SkyDesk E-mail konfigurálásához kell hozzáadnia SkyDesk e-mailt a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fbff4-124">To configure the integration of SkyDesk Email into Azure AD, you need to add SkyDesk Email from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fbff4-125">**A gyűjteményből SkyDesk E-mail hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbff4-125">**To add SkyDesk Email from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fbff4-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fbff4-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fbff4-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fbff4-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fbff4-133">Írja be a keresőmezőbe, **SkyDesk E-mail**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-133">In the search box, type **SkyDesk Email**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. <span data-ttu-id="fbff4-135">Az eredmények panelen válassza ki a **SkyDesk E-mail**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fbff4-135">In the results panel, select **SkyDesk Email**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="fbff4-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="fbff4-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="fbff4-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés SkyDesk e-mail "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="fbff4-138">In this section, you configure and test Azure AD single sign-on with SkyDesk Email based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fbff4-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó SkyDesk e-mailben a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fbff4-139">For single sign-on to work, Azure AD needs to know what the counterpart user in SkyDesk Email is to a user in Azure AD.</span></span> <span data-ttu-id="fbff4-140">Ez azt jelenti közötti egy Azure AD-felhasználó és a kapcsolódó felhasználó SkyDesk e-mailben szereplő hivatkozásra kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fbff4-140">In other words, a link relationship between an Azure AD user and the related user in SkyDesk Email needs to be established.</span></span>

<span data-ttu-id="fbff4-141">SkyDesk e-mailben, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fbff4-141">In SkyDesk Email, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fbff4-142">Az Azure AD az egyszeri bejelentkezés SkyDesk e-mail tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fbff4-142">To configure and test Azure AD single sign-on with SkyDesk Email, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fbff4-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fbff4-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fbff4-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fbff4-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fbff4-145">**[SkyDesk E-mail tesztfelhasználó létrehozása](#creating-a-skydesk-email-test-user)**  - kell rendelkeznie a megfelelője a Britta Simon SkyDesk e-mailben, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fbff4-145">**[Creating a SkyDesk Email test user](#creating-a-skydesk-email-test-user)** - to have a counterpart of Britta Simon in SkyDesk Email that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fbff4-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fbff4-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fbff4-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fbff4-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="fbff4-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fbff4-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="fbff4-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és a SkyDesk levelezőprogramban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="fbff4-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your SkyDesk Email application.</span></span>

<span data-ttu-id="fbff4-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés SkyDesk e-mailek, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbff4-150">**To configure Azure AD single sign-on with SkyDesk Email, perform the following steps:**</span></span>

1. <span data-ttu-id="fbff4-151">Az Azure portálon a a **SkyDesk E-mail** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-151">In the Azure portal, on the **SkyDesk Email** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fbff4-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fbff4-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. <span data-ttu-id="fbff4-155">Az a **SkyDesk E-mail tartománya és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fbff4-155">On the **SkyDesk Email Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    <span data-ttu-id="fbff4-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://mail.skydesk.jp/portal/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="fbff4-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://mail.skydesk.jp/portal/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fbff4-158">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="fbff4-158">The value is not real.</span></span> <span data-ttu-id="fbff4-159">Frissítse az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="fbff4-159">Update the value with the actual Sign-On URL.</span></span> <span data-ttu-id="fbff4-160">Ügyfél [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/) az értéket be kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="fbff4-160">Contact [SkyDesk Email Client support team](https://www.skydesk.sg/support/) to get the value.</span></span> 
 
4. <span data-ttu-id="fbff4-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fbff4-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. <span data-ttu-id="fbff4-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fbff4-165">A a **SkyDesk E-mail-konfigurációjának** kattintson **konfigurálása SkyDesk E-mail** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fbff4-165">On the **SkyDesk Email Configuration** section, click **Configure SkyDesk Email** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fbff4-166">Másolás a **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fbff4-166">Copy the **Sign-Out URL, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. <span data-ttu-id="fbff4-168">Az egyszeri bejelentkezés engedélyezése **SkyDesk E-mail**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fbff4-168">To enable SSO in **SkyDesk Email**, perform the following steps:</span></span>

    <span data-ttu-id="fbff4-169">a.</span><span class="sxs-lookup"><span data-stu-id="fbff4-169">a.</span></span> <span data-ttu-id="fbff4-170">Bejelentkezés SkyDesk E-mail fiókja rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="fbff4-170">Sign-on to your SkyDesk Email account as administrator.</span></span>

    <span data-ttu-id="fbff4-171">b.</span><span class="sxs-lookup"><span data-stu-id="fbff4-171">b.</span></span> <span data-ttu-id="fbff4-172">Kattintson a felső menüben **telepítő**, és válassza ki **szervezeti**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-172">In the menu on the top, click **Setup**, and select **Org**.</span></span> 
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    <span data-ttu-id="fbff4-174">c.</span><span class="sxs-lookup"><span data-stu-id="fbff4-174">c.</span></span> <span data-ttu-id="fbff4-175">Kattintson a **tartományok** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="fbff4-175">Click on **Domains** from the left panel.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    <span data-ttu-id="fbff4-177">d.</span><span class="sxs-lookup"><span data-stu-id="fbff4-177">d.</span></span> <span data-ttu-id="fbff4-178">Kattintson a **tartomány hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-178">Click on **Add Domain**.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    <span data-ttu-id="fbff4-180">e.</span><span class="sxs-lookup"><span data-stu-id="fbff4-180">e.</span></span> <span data-ttu-id="fbff4-181">Adja meg a tartomány nevét, és ellenőrizze a tartomány.</span><span class="sxs-lookup"><span data-stu-id="fbff4-181">Enter your Domain name, and then verify the Domain.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    <span data-ttu-id="fbff4-183">f.</span><span class="sxs-lookup"><span data-stu-id="fbff4-183">f.</span></span> <span data-ttu-id="fbff4-184">Kattintson a **SAML-alapú hitelesítés** a bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="fbff4-184">Click on **SAML Authentication** from the left panel.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. <span data-ttu-id="fbff4-186">Az a **SAML-alapú hitelesítés** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fbff4-186">On the **SAML Authentication** dialog page, perform the following steps:</span></span>
   
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    ><span data-ttu-id="fbff4-188">SAML-alapú hitelesítést használ, vagy kell **ellenőrzött tartományához** vagy **portál URL-cím** beállítása.</span><span class="sxs-lookup"><span data-stu-id="fbff4-188">To use SAML based authentication, you should either have **verified domain** or **portal URL** setup.</span></span> <span data-ttu-id="fbff4-189">Beállíthatja, hogy a portál URL-cím elé egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="fbff4-189">You can set the portal URL with the unique name.</span></span>
    > 
    > 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    <span data-ttu-id="fbff4-191">a.</span><span class="sxs-lookup"><span data-stu-id="fbff4-191">a.</span></span> <span data-ttu-id="fbff4-192">A a **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="fbff4-192">In the **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="fbff4-193">b.</span><span class="sxs-lookup"><span data-stu-id="fbff4-193">b.</span></span> <span data-ttu-id="fbff4-194">A a **kijelentkezési** URL-cím beviteli mező, illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="fbff4-194">In the **Logout** URL textbox, paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fbff4-195">c.</span><span class="sxs-lookup"><span data-stu-id="fbff4-195">c.</span></span> <span data-ttu-id="fbff4-196">**Módosítsa jelszavát URL-címet** nem kötelező megadni, hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="fbff4-196">**Change Password URL** is optional so leave it blank.</span></span>

    <span data-ttu-id="fbff4-197">d.</span><span class="sxs-lookup"><span data-stu-id="fbff4-197">d.</span></span> <span data-ttu-id="fbff4-198">Kattintson a **fájl kulcs beszerzése** a letöltött tanúsítvány kiválasztása az Azure-portálon, és kattintson **nyitott** töltse fel a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="fbff4-198">Click on **Get Key From File** to select your downloaded certificate from Azure portal, and then click **Open** to upload the certificate.</span></span>

    <span data-ttu-id="fbff4-199">e.</span><span class="sxs-lookup"><span data-stu-id="fbff4-199">e.</span></span> <span data-ttu-id="fbff4-200">Mint **algoritmus**, jelölje be **RSA**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-200">As **Algorithm**, select **RSA**.</span></span>

    <span data-ttu-id="fbff4-201">f.</span><span class="sxs-lookup"><span data-stu-id="fbff4-201">f.</span></span> <span data-ttu-id="fbff4-202">Kattintson a **Ok** menti a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="fbff4-202">Click **Ok** to save the changes.</span></span>

> [!TIP]
> <span data-ttu-id="fbff4-203">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fbff4-203">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fbff4-204">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fbff4-204">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fbff4-205">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fbff4-205">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="fbff4-206">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbff4-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="fbff4-207">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fbff4-207">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fbff4-209">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbff4-209">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fbff4-210">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-210">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fbff4-212">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-212">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fbff4-214">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="fbff4-214">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fbff4-216">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fbff4-216">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fbff4-218">a.</span><span class="sxs-lookup"><span data-stu-id="fbff4-218">a.</span></span> <span data-ttu-id="fbff4-219">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-219">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fbff4-220">b.</span><span class="sxs-lookup"><span data-stu-id="fbff4-220">b.</span></span> <span data-ttu-id="fbff4-221">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fbff4-221">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fbff4-222">c.</span><span class="sxs-lookup"><span data-stu-id="fbff4-222">c.</span></span> <span data-ttu-id="fbff4-223">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-223">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fbff4-224">d.</span><span class="sxs-lookup"><span data-stu-id="fbff4-224">d.</span></span> <span data-ttu-id="fbff4-225">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-225">Click **Create**.</span></span>
 
### <a name="creating-a-skydesk-email-test-user"></a><span data-ttu-id="fbff4-226">SkyDesk E-mail tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fbff4-226">Creating a SkyDesk Email test user</span></span>

<span data-ttu-id="fbff4-227">Ebben a szakaszban hoz létre a felhasználó Britta Simon nevű SkyDesk e-mailben.</span><span class="sxs-lookup"><span data-stu-id="fbff4-227">In this section, you create a user called Britta Simon in SkyDesk Email.</span></span>

1. <span data-ttu-id="fbff4-228">Kattintson a **felhasználói hozzáférés** bal oldali panelen SkyDesk e-mailben, és írja be a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="fbff4-228">Click on **User Access** from the left panel in SkyDesk Email and then enter your username.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
><span data-ttu-id="fbff4-230">Felhasználók tömeges létrehozásához szükséges, ha szeretné-e lépjen kapcsolatba a [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/).</span><span class="sxs-lookup"><span data-stu-id="fbff4-230">If you need to create bulk users, you need to contact the [SkyDesk Email Client support team](https://www.skydesk.sg/support/).</span></span>


### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="fbff4-231">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="fbff4-231">Assigning the Azure AD test user</span></span>

<span data-ttu-id="fbff4-232">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó SkyDesk levelezéshez való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="fbff4-232">In this section, you enable Britta Simon to use Azure single sign-on by granting access to SkyDesk Email.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fbff4-234">**Britta Simon hozzárendelése SkyDesk e-mailek, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fbff4-234">**To assign Britta Simon to SkyDesk Email, perform the following steps:**</span></span>

1. <span data-ttu-id="fbff4-235">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-235">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fbff4-237">Az alkalmazások listában válassza ki a **SkyDesk E-mail**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-237">In the applications list, select **SkyDesk Email**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. <span data-ttu-id="fbff4-239">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fbff4-239">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fbff4-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fbff4-241">Click **Add** button.</span></span> <span data-ttu-id="fbff4-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbff4-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fbff4-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fbff4-244">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fbff4-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbff4-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fbff4-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fbff4-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="fbff4-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fbff4-247">Testing single sign-on</span></span>

<span data-ttu-id="fbff4-248">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fbff4-248">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="fbff4-249">Ha a hozzáférési panelen SkyDesk E-mail csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az SkyDesk E-mail alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="fbff4-249">When you click the SkyDesk Email tile in the Access Panel, you should get automatically signed-on to your SkyDesk Email application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fbff4-250">További források</span><span class="sxs-lookup"><span data-stu-id="fbff4-250">Additional resources</span></span>

* [<span data-ttu-id="fbff4-251">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fbff4-251">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fbff4-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fbff4-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

