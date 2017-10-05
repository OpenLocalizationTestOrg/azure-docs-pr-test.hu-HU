---
title: "Oktatóanyag: Azure Active Directoryval integrált Pluralsight |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Pluralsight között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4c3f07d2-4e1f-4ea3-9025-c663f1f2b7b4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeedes
ms.openlocfilehash: 62429643a108665544e42001d264046b5db1ec97
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-pluralsight"></a><span data-ttu-id="b906f-103">Oktatóanyag: Azure Active Directoryval integrált Pluralsight</span><span class="sxs-lookup"><span data-stu-id="b906f-103">Tutorial: Azure Active Directory integration with Pluralsight</span></span>

<span data-ttu-id="b906f-104">Ebben az oktatóanyagban elsajátíthatja Pluralsight integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b906f-104">In this tutorial, you learn how to integrate Pluralsight with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b906f-105">Pluralsight integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b906f-105">Integrating Pluralsight with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b906f-106">Megadhatja a Pluralsight hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b906f-106">You can control in Azure AD who has access to Pluralsight</span></span>
- <span data-ttu-id="b906f-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett Pluralsight (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b906f-107">You can enable your users to automatically get signed-on to Pluralsight (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b906f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b906f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="b906f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b906f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b906f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b906f-110">Prerequisites</span></span>

<span data-ttu-id="b906f-111">Konfigurálása az Azure AD-integrációs Pluralsight, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="b906f-111">To configure Azure AD integration with Pluralsight, you need the following items:</span></span>

- <span data-ttu-id="b906f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b906f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b906f-113">Egy Pluralsight egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b906f-113">A Pluralsight single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b906f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b906f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b906f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b906f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b906f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b906f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b906f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b906f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b906f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b906f-118">Scenario description</span></span>
<span data-ttu-id="b906f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b906f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b906f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b906f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b906f-121">A gyűjteményből Pluralsight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b906f-121">Adding Pluralsight from the gallery</span></span>
2. <span data-ttu-id="b906f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b906f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-pluralsight-from-the-gallery"></a><span data-ttu-id="b906f-123">A gyűjteményből Pluralsight hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b906f-123">Adding Pluralsight from the gallery</span></span>
<span data-ttu-id="b906f-124">Az Azure AD integrálása a Pluralsight konfigurálásához kell hozzáadnia Pluralsight a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b906f-124">To configure the integration of Pluralsight into Azure AD, you need to add Pluralsight from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b906f-125">**A gyűjteményből Pluralsight hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b906f-125">**To add Pluralsight from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b906f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b906f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b906f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b906f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b906f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b906f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b906f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="b906f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b906f-133">Írja be a keresőmezőbe, **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="b906f-133">In the search box, type **Pluralsight**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_search.png)

5. <span data-ttu-id="b906f-135">Az eredmények panelen válassza ki a **Pluralsight**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b906f-135">In the results panel, select **Pluralsight**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b906f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b906f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b906f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Pluralsight.</span><span class="sxs-lookup"><span data-stu-id="b906f-138">In this section, you configure and test Azure AD single sign-on with Pluralsight based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b906f-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó Pluralsight a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b906f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Pluralsight is to a user in Azure AD.</span></span> <span data-ttu-id="b906f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Pluralsight közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b906f-140">In other words, a link relationship between an Azure AD user and the related user in Pluralsight needs to be established.</span></span>

<span data-ttu-id="b906f-141">Pluralsight, rendelje hozzá a értékének a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="b906f-141">In Pluralsight, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="b906f-142">Az Azure AD egyszeri bejelentkezést a Pluralsight tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b906f-142">To configure and test Azure AD single sign-on with Pluralsight, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b906f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b906f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b906f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b906f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b906f-145">**[Pluralsight tesztfelhasználó létrehozása](#creating-a-pluralsight-test-user)**  - való Britta Simon valami Pluralsight, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b906f-145">**[Creating a Pluralsight test user](#creating-a-pluralsight-test-user)** - to have a counterpart of Britta Simon in Pluralsight that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="b906f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b906f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b906f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b906f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b906f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b906f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b906f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Pluralsight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b906f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Pluralsight application.</span></span>

<span data-ttu-id="b906f-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés Pluralsight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b906f-150">**To configure Azure AD single sign-on with Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="b906f-151">Az Azure portálon a a **Pluralsight** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b906f-151">In the Azure portal, on the **Pluralsight** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b906f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b906f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_samlbase.png)

3. <span data-ttu-id="b906f-155">Az a **Pluralsight tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b906f-155">On the **Pluralsight Domain and URLs** section, perform the following:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_url.png)

    <span data-ttu-id="b906f-157">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instance name>.pluralsight.com/sso/<company name>`</span><span class="sxs-lookup"><span data-stu-id="b906f-157">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instance name>.pluralsight.com/sso/<company name>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b906f-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="b906f-158">This value is not real.</span></span> <span data-ttu-id="b906f-159">Frissítse ezt az értéket a tényleges bejelentkezési URL-címet.</span><span class="sxs-lookup"><span data-stu-id="b906f-159">Update this value with the actual Sign-On URL.</span></span> <span data-ttu-id="b906f-160">Ügyfél [Pluralsight ügyfél-támogatási csoport](mailto:support@pluralsight.com) lekérni ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="b906f-160">Contact [Pluralsight Client support team](mailto:support@pluralsight.com) to get this value.</span></span> 
 


4. <span data-ttu-id="b906f-161">Az a **SAML-aláíró tanúsítványa** területen kattintson **metaadatainak XML-kódja** és mentse a metaadat-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b906f-161">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_certificate.png) 

5. <span data-ttu-id="b906f-163">Ez a szakasz célja az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon és egyszeri bejelentkezés konfigurálása az Pluralsight alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b906f-163">The objective of this section is to enable Azure AD single sign-on in the Azure portal and to configure SSO in the Pluralsight application.</span></span>

    <span data-ttu-id="b906f-164">A Pluralsight alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="b906f-164">The Pluralsight application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="b906f-165">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="b906f-165">The following screenshot shows an example for this.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_attribute.png)

    >[!NOTE]
    ><span data-ttu-id="b906f-167">Azt is megteheti a **"Egyedi azonosító"** attribútum EmployeeID vagy más hasonló a megfelelő értékkel, amely megfelel a szervezet számára.</span><span class="sxs-lookup"><span data-stu-id="b906f-167">You can also add the **"Unique ID"** attribute with the appropriate value like EmployeeID or something else which suits for your organization.</span></span> <span data-ttu-id="b906f-168">Vegye figyelembe azt is, hogy ez nem szükséges attribútuma; azonban úgy, hogy a felhasználó egyedi azonosítására is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="b906f-168">Also note that this is not the required attribute; however, you can add it to  identify the unique user.</span></span> 

6. <span data-ttu-id="b906f-169">A szükséges hozzáadandó **SAML-jogkivonat attribútumok**, az alábbi táblázatban szereplő minden egyes sorhoz kapcsolódóan végezze el a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b906f-169">To add the required **SAML token attributes**, for each row shown in the table below, perform the following steps:</span></span>
   
   | <span data-ttu-id="b906f-170">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b906f-170">Attribute Name</span></span> | <span data-ttu-id="b906f-171">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b906f-171">Attribute Value</span></span> |
   | ---| --- |
   | <span data-ttu-id="b906f-172">Utónév</span><span class="sxs-lookup"><span data-stu-id="b906f-172">First Name</span></span> |<span data-ttu-id="b906f-173">User.givenName</span><span class="sxs-lookup"><span data-stu-id="b906f-173">user.givenname</span></span> |
   | <span data-ttu-id="b906f-174">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="b906f-174">Last Name</span></span> |<span data-ttu-id="b906f-175">User.surname</span><span class="sxs-lookup"><span data-stu-id="b906f-175">user.surname</span></span> |
   | <span data-ttu-id="b906f-176">E-mail cím</span><span class="sxs-lookup"><span data-stu-id="b906f-176">Email</span></span> |<span data-ttu-id="b906f-177">User.mail</span><span class="sxs-lookup"><span data-stu-id="b906f-177">user.mail</span></span> |
   
   <span data-ttu-id="b906f-178">a.</span><span class="sxs-lookup"><span data-stu-id="b906f-178">a.</span></span> <span data-ttu-id="b906f-179">Kattintson a **hozzáadása a felhasználói attribútum** megnyitásához a **hozzáadása felhasználói Attribure** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b906f-179">Click **add user attribute** to open the **Add User Attribure** dialog.</span></span>
    
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_addattribute.png)
  
   <span data-ttu-id="b906f-181">b.</span><span class="sxs-lookup"><span data-stu-id="b906f-181">b.</span></span> <span data-ttu-id="b906f-182">Az a **attribútumnév** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="b906f-182">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
  
   <span data-ttu-id="b906f-183">c.</span><span class="sxs-lookup"><span data-stu-id="b906f-183">c.</span></span> <span data-ttu-id="b906f-184">Az a **attribútumérték** listára, válassza ki a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="b906f-184">From the **Attribute Value** list, select the attribute value shown for that row.</span></span>
  
   <span data-ttu-id="b906f-185">d.</span><span class="sxs-lookup"><span data-stu-id="b906f-185">d.</span></span> <span data-ttu-id="b906f-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="b906f-186">Click **Ok**.</span></span>    

7. <span data-ttu-id="b906f-187">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b906f-187">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b906f-189">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba [Pluralsight szolgáltatások](mailTo:professionalservices@pluralsight.com) vonja össze, és adja meg a letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="b906f-189">To get SSO configured for your application, contact [Pluralsight Professional Services](mailTo:professionalservices@pluralsight.com) team and provide the downloaded metadata file.</span></span>

> [!TIP]
> <span data-ttu-id="b906f-190">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="b906f-190">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="b906f-191">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="b906f-191">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="b906f-192">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b906f-192">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b906f-193">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b906f-193">Creating an Azure AD test user</span></span>
<span data-ttu-id="b906f-194">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b906f-194">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b906f-196">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b906f-196">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b906f-197">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b906f-197">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b906f-199">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b906f-199">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b906f-201">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b906f-201">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b906f-203">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b906f-203">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-pluralsight-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b906f-205">a.</span><span class="sxs-lookup"><span data-stu-id="b906f-205">a.</span></span> <span data-ttu-id="b906f-206">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b906f-206">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b906f-207">b.</span><span class="sxs-lookup"><span data-stu-id="b906f-207">b.</span></span> <span data-ttu-id="b906f-208">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b906f-208">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b906f-209">c.</span><span class="sxs-lookup"><span data-stu-id="b906f-209">c.</span></span> <span data-ttu-id="b906f-210">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b906f-210">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b906f-211">d.</span><span class="sxs-lookup"><span data-stu-id="b906f-211">d.</span></span> <span data-ttu-id="b906f-212">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b906f-212">Click **Create**.</span></span>
 
### <a name="creating-a-pluralsight-test-user"></a><span data-ttu-id="b906f-213">Pluralsight tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b906f-213">Creating a Pluralsight test user</span></span>

<span data-ttu-id="b906f-214">Ez a szakasz célja Pluralsight Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b906f-214">The objective of this section is to create a user called Britta Simon in Pluralsight.</span></span> <span data-ttu-id="b906f-215">Adjon együttműködve [Pluralsight ügyfél-támogatási csoport](mailto:support@pluralsight.com) a felhasználók hozzáadása a Pluralsight fiók.</span><span class="sxs-lookup"><span data-stu-id="b906f-215">Please work with [Pluralsight Client support team](mailto:support@pluralsight.com) to add the users in the Pluralsight account.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b906f-216">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b906f-216">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b906f-217">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Pluralsight Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b906f-217">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Pluralsight.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b906f-219">**Britta Simon hozzárendelése Pluralsight, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b906f-219">**To assign Britta Simon to Pluralsight, perform the following steps:**</span></span>

1. <span data-ttu-id="b906f-220">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b906f-220">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b906f-222">Az alkalmazások listában válassza ki a **Pluralsight**.</span><span class="sxs-lookup"><span data-stu-id="b906f-222">In the applications list, select **Pluralsight**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-pluralsight-tutorial/tutorial_pluralsight_app.png) 

3. <span data-ttu-id="b906f-224">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b906f-224">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b906f-226">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b906f-226">Click **Add** button.</span></span> <span data-ttu-id="b906f-227">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b906f-227">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b906f-229">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b906f-229">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b906f-230">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b906f-230">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b906f-231">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b906f-231">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b906f-232">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b906f-232">Testing single sign-on</span></span>

<span data-ttu-id="b906f-233">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="b906f-233">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b906f-234">Ha a hozzáférési panelen Pluralsight csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Pluralsight alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b906f-234">When you click the Pluralsight tile in the Access Panel, you should get automatically signed-on to your Pluralsight application.</span></span> <span data-ttu-id="b906f-235">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b906f-235">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b906f-236">További források</span><span class="sxs-lookup"><span data-stu-id="b906f-236">Additional resources</span></span>

* [<span data-ttu-id="b906f-237">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b906f-237">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b906f-238">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b906f-238">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-pluralsight-tutorial/tutorial_general_203.png

