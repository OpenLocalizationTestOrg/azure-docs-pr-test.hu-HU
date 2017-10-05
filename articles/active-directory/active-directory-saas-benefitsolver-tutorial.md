---
title: "Oktatóanyag: Azure Active Directoryval integrált Benefitsolver |} Microsoft Docs"
description: "Megtudhatja, hogyan Benefitsolver használata az Azure Active Directoryval az egyszeri bejelentkezés, automatizált üzembe helyezést és további engedélyezéséhez!"
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: cf4529b1-3fb6-4475-82b7-2ceedcb70b3c
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 8a13dd5ebd872f86247158379b28bc291a9c9d83
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="b382e-103">Oktatóanyag: Azure Active Directoryval integrált Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="b382e-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="b382e-104">Ez az oktatóanyag célja az Azure és Benefitsolver integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b382e-104">The objective of this tutorial is to show the integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="b382e-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="b382e-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="b382e-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="b382e-106">A valid Azure subscription</span></span>
* <span data-ttu-id="b382e-107">Egy Benefitsolver egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b382e-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="b382e-108">Ez az oktatóanyag befejezése után az Azure AD felhasználók Benefitsolver rendelt tudják az alkalmazás használatával történő egyszeri bejelentkezéshez a [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b382e-108">After completing this tutorial, the Azure AD users you have assigned to Benefitsolver will be able to single sign into the application using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="b382e-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b382e-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="b382e-110">Az alkalmazás Benefitsolver-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b382e-110">Enabling the application integration for Benefitsolver</span></span>
2. <span data-ttu-id="b382e-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b382e-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="b382e-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="b382e-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="b382e-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b382e-113">Assigning users</span></span>

<span data-ttu-id="b382e-114">![A forgatókönyv](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="b382e-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-benefitsolver"></a><span data-ttu-id="b382e-115">Az alkalmazás Benefitsolver-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b382e-115">Enabling the application integration for Benefitsolver</span></span>
<span data-ttu-id="b382e-116">Ez a szakasz célja felvázoló Benefitsolver az alkalmazás-integráció engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b382e-116">The objective of this section is to outline how to enable the application integration for Benefitsolver.</span></span>

### <a name="to-enable-the-application-integration-for-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="b382e-117">Ahhoz, hogy az alkalmazás-integráció Benefitsolver, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b382e-117">To enable the application integration for Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="b382e-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="b382e-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="b382e-119">![Az Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="b382e-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="b382e-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="b382e-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="b382e-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="b382e-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
   <span data-ttu-id="b382e-122">![Alkalmazások](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="b382e-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="b382e-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="b382e-123">Click **Add** at the bottom of the page.</span></span>
   
   <span data-ttu-id="b382e-124">![Alkalmazás hozzáadása](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b382e-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="b382e-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="b382e-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
   <span data-ttu-id="b382e-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="b382e-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="b382e-127">Az a **keresőmezőbe**, típus **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="b382e-127">In the **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="b382e-128">![Alkalmazáskatalógusában](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="b382e-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="b382e-129">Az eredmények ablaktáblájában válassza **Benefitsolver**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b382e-129">In the results pane, select **Benefitsolver**, and then click **Complete** to add the application.</span></span>
   
   <span data-ttu-id="b382e-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="b382e-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="b382e-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b382e-131">Configure single sign-on</span></span>

<span data-ttu-id="b382e-132">Ez a szakasz célja felvázoló engedélyezése a felhasználók hitelesítéséhez Benefitsolver fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="b382e-132">The objective of this section is to outline how to enable users to authenticate to Benefitsolver with their account in Azure AD using federation based on the SAML protocol.</span></span>  

<span data-ttu-id="b382e-133">A Benefitsolver alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban, amelyhez egyéni attribútum leképezései hozzáadása a **saml-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="b382e-133">Your Benefitsolver application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your **saml token attributes** configuration.</span></span> 

<span data-ttu-id="b382e-134">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="b382e-134">The following screenshot shows an example for this.</span></span>

<span data-ttu-id="b382e-135">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="b382e-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="b382e-136">**Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b382e-136">**To configure single sign-on, perform the following steps:**</span></span>

1. <span data-ttu-id="b382e-137">A klasszikus Azure portálon a a **Benefitsolver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b382e-137">In the Azure classic portal, on the **Benefitsolver** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="b382e-138">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b382e-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="b382e-139">Az a **hová bejelentkezni Benefitsolver felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b382e-139">On the **How would you like users to sign on to Benefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="b382e-140">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b382e-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="b382e-141">Az a **Alkalmazásbeállítások konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b382e-141">On the **Configure App Settings** page, perform the following steps:</span></span>
   
   <span data-ttu-id="b382e-142">![Alkalmazásbeállítások konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Alkalmazásbeállítások konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b382e-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="b382e-143">Az a **URL-cím bejelentkezési** szövegmezőhöz típus **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="b382e-143">In the **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="b382e-144">Az a **válasz URL-CÍMEN** szövegmezőhöz típus **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="b382e-144">In the **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="b382e-145">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b382e-145">Click **Next**.</span></span>
4. <span data-ttu-id="b382e-146">A a **konfigurálhatja az egyszeri bejelentkezés Benefitsolver** töltse le a metaadatokat, kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen a metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="b382e-146">On the **Configure single sign-on at Benefitsolver** page, to download your metadata, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="b382e-147">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b382e-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="b382e-148">A letöltött metaadatfájl küldeni a Benefitsolver támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="b382e-148">Send the downloaded metadata file to your Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="b382e-149">A Benefitsolver terméktámogató csapat rendelkezésére áll, a tényleges SSO konfigurációs elvégzéséhez.</span><span class="sxs-lookup"><span data-stu-id="b382e-149">Your Benefitsolver support team has to do the actual SSO configuration.</span></span> <span data-ttu-id="b382e-150">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="b382e-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="b382e-151">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b382e-151">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="b382e-152">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="b382e-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="b382e-153">Kattintson a felső menüben **attribútumok** megnyitásához a **SAML-jogkivonat attribútumok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b382e-153">In the menu on the top, click **Attributes** to open the **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="b382e-154">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="b382e-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="b382e-155">A kötelező attribútum-leképezésekhez hozzáadásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b382e-155">To add the required attribute mappings, perform the following steps:</span></span>
   
   <span data-ttu-id="b382e-156">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="b382e-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="b382e-157">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b382e-157">Attribute Name</span></span> | <span data-ttu-id="b382e-158">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b382e-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="b382e-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="b382e-159">ClientID</span></span> |<span data-ttu-id="b382e-160">Ez az érték lekérése a Benefitsolver támogatási csoportjához kell.</span><span class="sxs-lookup"><span data-stu-id="b382e-160">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="b382e-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="b382e-161">ClientKey</span></span> |<span data-ttu-id="b382e-162">Ez az érték lekérése a Benefitsolver támogatási csoportjához kell.</span><span class="sxs-lookup"><span data-stu-id="b382e-162">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="b382e-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="b382e-163">LogoutURL</span></span> |<span data-ttu-id="b382e-164">Ez az érték lekérése a Benefitsolver támogatási csoportjához kell.</span><span class="sxs-lookup"><span data-stu-id="b382e-164">You need to get this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="b382e-165">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="b382e-165">EmployeeID</span></span> |<span data-ttu-id="b382e-166">Ez az érték lekérése a Benefitsolver támogatási csoportjához kell.</span><span class="sxs-lookup"><span data-stu-id="b382e-166">You need to get this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="b382e-167">Kattintson a fenti adatokat minden egyes sorhoz kapcsolódóan **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="b382e-167">For each data row in the table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="b382e-168">Az a **attribútumnév** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="b382e-168">In the **Attribute Name** textbox, type the attribute name shown for that row.</span></span>
   3. <span data-ttu-id="b382e-169">Az a **attribútumérték** szövegmező, válassza ki az adott sorhoz feltüntetett attribútumot értéket.</span><span class="sxs-lookup"><span data-stu-id="b382e-169">In the **Attribute Value** textbox, select the attribute value shown for that row.</span></span>
   4. <span data-ttu-id="b382e-170">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="b382e-170">Click **Complete**.</span></span>
9. <span data-ttu-id="b382e-171">Kattintson a **módosítások alkalmazásához**.</span><span class="sxs-lookup"><span data-stu-id="b382e-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="b382e-172">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b382e-172">Configure user provisioning</span></span>
<span data-ttu-id="b382e-173">Ahhoz, hogy az Azure AD-felhasználók Benefitsolver bejelentkezni, akkor ki kell építenie Benefitsolver be.</span><span class="sxs-lookup"><span data-stu-id="b382e-173">In order to enable Azure AD users to log into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="b382e-174">Benefitsolver, esetében alkalmazott adata (általában minden éjjel) a HRIS rendszerről nyilvántartásba fájl feltöltve az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b382e-174">In the case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="b382e-175">Bármely más Benefitsolver felhasználói fiók létrehozása eszközök vagy rendelkezés AAD felhasználói fiókokhoz Benefitsolver által nyújtott API-k.</span><span class="sxs-lookup"><span data-stu-id="b382e-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver to provision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="b382e-176">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b382e-176">Assigning users</span></span>
<span data-ttu-id="b382e-177">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="b382e-177">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

### <a name="to-assign-users-to-benefitsolver-perform-the-following-steps"></a><span data-ttu-id="b382e-178">Felhasználók hozzárendelése Benefitsolver, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b382e-178">To assign users to Benefitsolver, perform the following steps:</span></span>
1. <span data-ttu-id="b382e-179">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="b382e-179">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="b382e-180">Az a ** Benefitsolver ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="b382e-180">On the **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="b382e-181">![Felhasználók hozzárendelése](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="b382e-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="b382e-182">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b382e-182">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
   <span data-ttu-id="b382e-183">![Igen](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="b382e-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="b382e-184">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="b382e-184">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="b382e-185">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="b382e-185">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

