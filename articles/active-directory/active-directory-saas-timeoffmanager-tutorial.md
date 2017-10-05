---
title: "Oktatóanyag: Azure Active Directoryval integrált TimeOffManager |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és TimeOffManager között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 3685912f-d5aa-4730-ab58-35a088fc1cc3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 3f944ffbf704694b293b4b1e5bdb4f2c93ae35a1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="tutorial-azure-active-directory-integration-with-timeoffmanager"></a><span data-ttu-id="9082b-103">Oktatóanyag: Azure Active Directoryval integrált TimeOffManager</span><span class="sxs-lookup"><span data-stu-id="9082b-103">Tutorial: Azure Active Directory integration with TimeOffManager</span></span>

<span data-ttu-id="9082b-104">Ebben az oktatóanyagban elsajátíthatja TimeOffManager integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="9082b-104">In this tutorial, you learn how to integrate TimeOffManager with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="9082b-105">TimeOffManager integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9082b-105">Integrating TimeOffManager with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="9082b-106">Megadhatja a TimeOffManager hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9082b-106">You can control in Azure AD who has access to TimeOffManager</span></span>
- <span data-ttu-id="9082b-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett TimeOffManager (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="9082b-107">You can enable your users to automatically get signed-on to TimeOffManager (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="9082b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="9082b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="9082b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9082b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9082b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9082b-110">Prerequisites</span></span>

<span data-ttu-id="9082b-111">Konfigurálása az Azure AD-integrációs TimeOffManager, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9082b-111">To configure Azure AD integration with TimeOffManager, you need the following items:</span></span>

- <span data-ttu-id="9082b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9082b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="9082b-113">Egy TimeOffManager egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="9082b-113">A TimeOffManager single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9082b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9082b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="9082b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9082b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="9082b-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9082b-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="9082b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9082b-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="9082b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9082b-118">Scenario description</span></span>
<span data-ttu-id="9082b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="9082b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="9082b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9082b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9082b-121">Adja hozzá a TimeOffManager a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9082b-121">Add TimeOffManager from the gallery</span></span>
2. <span data-ttu-id="9082b-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9082b-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-timeoffmanager-from-the-gallery"></a><span data-ttu-id="9082b-123">Adja hozzá a TimeOffManager a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="9082b-123">Add TimeOffManager from the gallery</span></span>
<span data-ttu-id="9082b-124">Az Azure AD integrálása a TimeOffManager konfigurálásához kell hozzáadnia TimeOffManager a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9082b-124">To configure the integration of TimeOffManager into Azure AD, you need to add TimeOffManager from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9082b-125">**A gyűjteményből TimeOffManager hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9082b-125">**To add TimeOffManager from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9082b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9082b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="9082b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="9082b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="9082b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9082b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="9082b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="9082b-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="9082b-133">Írja be a keresőmezőbe, **TimeOffManager**, jelölje be **TimeOffManager** eredmény panelen, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="9082b-133">In the search box, type **TimeOffManager**, select **TimeOffManager** from result panel and then click **Add** button to add the application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="9082b-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9082b-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="9082b-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="9082b-136">In this section, you configure and test Azure AD single sign-on with TimeOffManager based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9082b-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó TimeOffManager a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="9082b-137">For single sign-on to work, Azure AD needs to know what the counterpart user in TimeOffManager is to a user in Azure AD.</span></span> <span data-ttu-id="9082b-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a TimeOffManager közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9082b-138">In other words, a link relationship between an Azure AD user and the related user in TimeOffManager needs to be established.</span></span>

<span data-ttu-id="9082b-139">TimeOffManager, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="9082b-139">In TimeOffManager, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="9082b-140">Az Azure AD egyszeri bejelentkezést a TimeOffManager tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9082b-140">To configure and test Azure AD single sign-on with TimeOffManager, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9082b-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9082b-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9082b-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9082b-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9082b-143">**[TimeOffManager tesztfelhasználó létrehozása](#create-a-timeoffmanager-test-user)**  - való Britta Simon valami TimeOffManager, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="9082b-143">**[Create a TimeOffManager test user](#create-a-timeoffmanager-test-user)** - to have a counterpart of Britta Simon in TimeOffManager that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="9082b-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9082b-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9082b-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9082b-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="9082b-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9082b-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="9082b-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az TimeOffManager alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9082b-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your TimeOffManager application.</span></span>

<span data-ttu-id="9082b-148">**Konfigurálása az Azure AD az egyszeri bejelentkezés TimeOffManager, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9082b-148">**To configure Azure AD single sign-on with TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="9082b-149">Az Azure portálon a a **TimeOffManager** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="9082b-149">In the Azure portal, on the **TimeOffManager** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="9082b-151">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9082b-151">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_samlbase.png)

3. <span data-ttu-id="9082b-153">Az a **TimeOffManager tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9082b-153">On the **TimeOffManager Domain and URLs** section, perform the following:</span></span>

     ![TimeOffManager tartomány és az URL-címek szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_url.png)

    <span data-ttu-id="9082b-155">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span><span class="sxs-lookup"><span data-stu-id="9082b-155">In the **Reply URL** textbox, type a URL using the following pattern: `https://www.timeoffmanager.com/cpanel/sso/consume.aspx?company_id=<companyid>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="9082b-156">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="9082b-156">This value is not real.</span></span> <span data-ttu-id="9082b-157">Frissítse ezt az értéket a tényleges válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="9082b-157">Update this value with the actual Reply URL.</span></span> <span data-ttu-id="9082b-158">Ezt az értéket kaphat **egyszeri bejelentkezési beállítások lapon** amely esetén, tekintse meg az oktatóanyag használatával, vagy forduljon a [TimeOffManager támogatási csoport](http://www.timeoffmanager.com/contact-us.aspx).</span><span class="sxs-lookup"><span data-stu-id="9082b-158">You can get this value from **Single Sign on settings page** which is explained later in the tutorial or Contact [TimeOffManager support team](http://www.timeoffmanager.com/contact-us.aspx).</span></span>
 
4. <span data-ttu-id="9082b-159">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="9082b-159">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_certificate.png) 

5. <span data-ttu-id="9082b-161">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez TimeOffManger fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="9082b-161">The objective of this section is to outline how to enable users to authenticate to TimeOffManger with their account in Azure AD using federation based on the SAML protocol.</span></span>
    
    <span data-ttu-id="9082b-162">A TimeOffManger alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="9082b-162">Your TimeOffManger application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="9082b-163">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="9082b-163">The following screenshot shows an example for this.</span></span>

    <span data-ttu-id="9082b-164">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="9082b-164">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_attrb.png "saml token attributes")</span></span>
    
    | <span data-ttu-id="9082b-165">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="9082b-165">Attribute Name</span></span> | <span data-ttu-id="9082b-166">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="9082b-166">Attribute Value</span></span> |
    | --- | --- |
    | <span data-ttu-id="9082b-167">Utónév</span><span class="sxs-lookup"><span data-stu-id="9082b-167">Firstname</span></span> |<span data-ttu-id="9082b-168">User.givenName</span><span class="sxs-lookup"><span data-stu-id="9082b-168">User.givenname</span></span> |
    | <span data-ttu-id="9082b-169">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="9082b-169">Lastname</span></span> |<span data-ttu-id="9082b-170">User.surname</span><span class="sxs-lookup"><span data-stu-id="9082b-170">User.surname</span></span> |
    | <span data-ttu-id="9082b-171">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="9082b-171">Email</span></span> |<span data-ttu-id="9082b-172">User.mail</span><span class="sxs-lookup"><span data-stu-id="9082b-172">User.mail</span></span> |
    
    <span data-ttu-id="9082b-173">a.</span><span class="sxs-lookup"><span data-stu-id="9082b-173">a.</span></span>  <span data-ttu-id="9082b-174">Kattintson a fenti adatokat minden egyes sorhoz kapcsolódóan **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="9082b-174">For each data row in the table above, click **add user attribute**.</span></span>
    
    <span data-ttu-id="9082b-175">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="9082b-175">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb.png "saml token attributes")</span></span>
    
    <span data-ttu-id="9082b-176">![SAML-jogkivonat attribútumok](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml-jogkivonat attribútumok")</span><span class="sxs-lookup"><span data-stu-id="9082b-176">![saml token attributes](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_addattrb1.png "saml token attributes")</span></span>
    
    <span data-ttu-id="9082b-177">b.</span><span class="sxs-lookup"><span data-stu-id="9082b-177">b.</span></span>  <span data-ttu-id="9082b-178">Az a **attribútumnév** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="9082b-178">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="9082b-179">c.</span><span class="sxs-lookup"><span data-stu-id="9082b-179">c.</span></span>  <span data-ttu-id="9082b-180">Az a **attribútumérték** szövegmező, válassza ki az adott sorhoz feltüntetett attribútumot értéket.</span><span class="sxs-lookup"><span data-stu-id="9082b-180">In the **Attribute Value** textbox, select the attribute  value shown for that row.</span></span>
    
    <span data-ttu-id="9082b-181">d.</span><span class="sxs-lookup"><span data-stu-id="9082b-181">d.</span></span>  <span data-ttu-id="9082b-182">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="9082b-182">Click **Ok**.</span></span>
    
6. <span data-ttu-id="9082b-183">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="9082b-183">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="9082b-185">A a **TimeOffManager konfigurációs** kattintson **konfigurálása TimeOffManager** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="9082b-185">On the **TimeOffManager Configuration** section, click **Configure TimeOffManager** to open **Configure sign-on** window.</span></span> <span data-ttu-id="9082b-186">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="9082b-186">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![TimeOffManager konfigurációs szakasz](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_configure.png) 

8. <span data-ttu-id="9082b-188">Egy másik webes böngészőablakban jelentkezzen be a TimeOffManager vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="9082b-188">In a different web browser window, log into your TimeOffManager company site as an administrator.</span></span>

9. <span data-ttu-id="9082b-189">Nyissa meg a **fiók \> beállítások fiók \> az egyszeri bejelentkezés beállítások**.</span><span class="sxs-lookup"><span data-stu-id="9082b-189">Go to **Account \> Account Options \> Single Sign-On Settings**.</span></span>
   
   <span data-ttu-id="9082b-190">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="9082b-190">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795917.png "Single Sign-On Settings")</span></span>
7. <span data-ttu-id="9082b-191">Az a **egyszeri bejelentkezési beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="9082b-191">In the **Single Sign-On Settings** section, perform the following steps:</span></span>
   
   <span data-ttu-id="9082b-192">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="9082b-192">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795918.png "Single Sign-On Settings")</span></span>
   
   <span data-ttu-id="9082b-193">a.</span><span class="sxs-lookup"><span data-stu-id="9082b-193">a.</span></span> <span data-ttu-id="9082b-194">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be a teljes tanúsítványt **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9082b-194">Open your base-64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste the entire Certificate into **X.509 Certificate** textbox.</span></span>
   
   <span data-ttu-id="9082b-195">b.</span><span class="sxs-lookup"><span data-stu-id="9082b-195">b.</span></span> <span data-ttu-id="9082b-196">A **Idp kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9082b-196">In **Idp Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="9082b-197">c.</span><span class="sxs-lookup"><span data-stu-id="9082b-197">c.</span></span> <span data-ttu-id="9082b-198">A **IdP végponti URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9082b-198">In **IdP Endpoint URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="9082b-199">d.</span><span class="sxs-lookup"><span data-stu-id="9082b-199">d.</span></span> <span data-ttu-id="9082b-200">Mint **kényszerítése SAML**, jelölje be **nem**.</span><span class="sxs-lookup"><span data-stu-id="9082b-200">As **Enforce SAML**, select **No**.</span></span>
   
   <span data-ttu-id="9082b-201">e.</span><span class="sxs-lookup"><span data-stu-id="9082b-201">e.</span></span> <span data-ttu-id="9082b-202">Mint **automatikus létrehozása felhasználók**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="9082b-202">As **Auto-Create Users**, select **Yes**.</span></span>
   
   <span data-ttu-id="9082b-203">f.</span><span class="sxs-lookup"><span data-stu-id="9082b-203">f.</span></span> <span data-ttu-id="9082b-204">A **kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="9082b-204">In **Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
   
   <span data-ttu-id="9082b-205">g.</span><span class="sxs-lookup"><span data-stu-id="9082b-205">g.</span></span> <span data-ttu-id="9082b-206">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="9082b-206">click **Save Changes**.</span></span>

11. <span data-ttu-id="9082b-207">A **egyszeri bejelentkezési beállítások** lapján értékének másolása **helyességi feltétel ügyfél szolgáltatás URL-címe** és illessze be a **válasz URL-CÍMEN** szövegmező alatt **TimeOffManager Tartomány- és URL-címek** szakaszban az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="9082b-207">In **Single Sign on settings** page, copy the value of **Assertion Consumer Service URL** and paste it in the **Reply URL** text box under **TimeOffManager Domain and URLs** section in Azure portal.</span></span> 

      <span data-ttu-id="9082b-208">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="9082b-208">![Single Sign-On Settings](./media/active-directory-saas-timeoffmanager-tutorial/ic795915.png "Single Sign-On Settings")</span></span>

> [!TIP]
> <span data-ttu-id="9082b-209">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="9082b-209">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="9082b-210">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="9082b-210">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="9082b-211">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="9082b-211">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="9082b-212">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9082b-212">Create an Azure AD test user</span></span>
<span data-ttu-id="9082b-213">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9082b-213">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="9082b-215">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9082b-215">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9082b-216">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="9082b-216">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="9082b-218">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9082b-218">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok--> minden felhasználó](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="9082b-220">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="9082b-220">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Hozzáadás gomb](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="9082b-222">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9082b-222">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-timeoffmanager-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="9082b-224">a.</span><span class="sxs-lookup"><span data-stu-id="9082b-224">a.</span></span> <span data-ttu-id="9082b-225">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9082b-225">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="9082b-226">b.</span><span class="sxs-lookup"><span data-stu-id="9082b-226">b.</span></span> <span data-ttu-id="9082b-227">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="9082b-227">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="9082b-228">c.</span><span class="sxs-lookup"><span data-stu-id="9082b-228">c.</span></span> <span data-ttu-id="9082b-229">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9082b-229">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="9082b-230">d.</span><span class="sxs-lookup"><span data-stu-id="9082b-230">d.</span></span> <span data-ttu-id="9082b-231">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9082b-231">Click **Create**.</span></span>
 
### <a name="create-a-timeoffmanager-test-user"></a><span data-ttu-id="9082b-232">TimeOffManager tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9082b-232">Create a TimeOffManager test user</span></span>

<span data-ttu-id="9082b-233">Ahhoz, hogy az Azure AD-felhasználók TimeOffManager bejelentkezni, akkor ki kell építenie TimeOffManager.</span><span class="sxs-lookup"><span data-stu-id="9082b-233">In order to enable Azure AD users to log into TimeOffManager, they must be provisioned to TimeOffManager.</span></span>  

<span data-ttu-id="9082b-234">TimeOffManager csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="9082b-234">TimeOffManager supports just in time user provisioning.</span></span> <span data-ttu-id="9082b-235">Nincs művelet elem meg.</span><span class="sxs-lookup"><span data-stu-id="9082b-235">There is no action item for you.</span></span>  

<span data-ttu-id="9082b-236">A felhasználók automatikusan hozzáadja az egyszeri bejelentkezés használatával az első bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="9082b-236">The users are added automatically during the first login using single sign on.</span></span>

>[!NOTE]
><span data-ttu-id="9082b-237">Bármely más TimeOffManager felhasználói fiók létrehozása eszközök vagy TimeOffManager kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="9082b-237">You can use any other TimeOffManager user account creation tools or APIs provided by TimeOffManager to provision Azure AD user accounts.</span></span>
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="9082b-238">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="9082b-238">Assign the Azure AD test user</span></span>

<span data-ttu-id="9082b-239">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés TimeOffManager Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="9082b-239">In this section, you enable Britta Simon to use Azure single sign-on by granting access to TimeOffManager.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="9082b-241">**Britta Simon hozzárendelése TimeOffManager, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9082b-241">**To assign Britta Simon to TimeOffManager, perform the following steps:**</span></span>

1. <span data-ttu-id="9082b-242">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="9082b-242">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="9082b-244">Az alkalmazások listában válassza ki a **TimeOffManager**.</span><span class="sxs-lookup"><span data-stu-id="9082b-244">In the applications list, select **TimeOffManager**.</span></span>

    ![TimeOffManager alkalmazáslistában](./media/active-directory-saas-timeoffmanager-tutorial/tutorial_timeoffmanager_app.png) 

3. <span data-ttu-id="9082b-246">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9082b-246">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="9082b-248">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="9082b-248">Click **Add** button.</span></span> <span data-ttu-id="9082b-249">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9082b-249">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="9082b-251">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="9082b-251">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="9082b-252">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9082b-252">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="9082b-253">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9082b-253">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="9082b-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9082b-254">Test single sign-on</span></span>

<span data-ttu-id="9082b-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9082b-255">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="9082b-256">Ha a hozzáférési panelen TimeOffManager csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az TimeOffManager alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9082b-256">When you click the TimeOffManager tile in the Access Panel, you should get automatically signed-on to your TimeOffManager application.</span></span> <span data-ttu-id="9082b-257">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="9082b-257">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9082b-258">További források</span><span class="sxs-lookup"><span data-stu-id="9082b-258">Additional resources</span></span>

* [<span data-ttu-id="9082b-259">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9082b-259">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9082b-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9082b-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-timeoffmanager-tutorial/tutorial_general_203.png

