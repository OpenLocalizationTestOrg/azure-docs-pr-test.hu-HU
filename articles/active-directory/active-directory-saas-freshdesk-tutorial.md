---
title: "Oktatóanyag: Azure Active Directoryval integrált FreshDesk |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és FreshDesk között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c2a3e5aa-7b5a-4fe4-9285-45dbe6e8efcc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: f4b47e345a40b64f69ad8a4618564662b4a6c879
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-freshdesk"></a><span data-ttu-id="177f5-103">Oktatóanyag: Azure Active Directoryval integrált FreshDesk</span><span class="sxs-lookup"><span data-stu-id="177f5-103">Tutorial: Azure Active Directory integration with FreshDesk</span></span>

<span data-ttu-id="177f5-104">Ebben az oktatóanyagban elsajátíthatja FreshDesk integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="177f5-104">In this tutorial, you learn how to integrate FreshDesk with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="177f5-105">FreshDesk integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="177f5-105">Integrating FreshDesk with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="177f5-106">Megadhatja a FreshDesk hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="177f5-106">You can control in Azure AD who has access to FreshDesk</span></span>
- <span data-ttu-id="177f5-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett FreshDesk (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="177f5-107">You can enable your users to automatically get signed-on to FreshDesk (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="177f5-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="177f5-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="177f5-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="177f5-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="177f5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="177f5-110">Prerequisites</span></span>

<span data-ttu-id="177f5-111">Konfigurálása az Azure AD-integrációs FreshDesk, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="177f5-111">To configure Azure AD integration with FreshDesk, you need the following items:</span></span>

- <span data-ttu-id="177f5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="177f5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="177f5-113">Egy FreshDesk egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="177f5-113">A FreshDesk single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="177f5-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="177f5-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="177f5-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="177f5-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="177f5-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="177f5-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="177f5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="177f5-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="177f5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="177f5-118">Scenario description</span></span>
<span data-ttu-id="177f5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="177f5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="177f5-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="177f5-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="177f5-121">A gyűjteményből FreshDesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="177f5-121">Adding FreshDesk from the gallery</span></span>
2. <span data-ttu-id="177f5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="177f5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-freshdesk-from-the-gallery"></a><span data-ttu-id="177f5-123">A gyűjteményből FreshDesk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="177f5-123">Adding FreshDesk from the gallery</span></span>
<span data-ttu-id="177f5-124">Az Azure AD integrálása a FreshDesk konfigurálásához kell hozzáadnia FreshDesk a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="177f5-124">To configure the integration of FreshDesk into Azure AD, you need to add FreshDesk from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="177f5-125">**A gyűjteményből FreshDesk hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="177f5-125">**To add FreshDesk from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="177f5-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="177f5-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="177f5-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="177f5-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="177f5-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="177f5-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="177f5-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="177f5-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="177f5-133">Írja be a keresőmezőbe, **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="177f5-133">In the search box, type **FreshDesk**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_search.png)

5. <span data-ttu-id="177f5-135">Az eredmények panelen válassza ki a **FreshDesk**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="177f5-135">In the results panel, select **FreshDesk**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="177f5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="177f5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="177f5-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="177f5-138">In this section, you configure and test Azure AD single sign-on with FreshDesk based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="177f5-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó FreshDesk a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="177f5-139">For single sign-on to work, Azure AD needs to know what the counterpart user in FreshDesk is to a user in Azure AD.</span></span> <span data-ttu-id="177f5-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a FreshDesk közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="177f5-140">In other words, a link relationship between an Azure AD user and the related user in FreshDesk needs to be established.</span></span>

<span data-ttu-id="177f5-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** FreshDesk a.</span><span class="sxs-lookup"><span data-stu-id="177f5-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in FreshDesk.</span></span>

<span data-ttu-id="177f5-142">Az Azure AD egyszeri bejelentkezést a FreshDesk tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="177f5-142">To configure and test Azure AD single sign-on with FreshDesk, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="177f5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="177f5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="177f5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="177f5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="177f5-145">**[FreshDesk tesztfelhasználó létrehozása](#creating-a-freshdesk-test-user)**  - való Britta Simon valami FreshDesk, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="177f5-145">**[Creating a FreshDesk test user](#creating-a-freshdesk-test-user)** - to have a counterpart of Britta Simon in FreshDesk that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="177f5-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="177f5-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="177f5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="177f5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="177f5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="177f5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="177f5-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az FreshDesk alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="177f5-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your FreshDesk application.</span></span>

<span data-ttu-id="177f5-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés FreshDesk, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="177f5-150">**To configure Azure AD single sign-on with FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="177f5-151">Az Azure felügyeleti portálján a a **FreshDesk** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="177f5-151">In the Azure Management portal, on the **FreshDesk** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="177f5-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="177f5-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_samlbase.png)

3. <span data-ttu-id="177f5-155">A a **FreshDesk tartomány és az URL-címek** területen írja be a **bejelentkezési URL-cím** mint: `https://<tenant-name>.freshdesk.com` vagy bármely más érték Freshdesk javasolt.</span><span class="sxs-lookup"><span data-stu-id="177f5-155">On the **FreshDesk Domain and URLs** section, please enter the **Sign-on URL** as: `https://<tenant-name>.freshdesk.com` or any other value Freshdesk has suggested.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_url.png)

    > [!NOTE] 
    > <span data-ttu-id="177f5-157">Ne feledje, hogy ez a nem a tényleges érték.</span><span class="sxs-lookup"><span data-stu-id="177f5-157">Please note that this is not the real value.</span></span> <span data-ttu-id="177f5-158">Frissítse az értéket a tényleges bejelentkezési URL-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="177f5-158">You have to update the value with the actual Sign-on URL.</span></span> <span data-ttu-id="177f5-159">Ügyfél [FreshDesk ügyfél-támogatási csoport](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="177f5-159">Contact [FreshDesk Client support team](https://freshdesk.com/helpdesk-software?utm_source=Google-AdWords&utm_medium=Search-IND-Brand&utm_campaign=Search-IND-Brand&utm_term=freshdesk&device=c&gclid=COSH2_LH7NICFVUDvAodBPgBZg) to get this value.</span></span>  

4. <span data-ttu-id="177f5-160">Az a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a tanúsítványt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="177f5-160">On the **SAML Signing Certificate** section, click **Certificate** and then save the certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_certificate.png) 

5. <span data-ttu-id="177f5-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="177f5-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="177f5-164">A a **FreshDesk konfigurációs** kattintson **konfigurálása FreshDesk** konfigurálása bejelentkezés ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="177f5-164">On the **FreshDesk Configuration** section, click **Configure FreshDesk** to open Configure sign-on window.</span></span> <span data-ttu-id="177f5-165">Másolja a SAML-alapú egyszeri bejelentkezési URL-címe és Sign-Out URL-CÍMÉT a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="177f5-165">Copy the SAML Single Sign-On Service URL and Sign-Out URL from the **Quick Reference** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_configure.png)

7. <span data-ttu-id="177f5-167">Egy másik webes böngészőablakban jelentkezzen be a Freshdesk vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="177f5-167">In a different web browser window, log into your Freshdesk company site as an administrator.</span></span>

8. <span data-ttu-id="177f5-168">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="177f5-168">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="177f5-169">![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="177f5-169">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776768.png "Admin")</span></span>

9. <span data-ttu-id="177f5-170">Az a **általános beállítások** lapra, majd **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="177f5-170">In the **General Settings** tab, click **Security**.</span></span>
   
   <span data-ttu-id="177f5-171">![Biztonsági](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="177f5-171">![Security](./media/active-directory-saas-freshdesk-tutorial/IC776769.png "Security")</span></span>

10. <span data-ttu-id="177f5-172">Az a **biztonsági** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="177f5-172">In the **Security** section, perform the following steps:</span></span>
   
    <span data-ttu-id="177f5-173">![Egyszeri bejelentkezés](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="177f5-173">![Single Sign On](./media/active-directory-saas-freshdesk-tutorial/IC776770.png "Single Sign On")</span></span>
   
    <span data-ttu-id="177f5-174">a.</span><span class="sxs-lookup"><span data-stu-id="177f5-174">a.</span></span> <span data-ttu-id="177f5-175">A **egyszeri bejelentkezés (SSO)**, jelölje be **a**.</span><span class="sxs-lookup"><span data-stu-id="177f5-175">For **Single Sign On (SSO)**, select **On**.</span></span>

    <span data-ttu-id="177f5-176">b.</span><span class="sxs-lookup"><span data-stu-id="177f5-176">b.</span></span> <span data-ttu-id="177f5-177">Válassza ki **SAML SSO**.</span><span class="sxs-lookup"><span data-stu-id="177f5-177">Select **SAML SSO**.</span></span>

    <span data-ttu-id="177f5-178">c.</span><span class="sxs-lookup"><span data-stu-id="177f5-178">c.</span></span> <span data-ttu-id="177f5-179">Típus a **SAML-alapú egyszeri bejelentkezési URL-címe** másolta az Azure-portálon a **SAML bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="177f5-179">Type the **SAML Single Sign-On Service URL** you copied from Azure portal into the **SAML Login URL** textbox.</span></span>

    <span data-ttu-id="177f5-180">d.</span><span class="sxs-lookup"><span data-stu-id="177f5-180">d.</span></span> <span data-ttu-id="177f5-181">Típus a **Sign-Out URL-cím** másolta az Azure-portálon a **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="177f5-181">Type the **Sign-Out URL**  you copied from Azure portal into the **Logout URL** textbox.</span></span>

    <span data-ttu-id="177f5-182">e.</span><span class="sxs-lookup"><span data-stu-id="177f5-182">e.</span></span> <span data-ttu-id="177f5-183">Másolás a **ujjlenyomat** Azure portálról letöltött tanúsítvány értéket, és illessze be azt a **biztonsági tanúsítvány-ujjlenyomat** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="177f5-183">Copy the **Thumbprint** value from the downloaded certificate from Azure portal and paste it into the **Security Certificate Fingerprint** textbox.</span></span>  
 
    >[!TIP]
    ><span data-ttu-id="177f5-184">További részletekért lásd: [hogyan lehet lekérni egy tanúsítvány-ujjlenyomat értékének](http://youtu.be/YKQF266SAxI).</span><span class="sxs-lookup"><span data-stu-id="177f5-184">For more details, see [How to retrieve a certificate's thumbprint value](http://youtu.be/YKQF266SAxI).</span></span> 
    
    <span data-ttu-id="177f5-185">f.</span><span class="sxs-lookup"><span data-stu-id="177f5-185">f.</span></span> <span data-ttu-id="177f5-186">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="177f5-186">Click **Save**.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="177f5-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="177f5-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="177f5-188">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="177f5-188">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="177f5-190">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="177f5-190">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="177f5-191">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="177f5-191">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="177f5-193">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="177f5-193">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="177f5-195">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="177f5-195">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="177f5-197">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="177f5-197">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-freshdesk-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="177f5-199">a.</span><span class="sxs-lookup"><span data-stu-id="177f5-199">a.</span></span> <span data-ttu-id="177f5-200">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="177f5-200">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="177f5-201">b.</span><span class="sxs-lookup"><span data-stu-id="177f5-201">b.</span></span> <span data-ttu-id="177f5-202">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="177f5-202">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="177f5-203">c.</span><span class="sxs-lookup"><span data-stu-id="177f5-203">c.</span></span> <span data-ttu-id="177f5-204">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="177f5-204">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="177f5-205">d.</span><span class="sxs-lookup"><span data-stu-id="177f5-205">d.</span></span> <span data-ttu-id="177f5-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="177f5-206">Click **Create**.</span></span>
 
### <a name="creating-a-freshdesk-test-user"></a><span data-ttu-id="177f5-207">FreshDesk tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="177f5-207">Creating a FreshDesk test user</span></span>

<span data-ttu-id="177f5-208">Ahhoz, hogy az Azure AD-felhasználók FreshDesk bejelentkezni, akkor ki kell építenie FreshDesk be.</span><span class="sxs-lookup"><span data-stu-id="177f5-208">In order to enable Azure AD users to log into FreshDesk, they must be provisioned into FreshDesk.</span></span>  
<span data-ttu-id="177f5-209">FreshDesk, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="177f5-209">In the case of FreshDesk, provisioning is a manual task.</span></span>

<span data-ttu-id="177f5-210">**A felhasználói fiókok létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="177f5-210">**To provision a user accounts, perform the following steps:**</span></span>

1. <span data-ttu-id="177f5-211">Jelentkezzen be a **Freshdesk** bérlő.</span><span class="sxs-lookup"><span data-stu-id="177f5-211">Log in to your **Freshdesk** tenant.</span></span>
2. <span data-ttu-id="177f5-212">Kattintson a felső menüben **Admin**.</span><span class="sxs-lookup"><span data-stu-id="177f5-212">In the menu on the top, click **Admin**.</span></span>
   
   <span data-ttu-id="177f5-213">![Felügyeleti](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "rendszergazda")</span><span class="sxs-lookup"><span data-stu-id="177f5-213">![Admin](./media/active-directory-saas-freshdesk-tutorial/IC776772.png "Admin")</span></span>

3. <span data-ttu-id="177f5-214">Az a **általános beállítások** lapra, majd **ügynökök**.</span><span class="sxs-lookup"><span data-stu-id="177f5-214">In the **General Settings** tab, click **Agents**.</span></span>
   
   <span data-ttu-id="177f5-215">![Ügynökök](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "ügynökök")</span><span class="sxs-lookup"><span data-stu-id="177f5-215">![Agents](./media/active-directory-saas-freshdesk-tutorial/IC776773.png "Agents")</span></span>

4. <span data-ttu-id="177f5-216">Kattintson a **új ügynök**.</span><span class="sxs-lookup"><span data-stu-id="177f5-216">Click **New Agent**.</span></span>
   
    <span data-ttu-id="177f5-217">![Az új ügynök](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "új ügynök")</span><span class="sxs-lookup"><span data-stu-id="177f5-217">![New Agent](./media/active-directory-saas-freshdesk-tutorial/IC776774.png "New Agent")</span></span>

5. <span data-ttu-id="177f5-218">Az ügynök adatait párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="177f5-218">On the Agent Information dialog, perform the following steps:</span></span>
   
   <span data-ttu-id="177f5-219">![Az ügynök adatait](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "ügynök adatait")</span><span class="sxs-lookup"><span data-stu-id="177f5-219">![Agent Information](./media/active-directory-saas-freshdesk-tutorial/IC776775.png "Agent Information")</span></span>
   
   <span data-ttu-id="177f5-220">a.</span><span class="sxs-lookup"><span data-stu-id="177f5-220">a.</span></span> <span data-ttu-id="177f5-221">Az a **teljes nevét** szövegmező, írja be a kiépítés kívánt Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="177f5-221">In the **Full Name** textbox, type the name of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="177f5-222">b.</span><span class="sxs-lookup"><span data-stu-id="177f5-222">b.</span></span> <span data-ttu-id="177f5-223">Az a **E-mail** szövegmezőhöz típus az Azure AD e-mail címe rendelkezés kívánt Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="177f5-223">In the **Email** textbox, type the Azure AD email address of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="177f5-224">c.</span><span class="sxs-lookup"><span data-stu-id="177f5-224">c.</span></span> <span data-ttu-id="177f5-225">Az a **cím** szövegmező, írja be a kívánt rendelkezés Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="177f5-225">In the **Title** textbox, type the title of the Azure AD account you want to provision.</span></span>

   <span data-ttu-id="177f5-226">d.</span><span class="sxs-lookup"><span data-stu-id="177f5-226">d.</span></span> <span data-ttu-id="177f5-227">Válassza ki **ügynökök szerepkör**, és kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="177f5-227">Select **Agents role**, and then click **Assign**.</span></span>
       
   <span data-ttu-id="177f5-228">e.</span><span class="sxs-lookup"><span data-stu-id="177f5-228">e.</span></span> <span data-ttu-id="177f5-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="177f5-229">Click **Save**.</span></span>     
   
    >[!NOTE]
    ><span data-ttu-id="177f5-230">Az Azure AD fióktulajdonos kap egy e-mailt, amely tartalmaz egy hivatkozást a fiók megerősítéséhez, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="177f5-230">The Azure AD account holder will get an email that includes a link to confirm the account before it is activated.</span></span> 
    > 
    
    >[!NOTE]
    ><span data-ttu-id="177f5-231">Bármely más Freshdesk felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Freshdesk által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="177f5-231">You can use any other Freshdesk user account creation tools or APIs provided by Freshdesk to provision AAD user accounts.</span></span> <span data-ttu-id="177f5-232">a FreshDesk.</span><span class="sxs-lookup"><span data-stu-id="177f5-232">to FreshDesk.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="177f5-233">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="177f5-233">Assigning the Azure AD test user</span></span>

<span data-ttu-id="177f5-234">Ebben a szakaszban engedélyezze Britta Simon nyújtó saját mezőben Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="177f5-234">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to Box.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="177f5-236">**Britta Simon hozzárendelése FreshDesk, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="177f5-236">**To assign Britta Simon to FreshDesk, perform the following steps:**</span></span>

1. <span data-ttu-id="177f5-237">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="177f5-237">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="177f5-239">Az alkalmazások listában válassza ki a **FreshDesk**.</span><span class="sxs-lookup"><span data-stu-id="177f5-239">In the applications list, select **FreshDesk**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-freshdesk-tutorial/tutorial_freshdesk_app.png) 

3. <span data-ttu-id="177f5-241">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="177f5-241">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="177f5-243">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="177f5-243">Click **Add** button.</span></span> <span data-ttu-id="177f5-244">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="177f5-244">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="177f5-246">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="177f5-246">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="177f5-247">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="177f5-247">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="177f5-248">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="177f5-248">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="177f5-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="177f5-249">Testing single sign-on</span></span>

<span data-ttu-id="177f5-250">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="177f5-250">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="177f5-251">Ha a hozzáférési panelen FreshDesk csempére kattint, az beszerzése bejelentkezett FreshDesk Alkalmazásmódosítások bejelentkezési oldalt szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="177f5-251">When you click the FreshDesk tile in the Access Panel, you should get login page to get signed-on to your FreshDesk application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="177f5-252">További források</span><span class="sxs-lookup"><span data-stu-id="177f5-252">Additional resources</span></span>

* [<span data-ttu-id="177f5-253">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="177f5-253">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="177f5-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="177f5-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-freshdesk-tutorial/tutorial_general_203.png

