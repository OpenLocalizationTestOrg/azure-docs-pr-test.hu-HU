---
title: "Oktatóanyag: Azure Active Directoryval integrált Wdesk |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Wdesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 06900a91-a326-4663-8ba6-69ae741a536e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/22/2017
ms.author: jeedes
ms.openlocfilehash: 37660b80cfb01d6a3105aea5ce248f1e03c46695
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-wdesk"></a><span data-ttu-id="dfd0a-103">Oktatóanyag: Azure Active Directoryval integrált Wdesk</span><span class="sxs-lookup"><span data-stu-id="dfd0a-103">Tutorial: Azure Active Directory integration with Wdesk</span></span>

<span data-ttu-id="dfd0a-104">Ebben az oktatóanyagban elsajátíthatja Wdesk integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="dfd0a-104">In this tutorial, you learn how to integrate Wdesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="dfd0a-105">Wdesk integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-105">Integrating Wdesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="dfd0a-106">Megadhatja a Wdesk hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="dfd0a-106">You can control in Azure AD who has access to Wdesk</span></span>
- <span data-ttu-id="dfd0a-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Wdesk (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="dfd0a-107">You can enable your users to automatically get signed-on to Wdesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="dfd0a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="dfd0a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="dfd0a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-109">If you want to know more details about SaaS app integration with Azure AD, see.</span></span> <span data-ttu-id="dfd0a-110">[Alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="dfd0a-110">[What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="dfd0a-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dfd0a-111">Prerequisites</span></span>

<span data-ttu-id="dfd0a-112">Konfigurálása az Azure AD-integrációs Wdesk, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-112">To configure Azure AD integration with Wdesk, you need the following items:</span></span>

- <span data-ttu-id="dfd0a-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="dfd0a-113">An Azure AD subscription</span></span>
- <span data-ttu-id="dfd0a-114">Egy Wdesk egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="dfd0a-114">A Wdesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="dfd0a-115">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-115">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="dfd0a-116">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-116">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="dfd0a-117">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-117">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="dfd0a-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="dfd0a-118">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="dfd0a-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-119">Scenario description</span></span>
<span data-ttu-id="dfd0a-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="dfd0a-121">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-121">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="dfd0a-122">A gyűjteményből Wdesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-122">Adding Wdesk from the gallery</span></span>
2. <span data-ttu-id="dfd0a-123">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dfd0a-123">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-wdesk-from-the-gallery"></a><span data-ttu-id="dfd0a-124">A gyűjteményből Wdesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-124">Adding Wdesk from the gallery</span></span>
<span data-ttu-id="dfd0a-125">Az Azure AD integrálása a Wdesk konfigurálásához kell hozzáadnia Wdesk a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-125">To configure the integration of Wdesk into Azure AD, you need to add Wdesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="dfd0a-126">**A gyűjteményből Wdesk hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfd0a-126">**To add Wdesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd0a-127">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-127">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="dfd0a-129">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-129">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="dfd0a-130">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-130">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="dfd0a-132">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-132">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="dfd0a-134">Írja be a keresőmezőbe, **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-134">In the search box, type **Wdesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_search.png)

5. <span data-ttu-id="dfd0a-136">Az eredmények panelen válassza ki a **Wdesk**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-136">In the results panel, select **Wdesk**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="dfd0a-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="dfd0a-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="dfd0a-139">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján Wdesk</span><span class="sxs-lookup"><span data-stu-id="dfd0a-139">In this section, you configure and test Azure AD single sign-on with Wdesk based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="dfd0a-140">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Wdesk a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-140">For single sign-on to work, Azure AD needs to know what the counterpart user in Wdesk is to a user in Azure AD.</span></span> <span data-ttu-id="dfd0a-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Wdesk közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-141">In other words, a link relationship between an Azure AD user and the related user in Wdesk needs to be established.</span></span>

<span data-ttu-id="dfd0a-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Wdesk a.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Wdesk.</span></span>

<span data-ttu-id="dfd0a-143">Az Azure AD egyszeri bejelentkezést a Wdesk tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-143">To configure and test Azure AD single sign-on with Wdesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="dfd0a-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="dfd0a-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="dfd0a-146">**[Wdesk tesztfelhasználó létrehozása](#creating-a-wdesk-test-user)**  - való Britta Simon valami Wdesk, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-146">**[Creating a Wdesk test user](#creating-a-wdesk-test-user)** - to have a counterpart of Britta Simon in Wdesk that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="dfd0a-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="dfd0a-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="dfd0a-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="dfd0a-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Wdesk alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Wdesk application.</span></span>

<span data-ttu-id="dfd0a-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés Wdesk, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfd0a-151">**To configure Azure AD single sign-on with Wdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd0a-152">Az Azure portálon a a **Wdesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-152">In the Azure portal, on the **Wdesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="dfd0a-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_samlbase.png)

3. <span data-ttu-id="dfd0a-156">Az a **Wdesk tartomány és az URL-címek** szakaszban, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett módban hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-156">On the **Wdesk Domain and URLs** section, If you wish to configure the application in **IDP** initiated mode perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url.png)

    <span data-ttu-id="dfd0a-158">a.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-158">a.</span></span> <span data-ttu-id="dfd0a-159">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="dfd0a-159">In the **Identifier** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/metadata/<instancename>`</span></span>

    <span data-ttu-id="dfd0a-160">b.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-160">b.</span></span> <span data-ttu-id="dfd0a-161">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="dfd0a-161">In the **Reply URL** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/saml/sp/consumer/<instancename>`</span></span>

4. <span data-ttu-id="dfd0a-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-162">Check **Show advanced URL settings**.</span></span> <span data-ttu-id="dfd0a-163">Ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód, hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-163">If you wish to configure the application in **SP** initiated mode, perform the following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_url1.png)

    <span data-ttu-id="dfd0a-165">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="dfd0a-165">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<subdomain>.wdesk.com/auth/login/saml/<instancename>`</span></span>
     
    > [!NOTE] 
    > <span data-ttu-id="dfd0a-166">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-166">These values are not real.</span></span> <span data-ttu-id="dfd0a-167">Frissítheti ezeket az értékeket a tényleges azonosítója, válasz URL-CÍMEN és bejelentkezési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-167">Update these values with the actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="dfd0a-168">Ezek az értékek első WDesk portálról az SSO konfigurálásakor.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-168">You get these values from WDesk portal when you configure the SSO.</span></span> 
  
5. <span data-ttu-id="dfd0a-169">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-169">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_certificate.png) 

6. <span data-ttu-id="dfd0a-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="dfd0a-173">Egy másik webes böngészőablakban, jelentkezzen be a biztonsági-rendszergazdájaként Wdesk.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-173">In a different web browser window, login to Wdesk as a Security Administrator.</span></span>

8. <span data-ttu-id="dfd0a-174">Kattintson a bal alsó **Admin** válassza **Fiókadminisztrátor**:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-174">In the bottom left, click **Admin** and choose **Account Admin**:</span></span>
 
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

9. <span data-ttu-id="dfd0a-176">Wdesk rendszergazda, lépjen a **biztonsági**, majd **SAML** > **SAML beállítások**:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-176">In Wdesk Admin, navigate to **Security**, then **SAML** > **SAML Settings**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig2.png)

10. <span data-ttu-id="dfd0a-178">A **általános beállítások**, ellenőrizze a **SAML-alapú egyszeri bejelentkezés engedélyezése**:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-178">Under **General Settings**, check the **Enable SAML Single Sign On**:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig3.png)

11. <span data-ttu-id="dfd0a-180">A **szolgáltató részletei**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-180">Under **Service Provider Details**, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig4.png)

      <span data-ttu-id="dfd0a-182">a.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-182">a.</span></span> <span data-ttu-id="dfd0a-183">Másolás a **bejelentkezési URL-cím** és illessze be **bejelentkezési URL-cím** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-183">Copy the **Login URL** and paste it in **Sign-on Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="dfd0a-184">b.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-184">b.</span></span> <span data-ttu-id="dfd0a-185">Másolás a **metaadatainak URL-címét** és illessze be **azonosító** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-185">Copy the **Metadata Url** and paste it in **Identifier** textbox on Azure portal.</span></span>
       
      <span data-ttu-id="dfd0a-186">c.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-186">c.</span></span> <span data-ttu-id="dfd0a-187">Másolás a **ügyfél URL-címe** és illessze be **válasz URL-címen** szövegmező Azure-portál.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-187">Copy the **Consumer url** and paste it in **Reply Url** textbox on Azure portal.</span></span>
   
      <span data-ttu-id="dfd0a-188">d.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-188">d.</span></span> <span data-ttu-id="dfd0a-189">Kattintson a **mentése** menti a módosításokat az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-189">Click **Save** on Azure portal to save the changes.</span></span>      

12. <span data-ttu-id="dfd0a-190">Kattintson a **IdP beállításainak konfigurálása** megnyitásához **IdP beállításainak szerkesztése** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-190">Click **Configure IdP Settings** to open **Edit IdP Settings** dialog.</span></span> <span data-ttu-id="dfd0a-191">Kattintson a **Choose File** kereséséhez a **Metadata.xml** mentett fájlt, Azure-portálon, majd töltse fel.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-191">Click **Choose File** to locate the **Metadata.xml** file you saved from Azure portal, then upload it.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig5.png)
  
13. <span data-ttu-id="dfd0a-193">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-193">Click **Save changes**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfigsavebutton.png)

> [!TIP]
> <span data-ttu-id="dfd0a-195">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="dfd0a-195">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="dfd0a-196">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-196">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="dfd0a-197">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="dfd0a-197">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="dfd0a-198">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-198">Creating an Azure AD test user</span></span>
<span data-ttu-id="dfd0a-199">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-199">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="dfd0a-201">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfd0a-201">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd0a-202">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-202">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="dfd0a-204">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-204">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="dfd0a-206">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-206">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="dfd0a-208">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-208">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="dfd0a-210">a.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-210">a.</span></span> <span data-ttu-id="dfd0a-211">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-211">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="dfd0a-212">b.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-212">b.</span></span> <span data-ttu-id="dfd0a-213">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-213">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="dfd0a-214">c.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-214">c.</span></span> <span data-ttu-id="dfd0a-215">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-215">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="dfd0a-216">d.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-216">d.</span></span> <span data-ttu-id="dfd0a-217">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-217">Click **Create**.</span></span>
 
### <a name="creating-a-wdesk-test-user"></a><span data-ttu-id="dfd0a-218">Wdesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-218">Creating a Wdesk test user</span></span>

<span data-ttu-id="dfd0a-219">Ahhoz, hogy az Azure AD-felhasználók Wdesk bejelentkezni, akkor ki kell építenie a Wdesk.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-219">To enable Azure AD users to log in to Wdesk, they must be provisioned into Wdesk.</span></span> <span data-ttu-id="dfd0a-220">A Wdesk egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-220">In Wdesk, provisioning is a manual task.</span></span>

<span data-ttu-id="dfd0a-221">**Felhasználói fiók létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfd0a-221">**To provision a user account, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd0a-222">Jelentkezzen be a biztonsági-rendszergazdájaként Wdesk.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-222">Log in to Wdesk as a Security Administrator.</span></span>
2. <span data-ttu-id="dfd0a-223">Navigáljon a **Admin** > **rendszergazdai fiók**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-223">Navigate to **Admin** > **Account Admin**.</span></span>

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_ssoconfig1.png)

3. <span data-ttu-id="dfd0a-225">Kattintson a **tagok** alatt **személyek**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-225">Click **Members** under **People**.</span></span>

4. <span data-ttu-id="dfd0a-226">Most kattintson **tag hozzáadása** megnyitásához **tag hozzáadása** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-226">Now click **Add Member** to open **Add Member** dialog box.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser1.png)  

5. <span data-ttu-id="dfd0a-228">A **felhasználói** szöveg mezőbe írja be például a felhasználó felhasználóneve  **brittasimon@contoso.com**  kattintson **Folytatás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-228">In **User** text box, enter the username of user like **brittasimon@contoso.com** and click **Continue** button.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser3.png)

6.  <span data-ttu-id="dfd0a-230">Adja meg az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="dfd0a-230">Enter the details as shown below:</span></span>
  
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser4.png)
 
    <span data-ttu-id="dfd0a-232">a.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-232">a.</span></span> <span data-ttu-id="dfd0a-233">A **E-mail** szöveg mezőbe írja be például a felhasználó e-mail  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="dfd0a-233">In **E-mail** text box, enter the email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="dfd0a-234">b.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-234">b.</span></span> <span data-ttu-id="dfd0a-235">A **Utónév** szöveg mezőbe írja be például a felhasználó utónevét **Britta**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-235">In **First Name** text box, enter the first name of user like **Britta**.</span></span>

    <span data-ttu-id="dfd0a-236">c.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-236">c.</span></span> <span data-ttu-id="dfd0a-237">A **Vezetéknév** szöveg mezőbe írja be például a felhasználó vezetékneve **Simon**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-237">In **Last Name** text box, enter the last name of user like **Simon**.</span></span>

7. <span data-ttu-id="dfd0a-238">Kattintson a **mentése tag** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-238">Click **Save Member** button.</span></span>  

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-wdesk-tutorial/createuser5.png)

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="dfd0a-240">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="dfd0a-240">Assigning the Azure AD test user</span></span>

<span data-ttu-id="dfd0a-241">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Wdesk Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-241">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Wdesk.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="dfd0a-243">**Britta Simon hozzárendelése Wdesk, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="dfd0a-243">**To assign Britta Simon to Wdesk, perform the following steps:**</span></span>

1. <span data-ttu-id="dfd0a-244">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-244">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="dfd0a-246">Az alkalmazások listában válassza ki a **Wdesk**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-246">In the applications list, select **Wdesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-wdesk-tutorial/tutorial_wdesk_app.png) 

3. <span data-ttu-id="dfd0a-248">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-248">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="dfd0a-250">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-250">Click **Add** button.</span></span> <span data-ttu-id="dfd0a-251">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-251">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="dfd0a-253">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-253">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="dfd0a-254">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-254">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="dfd0a-255">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-255">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="dfd0a-256">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="dfd0a-256">Testing single sign-on</span></span>

<span data-ttu-id="dfd0a-257">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-257">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="dfd0a-258">Ha a hozzáférési panelen Wdesk csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Wdesk alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="dfd0a-258">When you click the Wdesk tile in the Access Panel, you should get automatically signed-on to your Wdesk application.</span></span>
<span data-ttu-id="dfd0a-259">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="dfd0a-259">For more information about the Access Panel, see [introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="dfd0a-260">További források</span><span class="sxs-lookup"><span data-stu-id="dfd0a-260">Additional resources</span></span>

* [<span data-ttu-id="dfd0a-261">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="dfd0a-261">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="dfd0a-262">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="dfd0a-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-wdesk-tutorial/tutorial_general_203.png

