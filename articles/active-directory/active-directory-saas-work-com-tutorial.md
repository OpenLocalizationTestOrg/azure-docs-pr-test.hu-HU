---
title: "Oktatóanyag: Azure Active Directoryval integrált Work.com |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Work.com között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 98e6739e-eb24-46bd-9dd3-20b489839076
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 7cfec8e9ac12d43095483696a15c0580776d3114
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workcom"></a><span data-ttu-id="fe78a-103">Oktatóanyag: Azure Active Directoryval integrált Work.com</span><span class="sxs-lookup"><span data-stu-id="fe78a-103">Tutorial: Azure Active Directory integration with Work.com</span></span>

<span data-ttu-id="fe78a-104">Ebben az oktatóanyagban elsajátíthatja Work.com integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="fe78a-104">In this tutorial, you learn how to integrate Work.com with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="fe78a-105">Work.com integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="fe78a-105">Integrating Work.com with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="fe78a-106">Megadhatja a Work.com hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="fe78a-106">You can control in Azure AD who has access to Work.com</span></span>
- <span data-ttu-id="fe78a-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Work.com (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="fe78a-107">You can enable your users to automatically get signed-on to Work.com (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="fe78a-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="fe78a-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="fe78a-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="fe78a-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe78a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="fe78a-110">Prerequisites</span></span>

<span data-ttu-id="fe78a-111">Konfigurálása az Azure AD-integrációs Work.com, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="fe78a-111">To configure Azure AD integration with Work.com, you need the following items:</span></span>

- <span data-ttu-id="fe78a-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="fe78a-112">An Azure AD subscription</span></span>
- <span data-ttu-id="fe78a-113">Egy Work.com egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="fe78a-113">A Work.com single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="fe78a-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="fe78a-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="fe78a-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="fe78a-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="fe78a-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="fe78a-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="fe78a-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe78a-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="fe78a-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="fe78a-118">Scenario description</span></span>
<span data-ttu-id="fe78a-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="fe78a-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="fe78a-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="fe78a-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="fe78a-121">Adja hozzá a Work.com a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fe78a-121">Add Work.com from the gallery</span></span>
2. <span data-ttu-id="fe78a-122">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe78a-122">Configure and test Azure AD single sign-on</span></span>

## <a name="add-workcom-from-the-gallery"></a><span data-ttu-id="fe78a-123">Adja hozzá a Work.com a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="fe78a-123">Add Work.com from the gallery</span></span>
<span data-ttu-id="fe78a-124">Az Azure AD integrálása a Work.com konfigurálásához kell hozzáadnia Work.com a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="fe78a-124">To configure the integration of Work.com into Azure AD, you need to add Work.com from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="fe78a-125">**A gyűjteményből Work.com hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe78a-125">**To add Work.com from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="fe78a-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="fe78a-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="fe78a-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="fe78a-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="fe78a-133">Írja be a keresőmezőbe, **Work.com**, jelölje be **Work.com** eredmények panelen kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="fe78a-133">In the search box, type **Work.com**, select **Work.com** from results panel then click **Add** button to add the application.</span></span>

    ![Adja hozzá a gyűjteményből](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="fe78a-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe78a-135">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="fe78a-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe78a-136">In this section, you configure and test Azure AD single sign-on with Work.com based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="fe78a-137">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Work.com a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="fe78a-137">For single sign-on to work, Azure AD needs to know what the counterpart user in Work.com is to a user in Azure AD.</span></span> <span data-ttu-id="fe78a-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Work.com közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="fe78a-138">In other words, a link relationship between an Azure AD user and the related user in Work.com needs to be established.</span></span>

<span data-ttu-id="fe78a-139">Work.com, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="fe78a-139">In Work.com, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="fe78a-140">Az Azure AD egyszeri bejelentkezést a Work.com tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="fe78a-140">To configure and test Azure AD single sign-on with Work.com, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="fe78a-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="fe78a-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="fe78a-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="fe78a-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="fe78a-143">**[Work.com tesztfelhasználó létrehozása](#create-a-workcom-test-user)**  - való Britta Simon valami Work.com, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="fe78a-143">**[Create a Work.com test user](#create-a-workcom-test-user)** - to have a counterpart of Britta Simon in Work.com that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="fe78a-144">**[Rendelje hozzá az Azure AD-teszt felhasználó](#assign-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe78a-144">**[Assign the Azure AD test user](#assign-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="fe78a-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="fe78a-145">**[Test Single Sign-On](#test-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="fe78a-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="fe78a-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="fe78a-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Work.com alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="fe78a-147">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Work.com application.</span></span>

>[!NOTE]
><span data-ttu-id="fe78a-148">Egyszeri bejelentkezés beállításához szüksége egy Work.com egyéni tartománynév beállítása még.</span><span class="sxs-lookup"><span data-stu-id="fe78a-148">To configure single sign-on, you need to setup a custom Work.com domain name yet.</span></span> <span data-ttu-id="fe78a-149">Adja meg legalább egy tartomány nevét, a tartománynév tesztelése és telepíteni kell a teljes szervezet kell.</span><span class="sxs-lookup"><span data-stu-id="fe78a-149">You need to define at least a domain name, test your domain name, and deploy it to your entire organization.</span></span>

<span data-ttu-id="fe78a-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Work.com, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe78a-150">**To configure Azure AD single sign-on with Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe78a-151">Az Azure portálon a a **Work.com** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-151">In the Azure portal, on the **Work.com** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="fe78a-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="fe78a-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![SAML-alapú bejelentkezés](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_samlbase.png)

3. <span data-ttu-id="fe78a-155">Az a **Work.com tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe78a-155">On the **Work.com Domain and URLs** section, perform the following:</span></span>

    ![Work.com tartomány és az URL-címek szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_url.png)

    <span data-ttu-id="fe78a-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`http://<companyname>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="fe78a-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `http://<companyname>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="fe78a-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="fe78a-158">This value is not real.</span></span> <span data-ttu-id="fe78a-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="fe78a-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="fe78a-160">Ügyfél [Work.com ügyfél-támogatási csoport](https://help.salesforce.com/articleView?id=000159855&type=3) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="fe78a-160">Contact [Work.com Client support team](https://help.salesforce.com/articleView?id=000159855&type=3) to get this value.</span></span> 

4. <span data-ttu-id="fe78a-161">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="fe78a-161">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![SAML-aláíró tanúsítványa szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_certificate.png) 

5. <span data-ttu-id="fe78a-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-163">Click **Save** button.</span></span>

    ![Mentés gomb](./media/active-directory-saas-work-com-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="fe78a-165">A a **Work.com konfigurációs** kattintson **konfigurálása Work.com** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="fe78a-165">On the **Work.com Configuration** section, click **Configure Work.com** to open **Configure sign-on** window.</span></span> <span data-ttu-id="fe78a-166">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="fe78a-166">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Work.com konfigurációs szakasz](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_configure.png) 
7. <span data-ttu-id="fe78a-168">Jelentkezzen be rendszergazdaként a Work.com bérlő.</span><span class="sxs-lookup"><span data-stu-id="fe78a-168">Log in to your Work.com tenant as administrator.</span></span>

8. <span data-ttu-id="fe78a-169">Ugrás a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-169">Go to **Setup**.</span></span>
   
    <span data-ttu-id="fe78a-170">![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="fe78a-170">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

9. <span data-ttu-id="fe78a-171">A bal oldali navigációs panelen a a **Administer** kattintson **tartományok** bontsa ki a kapcsolódó szakaszt, és kattintson **saját tartomány** megnyitásához a **saját tartomány** lap.</span><span class="sxs-lookup"><span data-stu-id="fe78a-171">On the left navigation pane, in the **Administer** section, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
   
    <span data-ttu-id="fe78a-172">![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic767825.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="fe78a-172">![My Domain](./media/active-directory-saas-work-com-tutorial/ic767825.png "My Domain")</span></span>

10. <span data-ttu-id="fe78a-173">Győződjön meg arról, hogy a tartomány megfelelően van beállítva, győződjön meg arról, hogy "**4. lépés telepíti a felhasználók számára**", és tekintse át a "**saját tartománybeállítások**".</span><span class="sxs-lookup"><span data-stu-id="fe78a-173">To verify that your domain has been set up correctly, make sure that it is in “**Step 4 Deployed to Users**” and review your “**My Domain Settings**”.</span></span>
   
    <span data-ttu-id="fe78a-174">![A tartományi felhasználó számára központilag telepített](./media/active-directory-saas-work-com-tutorial/ic784377.png "tartományi felhasználó számára központilag telepített")</span><span class="sxs-lookup"><span data-stu-id="fe78a-174">![Domain Deployed to User](./media/active-directory-saas-work-com-tutorial/ic784377.png "Domain Deployed to User")</span></span>

11. <span data-ttu-id="fe78a-175">Jelentkezzen be a Work.com bérlő.</span><span class="sxs-lookup"><span data-stu-id="fe78a-175">Log in to your Work.com tenant.</span></span>

12. <span data-ttu-id="fe78a-176">Ugrás a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-176">Go to **Setup**.</span></span>
    
    <span data-ttu-id="fe78a-177">![A telepítő](./media/active-directory-saas-work-com-tutorial/ic794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="fe78a-177">![Setup](./media/active-directory-saas-work-com-tutorial/ic794108.png "Setup")</span></span>

13. <span data-ttu-id="fe78a-178">Bontsa ki a **biztonsági vezérlők** menüben, majd kattintson **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-178">Expand the **Security Controls** menu, and then click **Single Sign-On Settings**.</span></span>
    
    <span data-ttu-id="fe78a-179">![Az egyszeri bejelentkezés beállítások](./media/active-directory-saas-work-com-tutorial/ic794113.png "az egyszeri bejelentkezés beállításai")</span><span class="sxs-lookup"><span data-stu-id="fe78a-179">![Single Sign-On Settings](./media/active-directory-saas-work-com-tutorial/ic794113.png "Single Sign-On Settings")</span></span>

14. <span data-ttu-id="fe78a-180">Az a **egyszeri bejelentkezési beállítások** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fe78a-180">On the **Single Sign-On Settings** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="fe78a-181">![A SAML engedélyezett](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML engedélyezett")</span><span class="sxs-lookup"><span data-stu-id="fe78a-181">![SAML Enabled](./media/active-directory-saas-work-com-tutorial/ic781026.png "SAML Enabled")</span></span>
    
    <span data-ttu-id="fe78a-182">a.</span><span class="sxs-lookup"><span data-stu-id="fe78a-182">a.</span></span> <span data-ttu-id="fe78a-183">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-183">Select **SAML Enabled**.</span></span>
    
    <span data-ttu-id="fe78a-184">b.</span><span class="sxs-lookup"><span data-stu-id="fe78a-184">b.</span></span> <span data-ttu-id="fe78a-185">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="fe78a-185">Click **New**.</span></span>

15. <span data-ttu-id="fe78a-186">Az a **SAML-alapú egyszeri bejelentkezés beállítások** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="fe78a-186">In the **SAML Single Sign-On Settings** section, perform the following steps:</span></span>
    
    <span data-ttu-id="fe78a-187">![SAML-alapú egyszeri bejelentkezés beállítása](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML-alapú egyszeri bejelentkezés beállítása")</span><span class="sxs-lookup"><span data-stu-id="fe78a-187">![SAML Single Sign-On Setting](./media/active-directory-saas-work-com-tutorial/ic794114.png "SAML Single Sign-On Setting")</span></span>
    
    <span data-ttu-id="fe78a-188">a.</span><span class="sxs-lookup"><span data-stu-id="fe78a-188">a.</span></span> <span data-ttu-id="fe78a-189">Az a **neve** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="fe78a-189">In the **Name** textbox, type a name for your configuration.</span></span>  
       
    > [!NOTE]
    > <span data-ttu-id="fe78a-190">Értéket biztosító **neve** automatikusan feltölti a **API-név** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="fe78a-190">Providing a value for **Name** does automatically populate the **API Name** textbox.</span></span>
    
    <span data-ttu-id="fe78a-191">b.</span><span class="sxs-lookup"><span data-stu-id="fe78a-191">b.</span></span> <span data-ttu-id="fe78a-192">A **kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="fe78a-192">In **Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="fe78a-193">c.</span><span class="sxs-lookup"><span data-stu-id="fe78a-193">c.</span></span> <span data-ttu-id="fe78a-194">Az Azure-portálról letöltött tanúsítvány feltöltése, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-194">To upload the downloaded certificate from Azure portal, click **Browse**.</span></span>
    
    <span data-ttu-id="fe78a-195">d.</span><span class="sxs-lookup"><span data-stu-id="fe78a-195">d.</span></span> <span data-ttu-id="fe78a-196">Az a **entitásazonosító** szövegmezőhöz típus `https://salesforce-work.com`.</span><span class="sxs-lookup"><span data-stu-id="fe78a-196">In the **Entity Id** textbox, type `https://salesforce-work.com`.</span></span>
    
    <span data-ttu-id="fe78a-197">e.</span><span class="sxs-lookup"><span data-stu-id="fe78a-197">e.</span></span> <span data-ttu-id="fe78a-198">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz az összevonási azonosító felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-198">As **SAML Identity Type**, select **Assertion contains the Federation ID from the User object**.</span></span>
    
    <span data-ttu-id="fe78a-199">f.</span><span class="sxs-lookup"><span data-stu-id="fe78a-199">f.</span></span> <span data-ttu-id="fe78a-200">Mint **SAML-alapú identitás hely**, jelölje be **identitás a tulajdonos utasítás NameIdentfier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-200">As **SAML Identity Location**, select **Identity is in the NameIdentfier element of the Subject statement**.</span></span>
    
    <span data-ttu-id="fe78a-201">g.</span><span class="sxs-lookup"><span data-stu-id="fe78a-201">g.</span></span> <span data-ttu-id="fe78a-202">A **Identity Provider bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="fe78a-202">In **Identity Provider Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="fe78a-203">h.</span><span class="sxs-lookup"><span data-stu-id="fe78a-203">h.</span></span> <span data-ttu-id="fe78a-204">A **Identity Provider kijelentkezési URL-cím** szövegmezőhöz illessze be az értékét **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="fe78a-204">In **Identity Provider Logout URL** textbox, paste the value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="fe78a-205">i.</span><span class="sxs-lookup"><span data-stu-id="fe78a-205">i.</span></span> <span data-ttu-id="fe78a-206">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP Post**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-206">As **Service Provider Initiated Request Binding**, select **HTTP Post**.</span></span>
    
    <span data-ttu-id="fe78a-207">j.</span><span class="sxs-lookup"><span data-stu-id="fe78a-207">j.</span></span> <span data-ttu-id="fe78a-208">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-208">Click **Save**.</span></span>

16. <span data-ttu-id="fe78a-209">Kattintson a bal oldali navigációs panelen a Work.com klasszikus portál **tartományok** bontsa ki a kapcsolódó szakaszt, és kattintson **saját tartomány** megnyitásához a **saját tartomány** lap.</span><span class="sxs-lookup"><span data-stu-id="fe78a-209">In your Work.com classic portal, on the left navigation pane, click **Domain Management** to expand the related section, and then click **My Domain** to open the **My Domain** page.</span></span> 
    
    <span data-ttu-id="fe78a-210">![Saját tartomány](./media/active-directory-saas-work-com-tutorial/ic794115.png "saját tartomány")</span><span class="sxs-lookup"><span data-stu-id="fe78a-210">![My Domain](./media/active-directory-saas-work-com-tutorial/ic794115.png "My Domain")</span></span>

17. <span data-ttu-id="fe78a-211">Az a **saját tartomány** lap a **bejelentkezési oldal vállalati arculata** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-211">On the **My Domain** page, in the **Login Page Branding** section, click **Edit**.</span></span>
    
    <span data-ttu-id="fe78a-212">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic767826.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="fe78a-212">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic767826.png "Login Page Branding")</span></span>

14. <span data-ttu-id="fe78a-213">Az a **bejelentkezési oldal vállalati arculata** lap a **hitelesítési szolgáltatás** részben, a neve a **SAML SSO beállítások** jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="fe78a-213">On the **Login Page Branding** page, in the **Authentication Service** section, the name of your **SAML SSO Settings** is displayed.</span></span> <span data-ttu-id="fe78a-214">Válassza ki azt, és kattintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-214">Select it, and then click **Save**.</span></span>
    
    <span data-ttu-id="fe78a-215">![Bejelentkezési oldal vállalati arculatán alkalmazott](./media/active-directory-saas-work-com-tutorial/ic784366.png "bejelentkezési oldal vállalati arculatán alkalmazott")</span><span class="sxs-lookup"><span data-stu-id="fe78a-215">![Login Page Branding](./media/active-directory-saas-work-com-tutorial/ic784366.png "Login Page Branding")</span></span>

> [!TIP]
> <span data-ttu-id="fe78a-216">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="fe78a-216">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="fe78a-217">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="fe78a-217">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="fe78a-218">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="fe78a-218">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="fe78a-219">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fe78a-219">Create an Azure AD test user</span></span>
<span data-ttu-id="fe78a-220">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="fe78a-220">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="fe78a-222">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe78a-222">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="fe78a-223">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-223">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-work-com-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="fe78a-225">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-225">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Felhasználók és csoportok -> minden felhasználó](./media/active-directory-saas-work-com-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="fe78a-227">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="fe78a-227">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Hozzáadás](./media/active-directory-saas-work-com-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="fe78a-229">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="fe78a-229">On the **User** dialog page, perform the following steps:</span></span>
 
    ![A felhasználó párbeszédpanel lap](./media/active-directory-saas-work-com-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="fe78a-231">a.</span><span class="sxs-lookup"><span data-stu-id="fe78a-231">a.</span></span> <span data-ttu-id="fe78a-232">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-232">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="fe78a-233">b.</span><span class="sxs-lookup"><span data-stu-id="fe78a-233">b.</span></span> <span data-ttu-id="fe78a-234">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="fe78a-234">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="fe78a-235">c.</span><span class="sxs-lookup"><span data-stu-id="fe78a-235">c.</span></span> <span data-ttu-id="fe78a-236">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-236">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="fe78a-237">d.</span><span class="sxs-lookup"><span data-stu-id="fe78a-237">d.</span></span> <span data-ttu-id="fe78a-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-238">Click **Create**.</span></span>
 
### <a name="create-a-workcom-test-user"></a><span data-ttu-id="fe78a-239">Work.com tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="fe78a-239">Create a Work.com test user</span></span>
<span data-ttu-id="fe78a-240">Az Azure Active Directory-felhasználók jelentkezhetnek be kell hogy ki kell építenie Work.com.</span><span class="sxs-lookup"><span data-stu-id="fe78a-240">For Azure Active Directory users to be able to sign in, they must be provisioned to Work.com.</span></span> <span data-ttu-id="fe78a-241">Work.com, ha egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="fe78a-241">In the case of Work.com, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="fe78a-242">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="fe78a-242">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="fe78a-243">Jelentkezzen be rendszergazdaként a Work.com vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="fe78a-243">Sign on to your Work.com company site as an administrator.</span></span>

2. <span data-ttu-id="fe78a-244">Ugrás a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-244">Go to **Setup**.</span></span>
   
    <span data-ttu-id="fe78a-245">![A telepítő](./media/active-directory-saas-work-com-tutorial/IC794108.png "beállítása")</span><span class="sxs-lookup"><span data-stu-id="fe78a-245">![Setup](./media/active-directory-saas-work-com-tutorial/IC794108.png "Setup")</span></span>
3. <span data-ttu-id="fe78a-246">Ugrás a **felhasználók kezelése \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-246">Go to **Manage Users \> Users**.</span></span>
   
    <span data-ttu-id="fe78a-247">![Felhasználók kezelése](./media/active-directory-saas-work-com-tutorial/IC784369.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="fe78a-247">![Manage Users](./media/active-directory-saas-work-com-tutorial/IC784369.png "Manage Users")</span></span>

4. <span data-ttu-id="fe78a-248">Kattintson a **új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-248">Click **New User**.</span></span>
   
    <span data-ttu-id="fe78a-249">![Minden felhasználó](./media/active-directory-saas-work-com-tutorial/IC794117.png "minden felhasználó")</span><span class="sxs-lookup"><span data-stu-id="fe78a-249">![All Users](./media/active-directory-saas-work-com-tutorial/IC794117.png "All Users")</span></span>

5. <span data-ttu-id="fe78a-250">A felhasználó szerkesztése a szakaszban a következő lépésekkel, az attribútumok egy érvényes Azure AD-fiókot szeretné azokat a kapcsolódó szövegmezők rendelkezni:</span><span class="sxs-lookup"><span data-stu-id="fe78a-250">In the User Edit section, perform the following steps, in attributes of a valid Azure AD account you want to provision into the related textboxes:</span></span>
   
    <span data-ttu-id="fe78a-251">![Felhasználó szerkesztése](./media/active-directory-saas-work-com-tutorial/ic794118.png "felhasználó szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="fe78a-251">![User Edit](./media/active-directory-saas-work-com-tutorial/ic794118.png "User Edit")</span></span>
   
    <span data-ttu-id="fe78a-252">a.</span><span class="sxs-lookup"><span data-stu-id="fe78a-252">a.</span></span> <span data-ttu-id="fe78a-253">Az a **Utónév** szövegmezőhöz típusa a **Utónév** felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-253">In the **First Name** textbox, type the **first name** of the user **Britta**.</span></span>
    
    <span data-ttu-id="fe78a-254">b.</span><span class="sxs-lookup"><span data-stu-id="fe78a-254">b.</span></span> <span data-ttu-id="fe78a-255">Az a **Vezetéknév** szövegmezőhöz típusa a **Vezetéknév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-255">In the **Last Name** textbox, type the **last name** of the user **Simon**.</span></span>
    
    <span data-ttu-id="fe78a-256">c.</span><span class="sxs-lookup"><span data-stu-id="fe78a-256">c.</span></span> <span data-ttu-id="fe78a-257">Az a **Alias** szövegmezőhöz típusa a **neve** felhasználó **BrittaS**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-257">In the **Alias** textbox, type the **name** of the user **BrittaS**.</span></span>
    
    <span data-ttu-id="fe78a-258">d.</span><span class="sxs-lookup"><span data-stu-id="fe78a-258">d.</span></span> <span data-ttu-id="fe78a-259">Az a **E-mail** szövegmezőhöz típusa a **e-mail cím** felhasználó  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fe78a-259">In the **Email** textbox, type the **email address** of user **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="fe78a-260">e.</span><span class="sxs-lookup"><span data-stu-id="fe78a-260">e.</span></span> <span data-ttu-id="fe78a-261">Az a **felhasználónév** szövegmező, írja be például a felhasználó a felhasználónév  **Brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="fe78a-261">In the **User Name** textbox, type a user name of user like **Brittasimon@contoso.com**.</span></span>
    
    <span data-ttu-id="fe78a-262">f.</span><span class="sxs-lookup"><span data-stu-id="fe78a-262">f.</span></span> <span data-ttu-id="fe78a-263">Az a **becenév** szövegmező, adjon meg egy **becenév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-263">In the **Nick Name** textbox, type a **nick name** of user **Simon**.</span></span>
    
    <span data-ttu-id="fe78a-264">g.</span><span class="sxs-lookup"><span data-stu-id="fe78a-264">g.</span></span> <span data-ttu-id="fe78a-265">Válassza ki **szerepkör**, **felhasználói licenc**, és **profil**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-265">Select **Role**, **User License**, and **Profile**.</span></span>
    
    <span data-ttu-id="fe78a-266">h.</span><span class="sxs-lookup"><span data-stu-id="fe78a-266">h.</span></span> <span data-ttu-id="fe78a-267">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-267">Click **Save**.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="fe78a-268">Az Azure AD fióktulajdonos kap egy e-mailt hivatkozással erősítse meg a fiókot, mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="fe78a-268">The Azure AD account holder will get an email including a link to confirm the account before it becomes active.</span></span>
    > 
    > 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="fe78a-269">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="fe78a-269">Assign the Azure AD test user</span></span>

<span data-ttu-id="fe78a-270">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Work.com Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="fe78a-270">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Work.com.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="fe78a-272">**Britta Simon hozzárendelése Work.com, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="fe78a-272">**To assign Britta Simon to Work.com, perform the following steps:**</span></span>

1. <span data-ttu-id="fe78a-273">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-273">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="fe78a-275">Az alkalmazások listában válassza ki a **Work.com**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-275">In the applications list, select **Work.com**.</span></span>

    ![Alkalmazás listában Work.com](./media/active-directory-saas-work-com-tutorial/tutorial_work-com_app.png) 

3. <span data-ttu-id="fe78a-277">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="fe78a-277">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="fe78a-279">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="fe78a-279">Click **Add** button.</span></span> <span data-ttu-id="fe78a-280">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe78a-280">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="fe78a-282">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="fe78a-282">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="fe78a-283">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe78a-283">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="fe78a-284">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="fe78a-284">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="fe78a-285">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="fe78a-285">Test single sign-on</span></span>

<span data-ttu-id="fe78a-286">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="fe78a-286">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="fe78a-287">Ha a hozzáférési panelen Work.com csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Work.com alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="fe78a-287">When you click the Work.com tile in the Access Panel, you should get automatically signed-on to your Work.com application.</span></span>
<span data-ttu-id="fe78a-288">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="fe78a-288">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fe78a-289">További források</span><span class="sxs-lookup"><span data-stu-id="fe78a-289">Additional resources</span></span>

* [<span data-ttu-id="fe78a-290">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="fe78a-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="fe78a-291">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="fe78a-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-work-com-tutorial/tutorial_general_203.png

