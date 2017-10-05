---
title: "Oktatóanyag: Azure Active Directoryval integrált Slackhez |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Slackhez között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2017
ms.author: jeedes
ms.openlocfilehash: 5aca630b2077d3f7d4ce9273ee668ed6a5f9843d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-slack"></a><span data-ttu-id="8538a-103">Oktatóanyag: Azure Active Directoryval integrált Slackhez</span><span class="sxs-lookup"><span data-stu-id="8538a-103">Tutorial: Azure Active Directory integration with Slack</span></span>

<span data-ttu-id="8538a-104">Ebben az oktatóanyagban elsajátíthatja Slackhez integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8538a-104">In this tutorial, you learn how to integrate Slack with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8538a-105">Slackhez integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="8538a-105">Integrating Slack with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="8538a-106">Megadhatja a Slackhez hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8538a-106">You can control in Azure AD who has access to Slack</span></span>
- <span data-ttu-id="8538a-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett slackhez (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8538a-107">You can enable your users to automatically get signed-on to Slack (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8538a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8538a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="8538a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8538a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8538a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8538a-110">Prerequisites</span></span>

<span data-ttu-id="8538a-111">Az Azure AD-integráció konfigurálása a Slackhez, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="8538a-111">To configure Azure AD integration with Slack, you need the following items:</span></span>

- <span data-ttu-id="8538a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8538a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8538a-113">A Slack egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8538a-113">A Slack single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8538a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8538a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8538a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="8538a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8538a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8538a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8538a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8538a-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8538a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8538a-118">Scenario description</span></span>
<span data-ttu-id="8538a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8538a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8538a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8538a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8538a-121">A gyűjteményből Slackhez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8538a-121">Adding Slack from the gallery</span></span>
2. <span data-ttu-id="8538a-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8538a-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-slack-from-the-gallery"></a><span data-ttu-id="8538a-123">A gyűjteményből Slackhez hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8538a-123">Adding Slack from the gallery</span></span>
<span data-ttu-id="8538a-124">A Slackhez az Azure AD-integráció konfigurálása, szüksége Slackhez hozzáadása a kezelt SaaS-alkalmazások listáját a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="8538a-124">To configure the integration of Slack into Azure AD, you need to add Slack from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="8538a-125">**Adja hozzá a Slackhez a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8538a-125">**To add Slack from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="8538a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8538a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8538a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8538a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="8538a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8538a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8538a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="8538a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8538a-133">Írja be a keresőmezőbe, **Slackhez**.</span><span class="sxs-lookup"><span data-stu-id="8538a-133">In the search box, type **Slack**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/tutorial_slack_search.png)

5. <span data-ttu-id="8538a-135">Az eredmények panelen válassza ki a **Slackhez**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8538a-135">In the results panel, select **Slack**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/tutorial_slack_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8538a-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8538a-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8538a-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Slackhez.</span><span class="sxs-lookup"><span data-stu-id="8538a-138">In this section, you configure and test Azure AD single sign-on with Slack based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8538a-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a Slackhez tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="8538a-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Slack is to a user in Azure AD.</span></span> <span data-ttu-id="8538a-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Slackhez közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8538a-140">In other words, a link relationship between an Azure AD user and the related user in Slack needs to be established.</span></span>

<span data-ttu-id="8538a-141">A Slackhez, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="8538a-141">In Slack, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="8538a-142">Az Azure AD egyszeri bejelentkezést a Slackhez tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="8538a-142">To configure and test Azure AD single sign-on with Slack, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="8538a-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="8538a-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="8538a-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8538a-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8538a-145">**[Slack tesztfelhasználó létrehozása](#creating-a-slack-test-user)**  - való Britta Simon egy megfelelője a Slackhez, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="8538a-145">**[Creating a Slack test user](#creating-a-slack-test-user)** - to have a counterpart of Britta Simon in Slack that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="8538a-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8538a-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8538a-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="8538a-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8538a-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8538a-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8538a-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az közzététele a Slack-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8538a-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Slack application.</span></span>

<span data-ttu-id="8538a-150">**Konfigurálja az Azure AD egyszeri bejelentkezést a Slackhez, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8538a-150">**To configure Azure AD single sign-on with Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="8538a-151">Az Azure portálon a a **Slackhez** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8538a-151">In the Azure portal, on the **Slack** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8538a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8538a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_samlbase.png)

3. <span data-ttu-id="8538a-155">Az a **Slackhez tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="8538a-155">On the **Slack Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_url.png)

    <span data-ttu-id="8538a-157">a.</span><span class="sxs-lookup"><span data-stu-id="8538a-157">a.</span></span> <span data-ttu-id="8538a-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.slack.com`</span><span class="sxs-lookup"><span data-stu-id="8538a-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.slack.com`</span></span>

    <span data-ttu-id="8538a-159">b.</span><span class="sxs-lookup"><span data-stu-id="8538a-159">b.</span></span> <span data-ttu-id="8538a-160">Az a **azonosító** szövegmező, írja be az URL-cím:`https://slack.com`</span><span class="sxs-lookup"><span data-stu-id="8538a-160">In the **Identifier** textbox, type the URL: `https://slack.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8538a-161">Az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="8538a-161">The value is not real.</span></span> <span data-ttu-id="8538a-162">Frissítse az értéket a tényleges bejelentkezési az URL-címmel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8538a-162">You have to update the value with the actual Sign On URL.</span></span> <span data-ttu-id="8538a-163">Ügyfél [Slack támogatási csoport](https://slack.com/help/contact) a érték</span><span class="sxs-lookup"><span data-stu-id="8538a-163">Contact [Slack support team](https://slack.com/help/contact) to get the value</span></span>
     
4. <span data-ttu-id="8538a-164">Alkalmazás közzététele a Slack vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="8538a-164">Slack application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="8538a-165">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="8538a-165">Configure the following claims for this application.</span></span> <span data-ttu-id="8538a-166">Ezek az attribútumok értékének kezelheti a "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="8538a-166">You can manage the values of these attributes from the "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="8538a-167">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="8538a-167">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute.png)

5. <span data-ttu-id="8538a-169">Az a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédablakban válassza **user.mail** , **felhasználói azonosító** és az alábbi táblázatban szereplő minden egyes sorhoz kapcsolódóan végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8538a-169">In the **User Attributes** section on the **Single sign-on** dialog, select **user.mail**  as **User Identifier** and for each row shown in the table below, perform the following steps:</span></span>
    
    | <span data-ttu-id="8538a-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="8538a-170">Attribute Name</span></span> | <span data-ttu-id="8538a-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="8538a-171">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="8538a-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="8538a-172">first_name</span></span> | <span data-ttu-id="8538a-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="8538a-173">user.givenname</span></span> |
    | <span data-ttu-id="8538a-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="8538a-174">last_name</span></span> | <span data-ttu-id="8538a-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="8538a-175">user.surname</span></span> |
    | <span data-ttu-id="8538a-176">User.Email</span><span class="sxs-lookup"><span data-stu-id="8538a-176">User.Email</span></span> | <span data-ttu-id="8538a-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="8538a-177">user.mail</span></span> |  
    | <span data-ttu-id="8538a-178">User.Username</span><span class="sxs-lookup"><span data-stu-id="8538a-178">User.Username</span></span> | <span data-ttu-id="8538a-179">User.userPrincipalName</span><span class="sxs-lookup"><span data-stu-id="8538a-179">user.userprincipalname</span></span> |

    <span data-ttu-id="8538a-180">a.</span><span class="sxs-lookup"><span data-stu-id="8538a-180">a.</span></span> <span data-ttu-id="8538a-181">Kattintson a **attribútum** megnyitásához **attribútum szerkesztése** párbeszédpanel mezőbe, majd hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8538a-181">Click on **Attribute** to open **Edit Attribute** dialog box and perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_attribute1.png)

    <span data-ttu-id="8538a-183">a.</span><span class="sxs-lookup"><span data-stu-id="8538a-183">a.</span></span> <span data-ttu-id="8538a-184">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="8538a-184">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="8538a-185">b.</span><span class="sxs-lookup"><span data-stu-id="8538a-185">b.</span></span> <span data-ttu-id="8538a-186">Az a **érték** listára, válassza ki a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="8538a-186">From the **Value** list, select the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="8538a-187">c.</span><span class="sxs-lookup"><span data-stu-id="8538a-187">c.</span></span> <span data-ttu-id="8538a-188">Kattintson az **OK** gombra</span><span class="sxs-lookup"><span data-stu-id="8538a-188">Click **OK**</span></span>

6. <span data-ttu-id="8538a-189">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8538a-189">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_certificate.png)

7. <span data-ttu-id="8538a-191">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8538a-191">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="8538a-193">A a **Slackhez konfigurációs** kattintson **konfigurálása Slackhez** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8538a-193">On the **Slack Configuration** section, click **Configure Slack** to open **Configure sign-on** window.</span></span> <span data-ttu-id="8538a-194">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8538a-194">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_configure.png) 

9.  <span data-ttu-id="8538a-196">Egy másik webes böngészőablakban jelentkezzen be a Slack vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8538a-196">In a different web browser window, log in to your Slack company site as an administrator.</span></span>

10.  <span data-ttu-id="8538a-197">Navigáljon a **Microsoft Azure AD** majd navigáljon **Team beállításainak**.</span><span class="sxs-lookup"><span data-stu-id="8538a-197">Navigate to **Microsoft Azure AD** then go to **Team Settings**.</span></span>

     ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_001.png)

11.  <span data-ttu-id="8538a-199">Az a **Team beállításainak** területen kattintson a **hitelesítési** fülre, majd **beállításainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="8538a-199">In the **Team Settings** section, click the **Authentication** tab, and then click **Change Settings**.</span></span>

     ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_002.png)

12. <span data-ttu-id="8538a-201">Az a **SAML-alapú hitelesítési beállítások** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="8538a-201">On the **SAML Authentication Settings** dialog, perform the following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_003.png)

    <span data-ttu-id="8538a-203">a.</span><span class="sxs-lookup"><span data-stu-id="8538a-203">a.</span></span>  <span data-ttu-id="8538a-204">Az a **SAML 2.0 végpontot (HTTP)** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8538a-204">In the **SAML 2.0 Endpoint (HTTP)** textbox, paste the value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8538a-205">b.</span><span class="sxs-lookup"><span data-stu-id="8538a-205">b.</span></span>  <span data-ttu-id="8538a-206">A a **Identity Provider kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8538a-206">In the **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8538a-207">c.</span><span class="sxs-lookup"><span data-stu-id="8538a-207">c.</span></span>  <span data-ttu-id="8538a-208">Nyissa meg a letöltött fájlt a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8538a-208">Open your downloaded certificate file in notepad, copy the content of it into your clipboard, and then paste it to the **Public Certificate** textbox.</span></span>

    <span data-ttu-id="8538a-209">d.</span><span class="sxs-lookup"><span data-stu-id="8538a-209">d.</span></span> <span data-ttu-id="8538a-210">Állítsa be a fenti három megfelelő beállításokat a Slack-csoport.</span><span class="sxs-lookup"><span data-stu-id="8538a-210">Configure the above three settings as appropriate for your Slack team.</span></span> <span data-ttu-id="8538a-211">A beállításokkal kapcsolatos további információkért kérjük található a **Slackhez tartozó SSO konfigurációs útmutató** itt.</span><span class="sxs-lookup"><span data-stu-id="8538a-211">For more information about the settings, please find the **Slack's SSO configuration guide** here.</span></span> `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    <span data-ttu-id="8538a-212">e.</span><span class="sxs-lookup"><span data-stu-id="8538a-212">e.</span></span>  <span data-ttu-id="8538a-213">Kattintson a **konfigurációjának mentéséhez**.</span><span class="sxs-lookup"><span data-stu-id="8538a-213">Click **Save Configuration**.</span></span>
     
    <!-- Deselect **Allow users to change their email address**.

    e.  Select **Allow users to choose their own username**.

    f.  As **Authentication for your team must be used by**, select **It’s optional**. -->

> [!TIP]
> <span data-ttu-id="8538a-214">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="8538a-214">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="8538a-215">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="8538a-215">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="8538a-216">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8538a-216">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8538a-217">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8538a-217">Creating an Azure AD test user</span></span>
<span data-ttu-id="8538a-218">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="8538a-218">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8538a-220">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8538a-220">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="8538a-221">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8538a-221">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8538a-223">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8538a-223">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8538a-225">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="8538a-225">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8538a-227">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="8538a-227">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-slack-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8538a-229">a.</span><span class="sxs-lookup"><span data-stu-id="8538a-229">a.</span></span> <span data-ttu-id="8538a-230">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8538a-230">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8538a-231">b.</span><span class="sxs-lookup"><span data-stu-id="8538a-231">b.</span></span> <span data-ttu-id="8538a-232">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8538a-232">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8538a-233">c.</span><span class="sxs-lookup"><span data-stu-id="8538a-233">c.</span></span> <span data-ttu-id="8538a-234">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8538a-234">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="8538a-235">d.</span><span class="sxs-lookup"><span data-stu-id="8538a-235">d.</span></span> <span data-ttu-id="8538a-236">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8538a-236">Click **Create**.</span></span>
 
### <a name="creating-a-slack-test-user"></a><span data-ttu-id="8538a-237">Slack tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8538a-237">Creating a Slack test user</span></span>

<span data-ttu-id="8538a-238">Ez a szakasz célja a Slackhez Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8538a-238">The objective of this section is to create a user called Britta Simon in Slack.</span></span> <span data-ttu-id="8538a-239">Slackhez támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="8538a-239">Slack supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="8538a-240">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="8538a-240">There is no action item for you in this section.</span></span> <span data-ttu-id="8538a-241">Új felhasználó jön létre az Ha még nem létezik a Slackhez elérésére tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="8538a-241">A new user is created during an attempt to access Slack if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="8538a-242">Ha manuálisan hozzon létre egy felhasználó van szüksége, lépjen kapcsolatba kell [Slack támogatási csoport](https://slack.com/help/contact).</span><span class="sxs-lookup"><span data-stu-id="8538a-242">If you need to create a user manually, you need to Contact [Slack support team](https://slack.com/help/contact).</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="8538a-243">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8538a-243">Assigning the Azure AD test user</span></span>

<span data-ttu-id="8538a-244">Ebben a szakaszban Britta Simon hozzáférés biztosítása a slackhez által használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="8538a-244">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Slack.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8538a-246">**Britta Simon hozzárendelése a Slackhez, hajtsa végre a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8538a-246">**To assign Britta Simon to Slack, perform the following steps:**</span></span>

1. <span data-ttu-id="8538a-247">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8538a-247">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8538a-249">Az alkalmazások listában válassza ki a **Slackhez**.</span><span class="sxs-lookup"><span data-stu-id="8538a-249">In the applications list, select **Slack**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-slack-tutorial/tutorial_slack_app.png) 

3. <span data-ttu-id="8538a-251">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8538a-251">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8538a-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8538a-253">Click **Add** button.</span></span> <span data-ttu-id="8538a-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8538a-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8538a-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8538a-256">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="8538a-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8538a-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8538a-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8538a-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8538a-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8538a-259">Testing single sign-on</span></span>

<span data-ttu-id="8538a-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8538a-260">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="8538a-261">A Slack csempe elemre a hozzáférési panelen, meg kell beolvasása automatikusan bejelentkezett a Slack alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8538a-261">When you click the Slack tile in the Access Panel, you should get automatically signed-on to your Slack application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8538a-262">További források</span><span class="sxs-lookup"><span data-stu-id="8538a-262">Additional resources</span></span>

* [<span data-ttu-id="8538a-263">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="8538a-263">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8538a-264">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8538a-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-slack-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-slack-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-slack-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-slack-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-slack-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-slack-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-slack-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-slack-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-slack-tutorial/tutorial_general_203.png

