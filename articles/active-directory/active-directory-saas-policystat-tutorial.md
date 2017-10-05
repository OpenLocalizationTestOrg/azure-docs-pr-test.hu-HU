---
title: "Oktatóanyag: Azure Active Directoryval integrált PolicyStat |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és PolicyStat között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: af5eb0f1-1c8e-4809-b0c4-8ccfb915ca42
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 704afd5515b02ce2a4fbf35da65fad74dc506271
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-policystat"></a><span data-ttu-id="66de1-103">Oktatóanyag: Azure Active Directoryval integrált PolicyStat</span><span class="sxs-lookup"><span data-stu-id="66de1-103">Tutorial: Azure Active Directory integration with PolicyStat</span></span>

<span data-ttu-id="66de1-104">Ebben az oktatóanyagban elsajátíthatja PolicyStat integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="66de1-104">In this tutorial, you learn how to integrate PolicyStat with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="66de1-105">PolicyStat integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="66de1-105">Integrating PolicyStat with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="66de1-106">Megadhatja a PolicyStat hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="66de1-106">You can control in Azure AD who has access to PolicyStat</span></span>
- <span data-ttu-id="66de1-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett PolicyStat (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="66de1-107">You can enable your users to automatically get signed-on to PolicyStat (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="66de1-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="66de1-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="66de1-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66de1-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="66de1-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="66de1-110">Prerequisites</span></span>

<span data-ttu-id="66de1-111">Konfigurálása az Azure AD-integrációs PolicyStat, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="66de1-111">To configure Azure AD integration with PolicyStat, you need the following items:</span></span>

- <span data-ttu-id="66de1-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="66de1-112">An Azure AD subscription</span></span>
- <span data-ttu-id="66de1-113">Egy PolicyStat egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="66de1-113">A PolicyStat single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="66de1-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="66de1-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="66de1-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="66de1-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="66de1-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="66de1-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="66de1-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="66de1-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="66de1-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="66de1-118">Scenario description</span></span>
<span data-ttu-id="66de1-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="66de1-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="66de1-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="66de1-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="66de1-121">A gyűjteményből PolicyStat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66de1-121">Adding PolicyStat from the gallery</span></span>
2. <span data-ttu-id="66de1-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66de1-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-policystat-from-the-gallery"></a><span data-ttu-id="66de1-123">A gyűjteményből PolicyStat hozzáadása</span><span class="sxs-lookup"><span data-stu-id="66de1-123">Adding PolicyStat from the gallery</span></span>
<span data-ttu-id="66de1-124">Az Azure AD integrálása a PolicyStat konfigurálásához kell hozzáadnia PolicyStat a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="66de1-124">To configure the integration of PolicyStat into Azure AD, you need to add PolicyStat from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="66de1-125">**A gyűjteményből PolicyStat hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66de1-125">**To add PolicyStat from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="66de1-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66de1-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="66de1-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="66de1-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="66de1-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66de1-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="66de1-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="66de1-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="66de1-133">Írja be a keresőmezőbe, **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="66de1-133">In the search box, type **PolicyStat**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_search.png)

5. <span data-ttu-id="66de1-135">Az eredmények panelen válassza ki a **PolicyStat**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="66de1-135">In the results panel, select **PolicyStat**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="66de1-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="66de1-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="66de1-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="66de1-138">In this section, you configure and test Azure AD single sign-on with PolicyStat based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="66de1-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó PolicyStat a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="66de1-139">For single sign-on to work, Azure AD needs to know what the counterpart user in PolicyStat is to a user in Azure AD.</span></span> <span data-ttu-id="66de1-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a PolicyStat közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="66de1-140">In other words, a link relationship between an Azure AD user and the related user in PolicyStat needs to be established.</span></span>

<span data-ttu-id="66de1-141">PolicyStat, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="66de1-141">In PolicyStat, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="66de1-142">Az Azure AD egyszeri bejelentkezést a PolicyStat tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="66de1-142">To configure and test Azure AD single sign-on with PolicyStat, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="66de1-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="66de1-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="66de1-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="66de1-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="66de1-145">**[PolicyStat tesztfelhasználó létrehozása](#creating-a-policystat-test-user)**  - való Britta Simon valami PolicyStat, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="66de1-145">**[Creating a PolicyStat test user](#creating-a-policystat-test-user)** - to have a counterpart of Britta Simon in PolicyStat that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="66de1-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="66de1-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="66de1-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="66de1-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="66de1-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66de1-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="66de1-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az PolicyStat alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="66de1-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your PolicyStat application.</span></span>

<span data-ttu-id="66de1-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés PolicyStat, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66de1-150">**To configure Azure AD single sign-on with PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="66de1-151">Az Azure portálon a a **PolicyStat** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="66de1-151">In the Azure portal, on the **PolicyStat** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="66de1-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="66de1-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_samlbase.png)

3. <span data-ttu-id="66de1-155">Az a **PolicyStat tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="66de1-155">On the **PolicyStat Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_url.png)

    <span data-ttu-id="66de1-157">a.</span><span class="sxs-lookup"><span data-stu-id="66de1-157">a.</span></span> <span data-ttu-id="66de1-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.policystat.com`</span><span class="sxs-lookup"><span data-stu-id="66de1-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com`</span></span>

    <span data-ttu-id="66de1-159">b.</span><span class="sxs-lookup"><span data-stu-id="66de1-159">b.</span></span> <span data-ttu-id="66de1-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://<companyname>.policystat.com/saml2/metadata/`</span><span class="sxs-lookup"><span data-stu-id="66de1-160">In the **Identifier** textbox, type a URL using the following pattern: `https://<companyname>.policystat.com/saml2/metadata/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="66de1-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="66de1-161">These values are not real.</span></span> <span data-ttu-id="66de1-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="66de1-162">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="66de1-163">Ügyfél [PolicyStat ügyfél-támogatási csoport](http://www.policystat.com/support/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="66de1-163">Contact [PolicyStat Client support team](http://www.policystat.com/support/) to get these values.</span></span> 
 
4. <span data-ttu-id="66de1-164">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="66de1-164">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_certificate.png) 

5. <span data-ttu-id="66de1-166">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez PolicyStat fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="66de1-166">The objective of this section is to outline how to enable users to authenticate to PolicyStat with their account in Azure AD using federation based on the SAML protocol.</span></span>

    <span data-ttu-id="66de1-167">A PolicyStat alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **SAML-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="66de1-167">The PolicyStat application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **SAML Token Attributes** configuration.</span></span>  

     <span data-ttu-id="66de1-168">Az alábbi képernyőfelvételen látható példa erre.</span><span class="sxs-lookup"><span data-stu-id="66de1-168">The following screenshot shows an example of this.</span></span>

     <span data-ttu-id="66de1-169">![Attribútumok](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="66de1-169">![Attributes](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_attribute.png "Attributes")</span></span>

6. <span data-ttu-id="66de1-170">A kötelező attribútum-leképezésekhez hozzáadásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="66de1-170">To add the required attribute mappings, perform the following steps:</span></span>

    | <span data-ttu-id="66de1-171">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="66de1-171">Attribute Name</span></span>    |   <span data-ttu-id="66de1-172">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="66de1-172">Attribute Value</span></span> |
    |------------------- | -------------------- |
    | <span data-ttu-id="66de1-173">egyedi azonosítója</span><span class="sxs-lookup"><span data-stu-id="66de1-173">uid</span></span> | <span data-ttu-id="66de1-174">ExtractMailPrefix([mail])</span><span class="sxs-lookup"><span data-stu-id="66de1-174">ExtractMailPrefix([mail])</span></span> |
    
    <span data-ttu-id="66de1-175">a.</span><span class="sxs-lookup"><span data-stu-id="66de1-175">a.</span></span> <span data-ttu-id="66de1-176">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66de1-176">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_addatribute.png)
    
    <span data-ttu-id="66de1-179">b.</span><span class="sxs-lookup"><span data-stu-id="66de1-179">b.</span></span> <span data-ttu-id="66de1-180">Az a **attribútumnév** szövegmezőhöz típus **uid**.</span><span class="sxs-lookup"><span data-stu-id="66de1-180">In the **Attribute Name** textbox, type **uid**.</span></span>

    <span data-ttu-id="66de1-181">c.</span><span class="sxs-lookup"><span data-stu-id="66de1-181">c.</span></span> <span data-ttu-id="66de1-182">Az a **attribútumérték** szövegmező, jelölje be **ExtractMailPrefix()**.</span><span class="sxs-lookup"><span data-stu-id="66de1-182">In the **Attribute Value** textbox, select **ExtractMailPrefix()**.</span></span>    
   
    <span data-ttu-id="66de1-183">d.</span><span class="sxs-lookup"><span data-stu-id="66de1-183">d.</span></span> <span data-ttu-id="66de1-184">Az a **Mail** listáról válassza ki **User.mail**.</span><span class="sxs-lookup"><span data-stu-id="66de1-184">From the **Mail** list, select **User.mail**.</span></span>
    
    <span data-ttu-id="66de1-185">e.</span><span class="sxs-lookup"><span data-stu-id="66de1-185">e.</span></span> <span data-ttu-id="66de1-186">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="66de1-186">Click **Ok**</span></span>

7. <span data-ttu-id="66de1-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="66de1-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="66de1-189">Egy másik webes böngészőablakban jelentkezzen be a PolicyStat vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="66de1-189">In a different web browser window, log into your PolicyStat company site as an administrator.</span></span>

9. <span data-ttu-id="66de1-190">Kattintson a **Admin** fülre, majd **egyszeri bejelentkezés konfigurációs** bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="66de1-190">Click the **Admin** tab, and then click **Single Sign-On Configuration** in left navigation pane.</span></span>
   
    <span data-ttu-id="66de1-191">![Rendszergazda menü](./media/active-directory-saas-policystat-tutorial/ic808633.png "Rendszergazda menü")</span><span class="sxs-lookup"><span data-stu-id="66de1-191">![Administrator Menu](./media/active-directory-saas-policystat-tutorial/ic808633.png "Administrator Menu")</span></span>

10. <span data-ttu-id="66de1-192">Az a **telepítő** szakaszban jelölje be **engedélyezése egyszeri bejelentkezéshez integrációs**.</span><span class="sxs-lookup"><span data-stu-id="66de1-192">In the **Setup** section, select **Enable Single Sign-on Integration**.</span></span>
   
    <span data-ttu-id="66de1-193">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808634.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="66de1-193">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808634.png "Single Sign-On Configuration")</span></span>

11. <span data-ttu-id="66de1-194">Kattintson a **attribútumok konfigurálása**, majd a a **attribútumok konfigurálása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="66de1-194">Click **Configure Attributes**, and then, in the **Configure Attributes** section, perform the following steps:</span></span>
   
    <span data-ttu-id="66de1-195">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808635.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="66de1-195">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808635.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="66de1-196">a.</span><span class="sxs-lookup"><span data-stu-id="66de1-196">a.</span></span> <span data-ttu-id="66de1-197">Az a **felhasználónév attribútum** szövegmezőhöz típus **uid**.</span><span class="sxs-lookup"><span data-stu-id="66de1-197">In the **Username Attribute** textbox, type **uid**.</span></span>

    <span data-ttu-id="66de1-198">b.</span><span class="sxs-lookup"><span data-stu-id="66de1-198">b.</span></span> <span data-ttu-id="66de1-199">Az a **Keresztnév attribútum** szövegmezőhöz típus **Keresztnév** felhasználó **Britta**.</span><span class="sxs-lookup"><span data-stu-id="66de1-199">In the **First Name Attribute** textbox, type **firstname** of user **Britta**.</span></span>

    <span data-ttu-id="66de1-200">c.</span><span class="sxs-lookup"><span data-stu-id="66de1-200">c.</span></span> <span data-ttu-id="66de1-201">Az a **utolsó Name attribútum** szövegmezőhöz típus **Vezetéknév** felhasználó **Simon**.</span><span class="sxs-lookup"><span data-stu-id="66de1-201">In the **Last Name Attribute** textbox, type **lastname** of user **Simon**.</span></span>

    <span data-ttu-id="66de1-202">d.</span><span class="sxs-lookup"><span data-stu-id="66de1-202">d.</span></span> <span data-ttu-id="66de1-203">Az a **E-mail attribútum** szövegmezőhöz típus **emailaddress** felhasználó  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="66de1-203">In the **Email Attribute** textbox, type **emailaddress** of user **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="66de1-204">e.</span><span class="sxs-lookup"><span data-stu-id="66de1-204">e.</span></span> <span data-ttu-id="66de1-205">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="66de1-205">Click **Save Changes**.</span></span>

12. <span data-ttu-id="66de1-206">Kattintson a **a kiállító terjesztési hely metaadatok**, majd a a **a kiállító terjesztési hely metaadatok** területen az alábbi lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="66de1-206">Click **Your IDP Metadata**, and then, in the **Your IDP Metadata** section, perform the following steps:</span></span>
   
    <span data-ttu-id="66de1-207">![Az egyszeri bejelentkezés konfigurációs](./media/active-directory-saas-policystat-tutorial/ic808636.png "az egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="66de1-207">![Single Sign-On Configuration](./media/active-directory-saas-policystat-tutorial/ic808636.png "Single Sign-On Configuration")</span></span>
   
    <span data-ttu-id="66de1-208">a.</span><span class="sxs-lookup"><span data-stu-id="66de1-208">a.</span></span> <span data-ttu-id="66de1-209">Nyissa meg a letöltött metaadatfájl, másolja a tartalmat, és illessze be azt a **a Identity Provider metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="66de1-209">Open your downloaded metadata file, copy the content, and  then paste it into the **Your Identity Provider Metadata** textbox.</span></span>

    <span data-ttu-id="66de1-210">b.</span><span class="sxs-lookup"><span data-stu-id="66de1-210">b.</span></span> <span data-ttu-id="66de1-211">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="66de1-211">Click **Save Changes**.</span></span>

> [!TIP]
> <span data-ttu-id="66de1-212">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="66de1-212">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="66de1-213">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="66de1-213">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="66de1-214">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="66de1-214">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="66de1-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66de1-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="66de1-216">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="66de1-216">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="66de1-218">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66de1-218">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="66de1-219">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="66de1-219">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="66de1-221">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="66de1-221">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="66de1-223">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="66de1-223">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="66de1-225">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="66de1-225">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-policystat-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="66de1-227">a.</span><span class="sxs-lookup"><span data-stu-id="66de1-227">a.</span></span> <span data-ttu-id="66de1-228">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="66de1-228">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="66de1-229">b.</span><span class="sxs-lookup"><span data-stu-id="66de1-229">b.</span></span> <span data-ttu-id="66de1-230">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="66de1-230">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="66de1-231">c.</span><span class="sxs-lookup"><span data-stu-id="66de1-231">c.</span></span> <span data-ttu-id="66de1-232">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="66de1-232">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="66de1-233">d.</span><span class="sxs-lookup"><span data-stu-id="66de1-233">d.</span></span> <span data-ttu-id="66de1-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="66de1-234">Click **Create**.</span></span>
 
### <a name="creating-a-policystat-test-user"></a><span data-ttu-id="66de1-235">PolicyStat tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="66de1-235">Creating a PolicyStat test user</span></span>

<span data-ttu-id="66de1-236">Ahhoz, hogy az Azure AD-felhasználók PolicyStat bejelentkezni, akkor ki kell építenie PolicyStat be.</span><span class="sxs-lookup"><span data-stu-id="66de1-236">In order to enable Azure AD users to log into PolicyStat, they must be provisioned into PolicyStat.</span></span>  

<span data-ttu-id="66de1-237">PolicyStat csak az idő a felhasználók átadása támogatja.</span><span class="sxs-lookup"><span data-stu-id="66de1-237">PolicyStat supports just in time user provisioning.</span></span> <span data-ttu-id="66de1-238">Ez azt jelenti, hogy nem kell a felhasználók manuális hozzáadása PolicyStat.</span><span class="sxs-lookup"><span data-stu-id="66de1-238">This means, you do not need to add the users manually to PolicyStat.</span></span> <span data-ttu-id="66de1-239">A felhasználók automatikusan az első bejelentkezés SSO keresztül a rendszer hozzáadják.</span><span class="sxs-lookup"><span data-stu-id="66de1-239">The users will get added automatically on their first login through SSO.</span></span>

>[!NOTE]
><span data-ttu-id="66de1-240">Bármely más PolicyStat felhasználói fiók létrehozása eszközök vagy PolicyStat kiépíteni az Azure AD-felhasználói fiókok által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="66de1-240">You can use any other PolicyStat user account creation tools or APIs provided by PolicyStat to provision Azure AD user accounts.</span></span>
> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="66de1-241">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="66de1-241">Assigning the Azure AD test user</span></span>

<span data-ttu-id="66de1-242">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés PolicyStat Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="66de1-242">In this section, you enable Britta Simon to use Azure single sign-on by granting access to PolicyStat.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="66de1-244">**Britta Simon hozzárendelése PolicyStat, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="66de1-244">**To assign Britta Simon to PolicyStat, perform the following steps:**</span></span>

1. <span data-ttu-id="66de1-245">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="66de1-245">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="66de1-247">Az alkalmazások listában válassza ki a **PolicyStat**.</span><span class="sxs-lookup"><span data-stu-id="66de1-247">In the applications list, select **PolicyStat**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-policystat-tutorial/tutorial_policystat_app.png) 

3. <span data-ttu-id="66de1-249">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="66de1-249">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="66de1-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="66de1-251">Click **Add** button.</span></span> <span data-ttu-id="66de1-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66de1-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="66de1-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="66de1-254">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="66de1-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66de1-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="66de1-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="66de1-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="66de1-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="66de1-257">Testing single sign-on</span></span>

<span data-ttu-id="66de1-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="66de1-258">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="66de1-259">Ha a hozzáférési panelen PolicyStat csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az PolicyStat alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="66de1-259">When you click the PolicyStat tile in the Access Panel, you should get automatically signed-on to your PolicyStat application.</span></span>
<span data-ttu-id="66de1-260">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="66de1-260">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="66de1-261">További források</span><span class="sxs-lookup"><span data-stu-id="66de1-261">Additional resources</span></span>

* [<span data-ttu-id="66de1-262">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="66de1-262">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="66de1-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="66de1-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-policystat-tutorial/tutorial_general_203.png

