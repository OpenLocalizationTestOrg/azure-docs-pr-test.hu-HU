---
title: "Oktatóanyag: Azure Active Directoryval integrált ScreenSteps |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ScreenSteps között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 4563fe94-a88f-4895-a07f-79df44889cf9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: b6ded8ba1adf03fdccbdb7573c09fae1857c8b16
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-screensteps"></a><span data-ttu-id="74612-103">Oktatóanyag: Azure Active Directoryval integrált ScreenSteps</span><span class="sxs-lookup"><span data-stu-id="74612-103">Tutorial: Azure Active Directory integration with ScreenSteps</span></span>

<span data-ttu-id="74612-104">Ebben az oktatóanyagban elsajátíthatja ScreenSteps integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="74612-104">In this tutorial, you learn how to integrate ScreenSteps with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="74612-105">ScreenSteps integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="74612-105">Integrating ScreenSteps with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="74612-106">Az Azure AD, aki hozzáfér ScreenSteps szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="74612-106">You can control in Azure AD who has access to ScreenSteps.</span></span>
- <span data-ttu-id="74612-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ScreenSteps (egyszeri bejelentkezés) számára a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="74612-107">You can enable your users to automatically get signed-on to ScreenSteps (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="74612-108">A fiók egyetlen központi helyen – az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="74612-108">You can manage your accounts in one central location - the Azure portal.</span></span>

<span data-ttu-id="74612-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="74612-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74612-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="74612-110">Prerequisites</span></span>

<span data-ttu-id="74612-111">Konfigurálása az Azure AD-integrációs ScreenSteps, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="74612-111">To configure Azure AD integration with ScreenSteps, you need the following items:</span></span>

- <span data-ttu-id="74612-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="74612-112">An Azure AD subscription</span></span>
- <span data-ttu-id="74612-113">Egy ScreenSteps egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="74612-113">A ScreenSteps single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="74612-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="74612-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="74612-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="74612-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="74612-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="74612-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="74612-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="74612-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="74612-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="74612-118">Scenario description</span></span>
<span data-ttu-id="74612-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="74612-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="74612-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="74612-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="74612-121">A gyűjteményből ScreenSteps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="74612-121">Adding ScreenSteps from the gallery</span></span>
2. <span data-ttu-id="74612-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="74612-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-screensteps-from-the-gallery"></a><span data-ttu-id="74612-123">A gyűjteményből ScreenSteps hozzáadása</span><span class="sxs-lookup"><span data-stu-id="74612-123">Adding ScreenSteps from the gallery</span></span>
<span data-ttu-id="74612-124">Az Azure AD integrálása a ScreenSteps konfigurálásához kell hozzáadnia ScreenSteps a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="74612-124">To configure the integration of ScreenSteps into Azure AD, you need to add ScreenSteps from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="74612-125">**A gyűjteményből ScreenSteps hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="74612-125">**To add ScreenSteps from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="74612-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="74612-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Az Azure Active Directory gomb][1]

2. <span data-ttu-id="74612-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="74612-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="74612-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="74612-129">Then go to **All applications**.</span></span>

    ![A vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="74612-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Az új alkalmazás gomb][3]

4. <span data-ttu-id="74612-133">Írja be a keresőmezőbe, **ScreenSteps**, jelölje be **ScreenSteps** eredmény panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="74612-133">In the search box, type **ScreenSteps**, select **ScreenSteps** from result panel then click **Add** button to add the application.</span></span>

    ![Az eredménylistában ScreenSteps](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="74612-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="74612-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="74612-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ScreenSteps.</span><span class="sxs-lookup"><span data-stu-id="74612-136">In this section, you configure and test Azure AD single sign-on with ScreenSteps based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="74612-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ScreenSteps a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="74612-137">For single sign-on to work, Azure AD needs to know what the counterpart user in ScreenSteps is to a user in Azure AD.</span></span> <span data-ttu-id="74612-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ScreenSteps közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="74612-138">In other words, a link relationship between an Azure AD user and the related user in ScreenSteps needs to be established.</span></span>

<span data-ttu-id="74612-139">ScreenSteps, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="74612-139">In ScreenSteps, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="74612-140">Az Azure AD egyszeri bejelentkezést a ScreenSteps tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="74612-140">To configure and test Azure AD single sign-on with ScreenSteps, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="74612-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="74612-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="74612-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="74612-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="74612-143">**[ScreenSteps tesztfelhasználó létrehozása](#create-a-screensteps-test-user)**  - való Britta Simon valami ScreenSteps, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="74612-143">**[Create a ScreenSteps test user](#create-a-screensteps-test-user)** - to have a counterpart of Britta Simon in ScreenSteps that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="74612-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="74612-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="74612-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="74612-145">**[Test single sign-on](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="74612-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="74612-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="74612-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az ScreenSteps alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="74612-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your ScreenSteps application.</span></span>

<span data-ttu-id="74612-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés ScreenSteps, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="74612-148">**To configure Azure AD single sign-on with ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="74612-149">Az Azure portálon a a **ScreenSteps** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="74612-149">In the Azure portal, on the **ScreenSteps** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="74612-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="74612-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_samlbase.png)

3. <span data-ttu-id="74612-153">Az a **ScreenSteps tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="74612-153">On the **ScreenSteps Domain and URLs** section, perform the following steps:</span></span>

    ![Az egyszeri bejelentkezés információk ScreenSteps tartomány és az URL-címek](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_url.png)

    <span data-ttu-id="74612-155">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<tenantname>.ScreenSteps.com`</span><span class="sxs-lookup"><span data-stu-id="74612-155">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<tenantname>.ScreenSteps.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="74612-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="74612-156">This value is not real.</span></span> <span data-ttu-id="74612-157">A tényleges bejelentkezési URL-cím, az oktatóanyag későbbi részében ismertetett frissítse ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="74612-157">Update this value with the actual Sign-On URL, which is explained later in this tutorial.</span></span> 

4. <span data-ttu-id="74612-158">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="74612-158">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![A tanúsítvány letöltési hivatkozását](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_certificate.png) 

5. <span data-ttu-id="74612-160">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-160">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-screensteps-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="74612-162">A a **ScreenSteps konfigurációs** kattintson **konfigurálása ScreenSteps** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="74612-162">On the **ScreenSteps Configuration** section, click **Configure ScreenSteps** to open **Configure sign-on** window.</span></span> <span data-ttu-id="74612-163">Másolás a **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="74612-163">Copy the **Sign-Out URL and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![ScreenSteps konfiguráció](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_configure.png) 

7. <span data-ttu-id="74612-165">Egy másik webes böngészőablakban jelentkezzen be a ScreenSteps vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="74612-165">In a different web browser window, log into your ScreenSteps company site as an administrator.</span></span>

8. <span data-ttu-id="74612-166">Kattintson a **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="74612-166">Click **Account Settings**.</span></span>

    <span data-ttu-id="74612-167">![Fiókkezelés](./media/active-directory-saas-screensteps-tutorial/ic778523.png "fiókkezelés")</span><span class="sxs-lookup"><span data-stu-id="74612-167">![Account management](./media/active-directory-saas-screensteps-tutorial/ic778523.png "Account management")</span></span>

9. <span data-ttu-id="74612-168">Kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="74612-168">Click **Single Sign-on**.</span></span>

    <span data-ttu-id="74612-169">![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778524.png "távoli hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="74612-169">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778524.png "Remote authentication")</span></span>

10. <span data-ttu-id="74612-170">Kattintson a **egyszeri bejelentkezési végpont létrehozásához**.</span><span class="sxs-lookup"><span data-stu-id="74612-170">Click **Create Single Sign-on Endpoint**.</span></span>

    <span data-ttu-id="74612-171">![Távoli hitelesítési](./media/active-directory-saas-screensteps-tutorial/ic778525.png "távoli hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="74612-171">![Remote authentication](./media/active-directory-saas-screensteps-tutorial/ic778525.png "Remote authentication")</span></span>

11. <span data-ttu-id="74612-172">Az a **létrehozása egyszeri bejelentkezés végpont** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="74612-172">In the **Create Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="74612-173">![Hozzon létre egy hitelesítési végpontot](./media/active-directory-saas-screensteps-tutorial/ic778526.png "hozzon létre egy hitelesítési végpontot")</span><span class="sxs-lookup"><span data-stu-id="74612-173">![Create an authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778526.png "Create an authentication endpoint")</span></span>
    
    <span data-ttu-id="74612-174">a.</span><span class="sxs-lookup"><span data-stu-id="74612-174">a.</span></span> <span data-ttu-id="74612-175">Az a **cím** szövegmezőhöz címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="74612-175">In the **Title** textbox, type a title.</span></span>
    
    <span data-ttu-id="74612-176">b.</span><span class="sxs-lookup"><span data-stu-id="74612-176">b.</span></span> <span data-ttu-id="74612-177">Az a **mód** listáról válassza ki **SAML**.</span><span class="sxs-lookup"><span data-stu-id="74612-177">From the **Mode** list, select **SAML**.</span></span>
    
    <span data-ttu-id="74612-178">c.</span><span class="sxs-lookup"><span data-stu-id="74612-178">c.</span></span> <span data-ttu-id="74612-179">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-179">Click **Create**.</span></span>

12. <span data-ttu-id="74612-180">**Szerkesztés** az új végpont.</span><span class="sxs-lookup"><span data-stu-id="74612-180">**Edit** the new endpoint.</span></span>

    <span data-ttu-id="74612-181">![Szerkessze a végpont](./media/active-directory-saas-screensteps-tutorial/ic778528.png "végpont szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="74612-181">![Edit endpoint](./media/active-directory-saas-screensteps-tutorial/ic778528.png "Edit endpoint")</span></span>

13. <span data-ttu-id="74612-182">Az a **szerkesztése egyszeri bejelentkezés végpont** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="74612-182">In the **Edit Single Sign-on Endpoint** section, perform the following steps:</span></span>

    <span data-ttu-id="74612-183">![Távoli hitelesítési végpont](./media/active-directory-saas-screensteps-tutorial/ic778527.png "távoli hitelesítési végpontot")</span><span class="sxs-lookup"><span data-stu-id="74612-183">![Remote authentication endpoint](./media/active-directory-saas-screensteps-tutorial/ic778527.png "Remote authentication endpoint")</span></span>

    <span data-ttu-id="74612-184">a.</span><span class="sxs-lookup"><span data-stu-id="74612-184">a.</span></span> <span data-ttu-id="74612-185">Kattintson a **feltöltés új SAML tanúsítványfájl**, majd töltse fel a tanúsítványt, amely az Azure-portálról letöltött.</span><span class="sxs-lookup"><span data-stu-id="74612-185">Click **Upload new SAML Certificate file**, and then upload the certificate, which you have downloaded from Azure portal.</span></span>
    
    <span data-ttu-id="74612-186">b.</span><span class="sxs-lookup"><span data-stu-id="74612-186">b.</span></span> <span data-ttu-id="74612-187">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálról másolta a **távoli bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="74612-187">Paste **SAML Single Sign-On Service URL** value, which you have copied from the Azure portal into the **Remote Login URL** textbox.</span></span>
    
    <span data-ttu-id="74612-188">c.</span><span class="sxs-lookup"><span data-stu-id="74612-188">c.</span></span> <span data-ttu-id="74612-189">Beillesztés **Sign-Out URL-cím** értéket, amely az Azure-portálról másolta a **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="74612-189">Paste **Sign-Out URL** value, which you have copied from the Azure portal into the **Log out URL** textbox.</span></span>
    
    <span data-ttu-id="74612-190">d.</span><span class="sxs-lookup"><span data-stu-id="74612-190">d.</span></span> <span data-ttu-id="74612-191">Válassza ki a **csoport** kiépítésüket amikor a felhasználók hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="74612-191">Select a **Group** to assign users to when they are provisioned.</span></span>
    
    <span data-ttu-id="74612-192">e.</span><span class="sxs-lookup"><span data-stu-id="74612-192">e.</span></span> <span data-ttu-id="74612-193">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="74612-193">Click **Update**.</span></span>

    <span data-ttu-id="74612-194">f.</span><span class="sxs-lookup"><span data-stu-id="74612-194">f.</span></span> <span data-ttu-id="74612-195">Másolás a **SAML-alapú ügyfél URL-címe** a vágólapra és illessze be a a **bejelentkezési URL-cím** textbox **ScreenSteps tartomány és az URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="74612-195">Copy the **SAML Consumer URL** to the clipboard and paste in to the **Sign-on URL** textbox in **ScreenSteps Domain and URLs** section.</span></span>
    
    <span data-ttu-id="74612-196">g.</span><span class="sxs-lookup"><span data-stu-id="74612-196">g.</span></span> <span data-ttu-id="74612-197">Lépjen vissza a **egyszeri bejelentkezési végpont szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="74612-197">Return to the **Edit Single Sign-on Endpoint**.</span></span>
    
    <span data-ttu-id="74612-198">h.</span><span class="sxs-lookup"><span data-stu-id="74612-198">h.</span></span> <span data-ttu-id="74612-199">Kattintson a **fiók alapértelmezett** gombra kattintva ezt a végpontot használata az összes olyan felhasználó, aki ScreenSteps be tudjon jelentkezni.</span><span class="sxs-lookup"><span data-stu-id="74612-199">Click the **Make default for account** button to use this endpoint for all users who log into ScreenSteps.</span></span> <span data-ttu-id="74612-200">Másik lehetőségként kattinthat a **helyen** gombra kattintva használni ezt a végpontot az adott webhelyekhez **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="74612-200">Alternatively you can click the **Add to Site** button to use this endpoint for specific sites in **ScreenSteps**.</span></span>

> [!TIP]
> <span data-ttu-id="74612-201">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="74612-201">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="74612-202">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="74612-202">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="74612-203">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="74612-203">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="74612-204">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="74612-204">Create an Azure AD test user</span></span>

<span data-ttu-id="74612-205">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="74612-205">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="74612-207">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="74612-207">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="74612-208">Az Azure portálon a bal oldali ablaktáblán kattintson a **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-208">In the Azure portal, in the left pane, click the **Azure Active Directory** button.</span></span>

    ![Az Azure Active Directory gomb](./media/active-directory-saas-screensteps-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="74612-210">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="74612-210">To display the list of users, go to **Users and groups**, and then click **All users**.</span></span>

    ![A "felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-screensteps-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="74612-212">Megnyitásához a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** tetején a **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="74612-212">To open the **User** dialog box, click **Add** at the top of the **All Users** dialog box.</span></span>

    ![A Hozzáadás gombra.](./media/active-directory-saas-screensteps-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="74612-214">Az a **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="74612-214">In the **User** dialog box, perform the following steps:</span></span>

    ![A felhasználó párbeszédpanel](./media/active-directory-saas-screensteps-tutorial/create_aaduser_04.png)

    <span data-ttu-id="74612-216">a.</span><span class="sxs-lookup"><span data-stu-id="74612-216">a.</span></span> <span data-ttu-id="74612-217">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="74612-217">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="74612-218">b.</span><span class="sxs-lookup"><span data-stu-id="74612-218">b.</span></span> <span data-ttu-id="74612-219">Az a **felhasználónév** mezőbe írja be a felhasználó e-mail címe az Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="74612-219">In the **User name** box, type the email address of user Britta Simon.</span></span>

    <span data-ttu-id="74612-220">c.</span><span class="sxs-lookup"><span data-stu-id="74612-220">c.</span></span> <span data-ttu-id="74612-221">Válassza ki a **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a megjelenített érték a **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="74612-221">Select the **Show Password** check box, and then write down the value that's displayed in the **Password** box.</span></span>

    <span data-ttu-id="74612-222">d.</span><span class="sxs-lookup"><span data-stu-id="74612-222">d.</span></span> <span data-ttu-id="74612-223">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-223">Click **Create**.</span></span>
 
### <a name="create-a-screensteps-test-user"></a><span data-ttu-id="74612-224">ScreenSteps tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="74612-224">Create a ScreenSteps test user</span></span>

<span data-ttu-id="74612-225">Ebben a szakaszban egy ScreenSteps Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="74612-225">In this section, you create a user called Britta Simon in ScreenSteps.</span></span> <span data-ttu-id="74612-226">Együttműködve [ScreenSteps ügyfél-támogatási csoport](https://www.screensteps.com/contact) a felhasználók hozzáadása a ScreenSteps platform.</span><span class="sxs-lookup"><span data-stu-id="74612-226">Work with [ScreenSteps Client support team](https://www.screensteps.com/contact) to add the users in the ScreenSteps platform.</span></span> <span data-ttu-id="74612-227">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="74612-227">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="74612-228">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="74612-228">Assign the Azure AD test user</span></span>

<span data-ttu-id="74612-229">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés ScreenSteps Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="74612-229">In this section, you enable Britta Simon to use Azure single sign-on by granting access to ScreenSteps.</span></span>

![A felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="74612-231">**Britta Simon hozzárendelése ScreenSteps, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="74612-231">**To assign Britta Simon to ScreenSteps, perform the following steps:**</span></span>

1. <span data-ttu-id="74612-232">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="74612-232">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="74612-234">Az alkalmazások listában válassza ki a **ScreenSteps**.</span><span class="sxs-lookup"><span data-stu-id="74612-234">In the applications list, select **ScreenSteps**.</span></span>

    ![Az alkalmazások listáját a ScreenSteps hivatkozás](./media/active-directory-saas-screensteps-tutorial/tutorial_screensteps_app.png)  

3. <span data-ttu-id="74612-236">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="74612-236">In the menu on the left, click **Users and groups**.</span></span>

    ![A "Felhasználók és csoportok" hivatkozásra][202]

4. <span data-ttu-id="74612-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="74612-238">Click **Add** button.</span></span> <span data-ttu-id="74612-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74612-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![A hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="74612-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="74612-241">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="74612-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74612-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="74612-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="74612-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="74612-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="74612-244">Test single sign-on</span></span>

<span data-ttu-id="74612-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="74612-245">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="74612-246">Ha a hozzáférési panelen ScreenSteps csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ScreenSteps alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="74612-246">When you click the ScreenSteps tile in the Access Panel, you should get automatically signed-on to your ScreenSteps application.</span></span>
<span data-ttu-id="74612-247">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="74612-247">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="74612-248">További források</span><span class="sxs-lookup"><span data-stu-id="74612-248">Additional resources</span></span>

* [<span data-ttu-id="74612-249">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="74612-249">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="74612-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="74612-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-screensteps-tutorial/tutorial_general_203.png

