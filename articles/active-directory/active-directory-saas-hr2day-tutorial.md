---
title: "Oktatóanyag: Azure Active Directoryval integrált által Merces HR2day |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és HR2day által Merces között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 853d08c9-27b1-48d4-b8e7-3705140eb67f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/24/2017
ms.author: jeedes
ms.openlocfilehash: 77e2fcf9c306259286b15767f3a992510d6d79d8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-hr2day-by-merces"></a><span data-ttu-id="617fb-103">Oktatóanyag: Azure Active Directoryval integrált HR2day Merces által</span><span class="sxs-lookup"><span data-stu-id="617fb-103">Tutorial: Azure Active Directory integration with HR2day by Merces</span></span>

<span data-ttu-id="617fb-104">Ebben az oktatóanyagban elsajátíthatja által Merces HR2day integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="617fb-104">In this tutorial, you learn how to integrate HR2day by Merces with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="617fb-105">HR2day által Merces integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="617fb-105">Integrating HR2day by Merces with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="617fb-106">Az Azure AD, aki hozzáfér HR2day Merces által szabályozható.</span><span class="sxs-lookup"><span data-stu-id="617fb-106">You can control in Azure AD who has access to HR2day by Merces.</span></span>
- <span data-ttu-id="617fb-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett HR2day Merces által a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="617fb-107">You can enable your users to automatically get signed in to HR2day by Merces with their Azure AD accounts.</span></span>
- <span data-ttu-id="617fb-108">A fiók egyetlen központi helyen--az Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="617fb-108">You can manage your accounts in one central location--the Azure portal.</span></span>

<span data-ttu-id="617fb-109">Az Azure AD SaaS integrálásáról további információért lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="617fb-109">For more information about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="617fb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="617fb-110">Prerequisites</span></span>

<span data-ttu-id="617fb-111">Konfigurálása az Azure AD-integrációs HR2day Merces által, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="617fb-111">To configure Azure AD integration with HR2day by Merces, you need the following items:</span></span>

- <span data-ttu-id="617fb-112">Az Azure AD-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="617fb-112">An Azure AD subscription.</span></span>
- <span data-ttu-id="617fb-113">Egy HR2day által Merces egyszeri bejelentkezés előfizetés engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="617fb-113">An HR2day by Merces single sign-on enabled subscription.</span></span>

> [!NOTE]
> <span data-ttu-id="617fb-114">Nem ajánlott, éles környezetben teszteléséhez lépéseit az oktatóanyag segítségével.</span><span class="sxs-lookup"><span data-stu-id="617fb-114">We don't recommend using a production environment to test the steps in this tutorial.</span></span>

<span data-ttu-id="617fb-115">Ez az oktatóanyag lépéseit teszteléséhez hajtsa végre az ezek az ajánlások:</span><span class="sxs-lookup"><span data-stu-id="617fb-115">To test the steps in this tutorial, follow these recommendations:</span></span>

- <span data-ttu-id="617fb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="617fb-116">Don't use your production environment unless it's necessary.</span></span>
- <span data-ttu-id="617fb-117">Első egy [ingyenes egy hónapos az Azure AD](https://azure.microsoft.com/pricing/free-trial/) Ha már nincs.</span><span class="sxs-lookup"><span data-stu-id="617fb-117">Get a [one-month free trial of Azure AD](https://azure.microsoft.com/pricing/free-trial/) if you don't already have it.</span></span>  

## <a name="scenario-description"></a><span data-ttu-id="617fb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="617fb-118">Scenario description</span></span>
<span data-ttu-id="617fb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="617fb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="617fb-120">Az itt ismertetett forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="617fb-120">The scenario that's outlined here consists of two main building blocks:</span></span>

1. <span data-ttu-id="617fb-121">HR2day által Merces hozzáadása a gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="617fb-121">Adding HR2day by Merces from the gallery.</span></span>
2. <span data-ttu-id="617fb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="617fb-122">Configuring and testing Azure AD single sign-on.</span></span>

## <a name="add-hr2day-by-merces-from-the-gallery"></a><span data-ttu-id="617fb-123">Adja hozzá a HR2day Merces által a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="617fb-123">Add HR2day by Merces from the gallery</span></span>
<span data-ttu-id="617fb-124">Az Azure AD integrálása a HR2day által Merces konfigurálásához vegye fel HR2day Merces által a gyűjteményből a felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="617fb-124">To configure the integration of HR2day by Merces into Azure AD, add HR2day by Merces from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="617fb-125">**A gyűjteményből által Merces HR2day hozzáadásához tegye a következőket:**</span><span class="sxs-lookup"><span data-stu-id="617fb-125">**To add HR2day by Merces from the gallery, take the following steps:**</span></span>

1. <span data-ttu-id="617fb-126">Az a [Azure-portálon](https://portal.azure.com), a bal oldali navigációs panelen válassza ki a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="617fb-126">In the [Azure portal](https://portal.azure.com), on the left navigation pane, select the **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="617fb-128">Ugrás a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="617fb-128">Go to **Enterprise applications**.</span></span> <span data-ttu-id="617fb-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="617fb-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="617fb-131">Egy új alkalmazást szeretne telepíteni, válassza ki a **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="617fb-131">To add a new application, select the **New application** button on the top of the dialog box.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="617fb-133">Írja be a keresőmezőbe, **által Merces HR2day**.</span><span class="sxs-lookup"><span data-stu-id="617fb-133">In the search box, type **HR2day by Merces**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_search.png)

5. <span data-ttu-id="617fb-135">Az eredmények panelen válassza ki a **által Merces HR2day**, majd válassza ki a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="617fb-135">In the results panel, select **HR2day by Merces**, and then select the **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_addfromgallery.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="617fb-137">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="617fb-137">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="617fb-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a HR2day "Britta Simon." nevű tesztfelhasználó alapján Merces által</span><span class="sxs-lookup"><span data-stu-id="617fb-138">In this section, you configure and test Azure AD single sign-on with HR2day by Merces based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="617fb-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, aki a párjukhoz felhasználó által Merces HR2day egy felhasználó számára az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="617fb-139">For single sign-on to work, Azure AD needs to know who the counterpart user in HR2day by Merces is to a user in Azure AD.</span></span> <span data-ttu-id="617fb-140">Más szóval kell HR2day Merces által az Azure AD-felhasználó és a kapcsolódó felhasználó közötti kapcsolat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="617fb-140">In other words, you need to establish a link between an Azure AD user and the related user in HR2day by Merces.</span></span>

<span data-ttu-id="617fb-141">HR2day Merces által, rendelje hozzá a **felhasználónév** az Azure AD- **felhasználónév** a kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="617fb-141">In HR2day by Merces, assign the **user name** in Azure AD to  **Username** to establish the relationship.</span></span>

<span data-ttu-id="617fb-142">Az Azure AD egyszeri bejelentkezést a HR2day által Merces tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="617fb-142">To configure and test Azure AD single sign-on with HR2day by Merces, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="617fb-143">[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on): lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="617fb-143">[Configure Azure AD single sign-on](#configuring-azure-ad-single-sign-on): Enable your users to use this feature.</span></span>
2. <span data-ttu-id="617fb-144">[Hozzon létre egy Azure AD-teszt felhasználó](#creating-an-azure-ad-test-user): tesztelése az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="617fb-144">[Create an Azure AD test user](#creating-an-azure-ad-test-user): Test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="617fb-145">[Hozzon létre egy HR2day Merces teszt felhasználó](#creating-an-hr2day-by-merces-test-user): hozzon létre egy megfelelője a Britta Simon HR2day által Merces, amely csatolva van a felhasználó az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="617fb-145">[Create an HR2day by Merces test user](#creating-an-hr2day-by-merces-test-user): Create a counterpart of Britta Simon in HR2day by Merces that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="617fb-146">[Rendelje hozzá az Azure AD-teszt felhasználó](#assigning-the-azure-ad-test-user): Britta Simon engedélyezése az Azure AD egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="617fb-146">[Assign the Azure AD test user](#assigning-the-azure-ad-test-user): Enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="617fb-147">[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on): Győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="617fb-147">[Test single sign-on](#testing-single-sign-on): Verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="617fb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="617fb-148">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="617fb-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez a HR2day Merces alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="617fb-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your HR2day by Merces application.</span></span>

<span data-ttu-id="617fb-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása HR2day Merces által, a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="617fb-150">**To configure Azure AD single sign-on with HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="617fb-151">Az Azure portálon a a **által Merces HR2day** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="617fb-151">In the Azure portal, on the **HR2day by Merces** application integration page, select **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="617fb-153">Az egyszeri bejelentkezést, engedélyezése a **egyszeri bejelentkezés** párbeszédpanelen jelölje ki **mód** , **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="617fb-153">To enable single sign-on, in the **Single sign-on** dialog box, select **Mode** as **SAML-based Sign-on**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_samlbase.png)

3. <span data-ttu-id="617fb-155">Az a **HR2day Merces tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="617fb-155">In the **HR2day by Merces Domain and URLs** section, take the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_url.png)

    <span data-ttu-id="617fb-157">a.</span><span class="sxs-lookup"><span data-stu-id="617fb-157">a.</span></span> <span data-ttu-id="617fb-158">Az a **bejelentkezési URL-cím** mezőbe írja be egy URL-cím használatával a következő mintát: `https://<tenantname>.force.com/<instancename>`.</span><span class="sxs-lookup"><span data-stu-id="617fb-158">In the **Sign-on URL** box, type a URL by using the following pattern: `https://<tenantname>.force.com/<instancename>`.</span></span>

    <span data-ttu-id="617fb-159">b.</span><span class="sxs-lookup"><span data-stu-id="617fb-159">b.</span></span> <span data-ttu-id="617fb-160">Az a **azonosító** mezőbe írja be egy URL-cím használatával a következő mintát: `https://hr2day.force.com/<companyname>`.</span><span class="sxs-lookup"><span data-stu-id="617fb-160">In the **Identifier** box, type a URL by using the following pattern: `https://hr2day.force.com/<companyname>`.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="617fb-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="617fb-161">These values are not real.</span></span> <span data-ttu-id="617fb-162">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="617fb-162">Update these values with the actual sign-on URL and identifier.</span></span> <span data-ttu-id="617fb-163">Lépjen kapcsolatba a [HR2day Merces ügyfél-támogatási csapat](mailto:servicedesk@merces.nl) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="617fb-163">Contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl) to get these values.</span></span> 
 


4. <span data-ttu-id="617fb-164">Az a **SAML-aláíró tanúsítványa** szakaszban jelölje be **Certificate(Base64)**, majd mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="617fb-164">On the **SAML Signing Certificate** section, select **Certificate(Base64)**, and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_certificate.png) 

5. <span data-ttu-id="617fb-166">Ez a szakasz ismerteti, hogyan szeretné a felhasználók hitelesítéséhez által Merces HR2day fiókkal az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="617fb-166">This section describes how to enable users to authenticate to HR2day by Merces with their account in Azure AD.</span></span> <span data-ttu-id="617fb-167">Ennek segítségével tehetik összevonási, amely az SAML protokoll alapul.</span><span class="sxs-lookup"><span data-stu-id="617fb-167">They do this by using federation that's based on the SAML protocol.</span></span>

    <span data-ttu-id="617fb-168">A HR2day Merces alkalmazás által meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat a SAML helyességi feltételek vár.</span><span class="sxs-lookup"><span data-stu-id="617fb-168">Your HR2day by Merces application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token.</span></span> <span data-ttu-id="617fb-169">Az alábbi képernyőfelvételen látható példa erre.</span><span class="sxs-lookup"><span data-stu-id="617fb-169">The following screenshot shows an example of this.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2day_00.png)
    
    > [!NOTE] 
    <span data-ttu-id="617fb-171">A SAML-előfeltétel konfigurálása előtt vegye fel a kapcsolatot a [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl) és az egyedi azonosító attribútum értékének kérhetnek a bérlő.</span><span class="sxs-lookup"><span data-stu-id="617fb-171">Before you can configure the SAML assertion, you must contact the [HR2day by Merces Client support team](mailto:servicedesk@merces.nl) and request the value of the unique identifier attribute for your tenant.</span></span> <span data-ttu-id="617fb-172">Ez az érték a következő szakaszban a lépések elvégzéséhez szüksége.</span><span class="sxs-lookup"><span data-stu-id="617fb-172">You need this value to complete the steps in the next section.</span></span> 

6. <span data-ttu-id="617fb-173">Az a **egyszeri bejelentkezés** párbeszédpanel a **felhasználói attribútumok** területen konfigurálja az SAML-jogkivonat attribútum a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="617fb-173">In the **Single sign-on** dialog box, in the **User Attributes** section, configure the SAML token attribute as shown in the following image.</span></span> <span data-ttu-id="617fb-174">Ezután az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="617fb-174">Then take the following steps.</span></span>
    
      | <span data-ttu-id="617fb-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="617fb-175">Attribute name</span></span>    |   <span data-ttu-id="617fb-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="617fb-176">Attribute value</span></span> |  
    | ------------------- | -------------------- |    
    | <span data-ttu-id="617fb-177">ATTR_LOGINCLAIM</span><span class="sxs-lookup"><span data-stu-id="617fb-177">ATTR_LOGINCLAIM</span></span> | <span data-ttu-id="617fb-178">csatlakoztatás ([mail] "102938475Z", "@"</span><span class="sxs-lookup"><span data-stu-id="617fb-178">join([mail],"102938475Z","@"</span></span> |
    
      <span data-ttu-id="617fb-179">a.</span><span class="sxs-lookup"><span data-stu-id="617fb-179">a.</span></span> <span data-ttu-id="617fb-180">Lehetőségre a **attribútum hozzáadása** párbeszédablakban válassza **Hozzáadás attribútum**.</span><span class="sxs-lookup"><span data-stu-id="617fb-180">To open the **Add Attribute** dialog, select **Add attribute**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_attribute_05.png)

    <span data-ttu-id="617fb-183">b.</span><span class="sxs-lookup"><span data-stu-id="617fb-183">b.</span></span> <span data-ttu-id="617fb-184">Az a **neve** mezőbe írja be **ATTR_LOGINCLAIM**.</span><span class="sxs-lookup"><span data-stu-id="617fb-184">In the **Name** box, type **ATTR_LOGINCLAIM**.</span></span>

    <span data-ttu-id="617fb-185">c.</span><span class="sxs-lookup"><span data-stu-id="617fb-185">c.</span></span> <span data-ttu-id="617fb-186">Az a **érték** listáról válassza ki **Join()**.</span><span class="sxs-lookup"><span data-stu-id="617fb-186">From the **Value** list, select **Join()**.</span></span>

    <span data-ttu-id="617fb-187">d.</span><span class="sxs-lookup"><span data-stu-id="617fb-187">d.</span></span> <span data-ttu-id="617fb-188">Az a **karakterlánc1** listáról válassza ki **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="617fb-188">From the **String1** list, select **user.mail**.</span></span>

    <span data-ttu-id="617fb-189">e.</span><span class="sxs-lookup"><span data-stu-id="617fb-189">e.</span></span> <span data-ttu-id="617fb-190">A **karakterlánc2**, írja be a HR2day csapata által biztosított egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="617fb-190">For **String2**, type the unique identifier that's provided by your HR2day team.</span></span>

    <span data-ttu-id="617fb-191">f.</span><span class="sxs-lookup"><span data-stu-id="617fb-191">f.</span></span> <span data-ttu-id="617fb-192">Az a **elválasztó** mezőbe írja be  **@** .</span><span class="sxs-lookup"><span data-stu-id="617fb-192">In the **Separator** box, type **@**.</span></span>
    
    <span data-ttu-id="617fb-193">g.</span><span class="sxs-lookup"><span data-stu-id="617fb-193">g.</span></span> <span data-ttu-id="617fb-194">Válassza ki **Ok**.</span><span class="sxs-lookup"><span data-stu-id="617fb-194">Select **Ok**.</span></span>

7. <span data-ttu-id="617fb-195">Válassza ki a **Mentés** gombot.</span><span class="sxs-lookup"><span data-stu-id="617fb-195">Select the **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="617fb-197">Az a **Merces konfigurációja HR2day** szakaszban jelölje be **konfigurálása HR2day által Merces** megnyitásához a **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="617fb-197">In the **HR2day by Merces Configuration** section, select **Configure HR2day by Merces** to open the **Configure sign-on** window.</span></span> <span data-ttu-id="617fb-198">Másolás a **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="617fb-198">Copy the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** from the **Quick Reference** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_configure.png) 

9. <span data-ttu-id="617fb-200">Egyszeri bejelentkezés konfigurálása az alkalmazáshoz, lépjen kapcsolatba a [Merces ügyfél-támogatási csapat HR2day](mailTo:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="617fb-200">To configure SSO  for your application, contact the [HR2day by Merces client support team](mailTo:servicedesk@merces.nl).</span></span> <span data-ttu-id="617fb-201">Csatlakoztassa a letöltött **Certificate(Base64)** fájlt az e-maileket.</span><span class="sxs-lookup"><span data-stu-id="617fb-201">Attach the downloaded **Certificate(Base64)** file to your email.</span></span> <span data-ttu-id="617fb-202">Is megadhatja a **Sign-Out URL-cím**, **SAML Entitásazonosító**, és **SAML-alapú egyszeri bejelentkezési URL-címe** , hogy az egyszeri bejelentkezés integráció konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="617fb-202">Also provide the **Sign-Out URL**, **SAML Entity ID**, and **SAML Single Sign-On Service URL** so that they can be configured for SSO integration.</span></span>

    > [!NOTE]
    ><span data-ttu-id="617fb-203">A Merces csapathoz említse meg, hogy ez az integráció kell-e a mintával kell beállítani az entitás azonosítója **https://hr2day.force.com/INSTANCENAME**.</span><span class="sxs-lookup"><span data-stu-id="617fb-203">Mention to the Merces team that this integration needs the Entity ID to be set with the pattern **https://hr2day.force.com/INSTANCENAME**.</span></span>

    > [!TIP]
    ><span data-ttu-id="617fb-204">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="617fb-204">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="617fb-205">Ez az alkalmazás a hozzáadása után a **Active Directory** > **vállalati alkalmazások** szakaszban jelölje be a **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="617fb-205">After you add this app from the **Active Directory** > **Enterprise Applications** section, select the **Single Sign-On** tab.</span></span> <span data-ttu-id="617fb-206">Majd hozzáférni a beágyazott dokumentáció keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="617fb-206">Then access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="617fb-207">További embedded dokumentációjából szolgáltatásáról a [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="617fb-207">You can read more about the embedded documentation feature in the [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="617fb-208">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="617fb-208">Create an Azure AD test user</span></span>
<span data-ttu-id="617fb-209">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="617fb-209">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="617fb-211">**Tesztfelhasználó létrehozása az Azure ad-ben, a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="617fb-211">**To create a test user in Azure AD, take the following steps:**</span></span>

1. <span data-ttu-id="617fb-212">Az a **Azure-portálon**, a bal oldali navigációs panelen válassza ki a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="617fb-212">In the **Azure portal**, on the left navigation pane, select the **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="617fb-214">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="617fb-214">To display the list of users, go to **Users and groups**, and then select **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="617fb-216">Lehetőségre a **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás** a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="617fb-216">To open the **User** dialog box, select **Add** on the top of the dialog box.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="617fb-218">Az a **felhasználói** párbeszédpanel mezőben a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="617fb-218">In the **User** dialog box, take the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-hr2day-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="617fb-220">a.</span><span class="sxs-lookup"><span data-stu-id="617fb-220">a.</span></span> <span data-ttu-id="617fb-221">Az a **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="617fb-221">In the **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="617fb-222">b.</span><span class="sxs-lookup"><span data-stu-id="617fb-222">b.</span></span> <span data-ttu-id="617fb-223">Az a **felhasználónév** mezőbe írja be a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="617fb-223">In the **User name** box, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="617fb-224">c.</span><span class="sxs-lookup"><span data-stu-id="617fb-224">c.</span></span> <span data-ttu-id="617fb-225">Válassza ki **megjelenítése jelszó**, és jegyezze fel a jelszót.</span><span class="sxs-lookup"><span data-stu-id="617fb-225">Select **Show Password**, and then write down the password.</span></span>

    <span data-ttu-id="617fb-226">d.</span><span class="sxs-lookup"><span data-stu-id="617fb-226">d.</span></span> <span data-ttu-id="617fb-227">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="617fb-227">Select **Create**.</span></span>
 
### <a name="create-an-hr2day-by-merces-test-user"></a><span data-ttu-id="617fb-228">Hozzon létre egy HR2day Merces teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="617fb-228">Create an HR2day by Merces test user</span></span>

<span data-ttu-id="617fb-229">Ez a szakasz célja Britta Simon a HR2day Merces által meghívott felhasználó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="617fb-229">The objective of this section is to create a user called Britta Simon in HR2day by Merces.</span></span> <span data-ttu-id="617fb-230">A felhasználók a HR2day fiók hozzáadásához dolgozni a [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="617fb-230">To add the users in the HR2day account, work with the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span> 

> [!NOTE]
> <span data-ttu-id="617fb-231">Ha a felhasználó manuális létrehozása, forduljon a [Merces ügyfél-támogatási csapat HR2day](mailto:servicedesk@merces.nl).</span><span class="sxs-lookup"><span data-stu-id="617fb-231">If you need to create a user manually, contact the [HR2day by Merces client support team](mailto:servicedesk@merces.nl).</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="617fb-232">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="617fb-232">Assign the Azure AD test user</span></span>

<span data-ttu-id="617fb-233">Ebben a szakaszban Britta Simon HR2day Merces által biztosított saját hozzáférés szerint használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="617fb-233">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to HR2day by Merces.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="617fb-235">**Britta Simon hozzárendelése HR2day Merces által, a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="617fb-235">**To assign Britta Simon to HR2day by Merces, take the following steps:**</span></span>

1. <span data-ttu-id="617fb-236">Az Azure portálon, az alkalmazások nézet megnyitásához, nyissa meg a könyvtár nézetet, és folytassa a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="617fb-236">In the Azure portal, open the applications view, go to the directory view, and then go to **Enterprise applications**.</span></span> <span data-ttu-id="617fb-237">Válassza ki, **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="617fb-237">Next, select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="617fb-239">Az alkalmazások listában válassza ki a **által Merces HR2day**.</span><span class="sxs-lookup"><span data-stu-id="617fb-239">In the applications list, select **HR2day by Merces**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-hr2day-tutorial/tutorial_hr2daybymerces_app.png) 

3. <span data-ttu-id="617fb-241">A bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="617fb-241">In the menu on the left, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="617fb-243">Válassza ki a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="617fb-243">Select the **Add** button.</span></span> <span data-ttu-id="617fb-244">Ezt követően a a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="617fb-244">Then, in the **Add Assignment** dialog box, select **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="617fb-246">Az a **felhasználók és csoportok** párbeszédpanel a **felhasználók** listáról válassza ki **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="617fb-246">In the **Users and groups** dialog box, in the **Users** list, select **Britta Simon**.</span></span>

6. <span data-ttu-id="617fb-247">Kattintson a **válasszon** gombra.</span><span class="sxs-lookup"><span data-stu-id="617fb-247">Click the **Select** button.</span></span>

7. <span data-ttu-id="617fb-248">Az a **hozzáadása hozzárendelés** párbeszédpanelen jelölje ki **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="617fb-248">In the **Add Assignment** dialog box, select **Assign**.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="617fb-249">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="617fb-249">Test single sign-on</span></span>

<span data-ttu-id="617fb-250">Ez a szakasz célja a hozzáférési Panel tesztelése az Azure AD egyszeri bejelentkezés beállításai.</span><span class="sxs-lookup"><span data-stu-id="617fb-250">The objective of this section is to test your Azure AD single sign-on configuration by using the Access Panel.</span></span>  

<span data-ttu-id="617fb-251">Ha a HR2day jelöl ki a hozzáférési panelen Merces csempe, automatikusan beolvasása jelentkezett be a HR2day Merces alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="617fb-251">When you select the HR2day by Merces tile in the Access Panel, you automatically get signed in  to your HR2day by Merces application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="617fb-252">További források</span><span class="sxs-lookup"><span data-stu-id="617fb-252">Additional resources</span></span>

* [<span data-ttu-id="617fb-253">SaaS-alkalmazások integrálása az Azure Active Directoryval kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="617fb-253">List of tutorials about how to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="617fb-254">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="617fb-254">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-hr2day-tutorial/tutorial_general_203.png

