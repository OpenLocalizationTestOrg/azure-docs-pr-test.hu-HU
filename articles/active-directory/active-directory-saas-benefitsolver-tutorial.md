---
title: "Oktatóanyag: Azure Active Directoryval integrált Benefitsolver |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Benefitsolver az Azure Active Directory tooenable egyszeri bejelentkezést, automatizált üzembe helyezést és további!"
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
ms.openlocfilehash: 5bb8511ef9be1e386956188a93e899d6ebe56ed5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-benefitsolver"></a><span data-ttu-id="e0e8e-103">Oktatóanyag: Azure Active Directoryval integrált Benefitsolver</span><span class="sxs-lookup"><span data-stu-id="e0e8e-103">Tutorial: Azure Active Directory integration with Benefitsolver</span></span>
<span data-ttu-id="e0e8e-104">hello Ez az oktatóanyag célja az Azure és Benefitsolver tooshow hello integrációját.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-104">hello objective of this tutorial is tooshow hello integration of Azure and Benefitsolver.</span></span>  

<span data-ttu-id="e0e8e-105">Ebben az oktatóanyagban leírt hello forgatókönyv feltételezi, hogy már rendelkezik a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-105">hello scenario outlined in this tutorial assumes that you already have hello following items:</span></span>

* <span data-ttu-id="e0e8e-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="e0e8e-106">A valid Azure subscription</span></span>
* <span data-ttu-id="e0e8e-107">Egy Benefitsolver egyszeri bejelentkezés (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="e0e8e-107">A Benefitsolver single sign-on (SSO) enabled subscription</span></span>

<span data-ttu-id="e0e8e-108">Ez az oktatóanyag befejezése után tooBenefitsolver hozzárendelt hello Azure AD felhasználók fognak tudni toosingle jelentkezzen be a hello alkalmazást hello [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0e8e-108">After completing this tutorial, hello Azure AD users you have assigned tooBenefitsolver will be able toosingle sign into hello application using hello [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="e0e8e-109">Ebben az oktatóanyagban leírt hello forgatókönyv építőelemei következő hello áll:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-109">hello scenario outlined in this tutorial consists of hello following building blocks:</span></span>

1. <span data-ttu-id="e0e8e-110">Benefitsolver hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e0e8e-110">Enabling hello application integration for Benefitsolver</span></span>
2. <span data-ttu-id="e0e8e-111">Egyszeri bejelentkezés (SSO) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e0e8e-111">Configuring single sign-on (SSO)</span></span>
3. <span data-ttu-id="e0e8e-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="e0e8e-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="e0e8e-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e0e8e-113">Assigning users</span></span>

<span data-ttu-id="e0e8e-114">![A forgatókönyv](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-114">![Scenario](./media/active-directory-saas-benefitsolver-tutorial/IC804820.png "Scenario")</span></span>

## <a name="enabling-hello-application-integration-for-benefitsolver"></a><span data-ttu-id="e0e8e-115">Benefitsolver hello alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e0e8e-115">Enabling hello application integration for Benefitsolver</span></span>
<span data-ttu-id="e0e8e-116">hello ebben a szakaszban célja toooutline hogyan tooenable hello alkalmazásintegráció Benefitsolver.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-116">hello objective of this section is toooutline how tooenable hello application integration for Benefitsolver.</span></span>

### <a name="tooenable-hello-application-integration-for-benefitsolver-perform-hello-following-steps"></a><span data-ttu-id="e0e8e-117">tooenable hello alkalmazásintegráció Benefitsolver, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-117">tooenable hello application integration for Benefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="e0e8e-118">Hello hello bal oldali navigációs ablaktáblán, a klasszikus Azure portálon kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-118">In hello Azure classic portal, on hello left navigation pane, click **Active Directory**.</span></span>
   
   <span data-ttu-id="e0e8e-119">![Az Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-119">![Active Directory](./media/active-directory-saas-benefitsolver-tutorial/IC700993.png "Active Directory")</span></span>
2. <span data-ttu-id="e0e8e-120">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-120">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="e0e8e-121">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-121">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
   <span data-ttu-id="e0e8e-122">![Alkalmazások](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-122">![Applications](./media/active-directory-saas-benefitsolver-tutorial/IC700994.png "Applications")</span></span>
4. <span data-ttu-id="e0e8e-123">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-123">Click **Add** at hello bottom of hello page.</span></span>
   
   <span data-ttu-id="e0e8e-124">![Alkalmazás hozzáadása](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-124">![Add application](./media/active-directory-saas-benefitsolver-tutorial/IC749321.png "Add application")</span></span>
5. <span data-ttu-id="e0e8e-125">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-125">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
   <span data-ttu-id="e0e8e-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-126">![Add an application from gallerry](./media/active-directory-saas-benefitsolver-tutorial/IC749322.png "Add an application from gallerry")</span></span>
6. <span data-ttu-id="e0e8e-127">A hello **keresőmezőbe**, típus **Benefitsolver**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-127">In hello **search box**, type **Benefitsolver**.</span></span>
   
   <span data-ttu-id="e0e8e-128">![Alkalmazáskatalógusában](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-128">![Application Gallery](./media/active-directory-saas-benefitsolver-tutorial/IC804821.png "Application Gallery")</span></span>
7. <span data-ttu-id="e0e8e-129">Hello eredmények ablaktábláján jelöljön ki **Benefitsolver**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-129">In hello results pane, select **Benefitsolver**, and then click **Complete** tooadd hello application.</span></span>
   
   <span data-ttu-id="e0e8e-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-130">![Benefitssolver](./media/active-directory-saas-benefitsolver-tutorial/IC804822.png "Benefitssolver")</span></span>
   
## <a name="configure-single-sign-on"></a><span data-ttu-id="e0e8e-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e0e8e-131">Configure single sign-on</span></span>

<span data-ttu-id="e0e8e-132">hello ebben a szakaszban célja toooutline hogyan tooenable felhasználók tooauthenticate tooBenefitsolver fiókkal az Azure AD összevonási használatával hello SAML protokoll alapján.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-132">hello objective of this section is toooutline how tooenable users tooauthenticate tooBenefitsolver with their account in Azure AD using federation based on hello SAML protocol.</span></span>  

<span data-ttu-id="e0e8e-133">A Benefitsolver alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, amelyhez tooadd egyéni attribútum hozzárendelések tooyour **saml-jogkivonat attribútumok** konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-133">Your Benefitsolver application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour **saml token attributes** configuration.</span></span> 

<span data-ttu-id="e0e8e-134">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-134">hello following screenshot shows an example for this.</span></span>

<span data-ttu-id="e0e8e-135">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-135">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>

<span data-ttu-id="e0e8e-136">**tooconfigure egyszeri bejelentkezés, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="e0e8e-136">**tooconfigure single sign-on, perform hello following steps:**</span></span>

1. <span data-ttu-id="e0e8e-137">A klasszikus Azure portálon, a hello hello **Benefitsolver** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-137">In hello Azure classic portal, on hello **Benefitsolver** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="e0e8e-138">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-138">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804824.png "Configure Single Sign-On")</span></span>
2. <span data-ttu-id="e0e8e-139">A hello **hogyan szeretné tooBenefitsolver a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-139">On hello **How would you like users toosign on tooBenefitsolver** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
   <span data-ttu-id="e0e8e-140">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-140">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804825.png "Configure Single Sign-On")</span></span>
3. <span data-ttu-id="e0e8e-141">A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-141">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
   <span data-ttu-id="e0e8e-142">![Alkalmazásbeállítások konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Alkalmazásbeállítások konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-142">![Configure App Settings](./media/active-directory-saas-benefitsolver-tutorial/IC804826.png "Configure App Settings")</span></span>
   
   1. <span data-ttu-id="e0e8e-143">A hello **URL-cím bejelentkezési** szövegmezőhöz típus **http://azure.benefitsolver.com**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-143">In hello **Sign On URL** textbox, type **http://azure.benefitsolver.com**.</span></span>
   2. <span data-ttu-id="e0e8e-144">A hello **válasz URL-CÍMEN** szövegmezőhöz típus **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-144">In hello **Reply URL** textbox, type **https://www.benefitsolver.com/benefits/BenefitSolverView?page_name=single_signon_saml**.</span></span>  
   3. <span data-ttu-id="e0e8e-145">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-145">Click **Next**.</span></span>
4. <span data-ttu-id="e0e8e-146">A hello **konfigurálhatja az egyszeri bejelentkezés Benefitsolver** lap, toodownload a metaadatok kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen hello metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-146">On hello **Configure single sign-on at Benefitsolver** page, toodownload your metadata, click **Download metadata**, and then save hello metadata file locally on your computer.</span></span>
   
   <span data-ttu-id="e0e8e-147">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-147">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804827.png "Configure Single Sign-On")</span></span>
5. <span data-ttu-id="e0e8e-148">Küldés letöltött hello metaadatok fájl tooyour Benefitsolver támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-148">Send hello downloaded metadata file tooyour Benefitsolver support team.</span></span>
   
   >[!NOTE]
   ><span data-ttu-id="e0e8e-149">A Benefitsolver támogatási csoport rendelkezik toodo hello tényleges SSO konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-149">Your Benefitsolver support team has toodo hello actual SSO configuration.</span></span> <span data-ttu-id="e0e8e-150">Ha egyszeri bejelentkezés engedélyezve van az előfizetés értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-150">You will get a notification when SSO has been enabled for your subscription.</span></span>
   >

6. <span data-ttu-id="e0e8e-151">Hello a klasszikus Azure portálon, jelölje ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson a **Complete** tooclose hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-151">On hello Azure classic portal, select hello single sign-on configuration confirmation, and then click **Complete** tooclose hello **Configure Single Sign On** dialog.</span></span>
   
   <span data-ttu-id="e0e8e-152">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-152">![Configure Single Sign-On](./media/active-directory-saas-benefitsolver-tutorial/IC804828.png "Configure Single Sign-On")</span></span>
7. <span data-ttu-id="e0e8e-153">Hello hello felső menüben kattintson a **attribútumok** tooopen hello **SAML-jogkivonat attribútumok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-153">In hello menu on hello top, click **Attributes** tooopen hello **SAML Token Attributes** dialog.</span></span>
   
   <span data-ttu-id="e0e8e-154">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-154">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC795920.png "Attributes")</span></span>
8. <span data-ttu-id="e0e8e-155">tooadd szükséges hello attribútum-leképezésekhez, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-155">tooadd hello required attribute mappings, perform hello following steps:</span></span>
   
   <span data-ttu-id="e0e8e-156">![Attribútumok](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "attribútumok")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-156">![Attributes](./media/active-directory-saas-benefitsolver-tutorial/IC804823.png "Attributes")</span></span>
   
   | <span data-ttu-id="e0e8e-157">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="e0e8e-157">Attribute Name</span></span> | <span data-ttu-id="e0e8e-158">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="e0e8e-158">Attribute Value</span></span> |
   | --- | --- |
   | <span data-ttu-id="e0e8e-159">ClientID</span><span class="sxs-lookup"><span data-stu-id="e0e8e-159">ClientID</span></span> |<span data-ttu-id="e0e8e-160">Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-160">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="e0e8e-161">ClientKey</span><span class="sxs-lookup"><span data-stu-id="e0e8e-161">ClientKey</span></span> |<span data-ttu-id="e0e8e-162">Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-162">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="e0e8e-163">LogoutURL</span><span class="sxs-lookup"><span data-stu-id="e0e8e-163">LogoutURL</span></span> |<span data-ttu-id="e0e8e-164">Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-164">You need tooget this value from your Benefitsolver support team.</span></span> |
   | <span data-ttu-id="e0e8e-165">EmployeeID</span><span class="sxs-lookup"><span data-stu-id="e0e8e-165">EmployeeID</span></span> |<span data-ttu-id="e0e8e-166">Szükséges tooget ezt az értéket a Benefitsolver támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-166">You need tooget this value from your Benefitsolver support team.</span></span> |
   
   1. <span data-ttu-id="e0e8e-167">Minden egyes sorára adatok hello fenti táblázatban, kattintson a **hozzáadása a felhasználói attribútum**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-167">For each data row in hello table above, click **add user attribute**.</span></span>
   2. <span data-ttu-id="e0e8e-168">A hello **attribútumnév** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-168">In hello **Attribute Name** textbox, type hello attribute name shown for that row.</span></span>
   3. <span data-ttu-id="e0e8e-169">A hello **attribútumérték** szövegmezőben, az adott sorhoz feltüntetett válassza hello attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-169">In hello **Attribute Value** textbox, select hello attribute value shown for that row.</span></span>
   4. <span data-ttu-id="e0e8e-170">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-170">Click **Complete**.</span></span>
9. <span data-ttu-id="e0e8e-171">Kattintson a **módosítások alkalmazásához**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-171">Click **Apply Changes**.</span></span>

## <a name="configure-user-provisioning"></a><span data-ttu-id="e0e8e-172">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e0e8e-172">Configure user provisioning</span></span>
<span data-ttu-id="e0e8e-173">A sorrend tooenable az Azure AD felhasználók toolog Benefitsolver be azok ki kell építenie Benefitsolver be.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-173">In order tooenable Azure AD users toolog into Benefitsolver, they must be provisioned into Benefitsolver.</span></span>  

<span data-ttu-id="e0e8e-174">Benefitsolver hello esetében alkalmazott adata (általában minden éjjel) a HRIS rendszerről nyilvántartásba fájl feltöltve az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-174">In hello case of Benefitsolver, employee data is in your application populated through a Census file from your HRIS system (typically nightly).</span></span>  

>[!NOTE]
><span data-ttu-id="e0e8e-175">Bármely más Benefitsolver felhasználói fiók létrehozása eszközök vagy Benefitsolver tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-175">You can use any other Benefitsolver user account creation tools or APIs provided by Benefitsolver tooprovision AAD user accounts.</span></span> 
> 

## <a name="assigning-users"></a><span data-ttu-id="e0e8e-176">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="e0e8e-176">Assigning users</span></span>
<span data-ttu-id="e0e8e-177">tootest a konfigurációs kell azt szeretné, hogy az alkalmazás hozzáférési tooit használatával hozzárendelésével tooallow toogrant hello Azure AD felhasználók.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-177">tootest your configuration, you need toogrant hello Azure AD users you want tooallow using your application access tooit by assigning them.</span></span>

### <a name="tooassign-users-toobenefitsolver-perform-hello-following-steps"></a><span data-ttu-id="e0e8e-178">tooassign felhasználók tooBenefitsolver, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e0e8e-178">tooassign users tooBenefitsolver, perform hello following steps:</span></span>
1. <span data-ttu-id="e0e8e-179">Hello a klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-179">In hello Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="e0e8e-180">A hello ** Benefitsolver ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-180">On hello **Benefitsolver **application integration page, click **Assign users**.</span></span>
   
   <span data-ttu-id="e0e8e-181">![Felhasználók hozzárendelése](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-181">![Assign Users](./media/active-directory-saas-benefitsolver-tutorial/IC804829.png "Assign Users")</span></span>
3. <span data-ttu-id="e0e8e-182">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** tooconfirm a hozzárendelés.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-182">Select your test user, click **Assign**, and then click **Yes** tooconfirm your assignment.</span></span>
   
   <span data-ttu-id="e0e8e-183">![Igen](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="e0e8e-183">![Yes](./media/active-directory-saas-benefitsolver-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="e0e8e-184">Ha tootest az egyszeri bejelentkezés a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="e0e8e-184">If you want tootest your single sign-on settings, open hello Access Panel.</span></span> <span data-ttu-id="e0e8e-185">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="e0e8e-185">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

