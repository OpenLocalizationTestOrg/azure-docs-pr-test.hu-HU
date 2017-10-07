---
title: "Oktatóanyag: Azure Active Directoryval integrált ServiceNow |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a ServiceNow és a ServiceNow Express között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: df6a07dd1aa437198fbdb9d0a04ea14f3a320249
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a><span data-ttu-id="0d840-103">Oktatóanyag: Azure Active Directoryval integrált ServiceNow</span><span class="sxs-lookup"><span data-stu-id="0d840-103">Tutorial: Azure Active Directory integration with ServiceNow</span></span>
<span data-ttu-id="0d840-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate a ServiceNow és a ServiceNow Express, az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0d840-104">In this tutorial, you learn how toointegrate ServiceNow and ServiceNow Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0d840-105">A ServiceNow és a ServiceNow Express integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-105">Integrating ServiceNow and ServiceNow Express with Azure AD provides you with hello following benefits:</span></span>

* <span data-ttu-id="0d840-106">Megadhatja a hozzáférés tooServiceNow, aki az Azure AD és a ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="0d840-106">You can control in Azure AD who has access tooServiceNow and ServiceNow Express</span></span>
* <span data-ttu-id="0d840-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooServiceNow és ServiceNow Express (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="0d840-107">You can enable your users tooautomatically get signed-on tooServiceNow and ServiceNow Express (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="0d840-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="0d840-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="0d840-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0d840-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0d840-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0d840-110">Prerequisites</span></span>
<span data-ttu-id="0d840-111">az Azure AD-integráció a ServiceNow és a ServiceNow Express tooconfigure, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="0d840-111">tooconfigure Azure AD integration with ServiceNow and ServiceNow Express, you need hello following items:</span></span>

* <span data-ttu-id="0d840-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0d840-112">An Azure AD subscription</span></span>
* <span data-ttu-id="0d840-113">A ServiceNow, egy példány vagy bérlő ServiceNow, Calgary verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="0d840-113">For ServiceNow, an instance or tenant of ServiceNow, Calgary version or higher</span></span>
* <span data-ttu-id="0d840-114">A ServiceNow expressz, ServiceNow kifejezett, Helsinki verzió példányának vagy újabb</span><span class="sxs-lookup"><span data-stu-id="0d840-114">For ServiceNow Express, an instance of ServiceNow Express, Helsinki version or higher</span></span>
* <span data-ttu-id="0d840-115">hello ServiceNow bérlői rendelkeznie kell hello [több szolgáltató egyszeri bejelentkezést a beépülő modul](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="0d840-115">hello ServiceNow tenant must have hello [Multiple Provider Single Sign On Plugin](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) enabled.</span></span> <span data-ttu-id="0d840-116">Ezt úgy teheti [szolgáltatási kérelem elküldése](https://hi.service-now.com).</span><span class="sxs-lookup"><span data-stu-id="0d840-116">This can be done by [submitting a service request](https://hi.service-now.com).</span></span> 

> [!NOTE]
> <span data-ttu-id="0d840-117">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0d840-117">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="0d840-118">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0d840-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="0d840-119">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0d840-119">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="0d840-120">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0d840-120">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0d840-121">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0d840-121">Scenario description</span></span>
<span data-ttu-id="0d840-122">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0d840-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0d840-123">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0d840-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0d840-124">A ServiceNow hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0d840-124">Adding ServiceNow from hello gallery</span></span>
2. <span data-ttu-id="0d840-125">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés ServiceNow vagy ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="0d840-125">Configuring and testing Azure AD single sign-on for ServiceNow or ServiceNow Express</span></span>

## <a name="adding-servicenow-from-hello-gallery"></a><span data-ttu-id="0d840-126">A ServiceNow hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="0d840-126">Adding ServiceNow from hello gallery</span></span>
<span data-ttu-id="0d840-127">tooconfigure hello integrációs ServiceNow vagy ServiceNow Express, az Azure AD-be, meg kell tooadd ServiceNow hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0d840-127">tooconfigure hello integration of ServiceNow or ServiceNow Express into Azure AD, you need tooadd ServiceNow from hello gallery tooyour list of managed SaaS apps.</span></span> 

<span data-ttu-id="0d840-128">**tooadd ServiceNow hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0d840-128">**tooadd ServiceNow from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d840-129">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0d840-129">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="0d840-131">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="0d840-131">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
3. <span data-ttu-id="0d840-132">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="0d840-132">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="0d840-134">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="0d840-134">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="0d840-136">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0d840-136">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="0d840-138">Hello keresési mezőbe, írja be a **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="0d840-138">In hello search box, type **ServiceNow**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. <span data-ttu-id="0d840-140">Hello eredmények ablaktábláján jelöljön ki **ServiceNow**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0d840-140">In hello results pane, select **ServiceNow**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="0d840-142">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0d840-142">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="0d840-143">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés ServiceNow vagy ServiceNow Express "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="0d840-143">In this section, you configure and test Azure AD single sign-on with ServiceNow or ServiceNow Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0d840-144">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a ServiceNow tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0d840-144">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceNow is tooa user in Azure AD.</span></span> <span data-ttu-id="0d840-145">Ez azt jelenti hello kapcsolódó felhasználó a ServiceNow és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0d840-145">In other words, a link relationship between an Azure AD user and hello related user in ServiceNow needs toobe established.</span></span>
<span data-ttu-id="0d840-146">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="0d840-146">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceNow.</span></span> <span data-ttu-id="0d840-147">tooconfigure és az Azure AD egyszeri bejelentkezést a ServiceNow-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0d840-147">tooconfigure and test Azure AD single sign-on with ServiceNow, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0d840-148">**[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0d840-148">**[Configuring Azure AD Single Sign-On for ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0d840-149">**[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0d840-149">**[Configuring Azure AD Single Sign-On for ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - tooenable your users toouse this feature.</span></span>
3. <span data-ttu-id="0d840-150">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0d840-150">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="0d840-151">**[A ServiceNow tesztfelhasználó létrehozása](#creating-a-servicenow-test-user)**  -toohave Britta Simon a ServiceNow, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="0d840-151">**[Creating a ServiceNow test user](#creating-a-servicenow-test-user)** - toohave a counterpart of Britta Simon in ServiceNow that is linked toohello Azure AD representation of her.</span></span>
5. <span data-ttu-id="0d840-152">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0d840-152">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
6. <span data-ttu-id="0d840-153">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0d840-153">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

> [!NOTE]
> <span data-ttu-id="0d840-154">Ha azt szeretné, hogy a ServiceNow tooconfigure hagyja ki ezt a 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="0d840-154">If you want tooconfigure ServiceNow omit step 2.</span></span> <span data-ttu-id="0d840-155">Hasonlóképpen ha azt szeretné, hogy a ServiceNow Express tooconfigure hagyja el az 1. lépés.</span><span class="sxs-lookup"><span data-stu-id="0d840-155">Likewise, if you want tooconfigure ServiceNow Express omit step 1.</span></span>
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a><span data-ttu-id="0d840-156">A ServiceNow az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d840-156">Configuring Azure AD Single Sign-On for ServiceNow</span></span>
1. <span data-ttu-id="0d840-157">A klasszikus portálon hello Azure AD, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel .</span><span class="sxs-lookup"><span data-stu-id="0d840-157">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="0d840-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-158">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="0d840-159">A hello **hogyan szeretné tooServiceNow a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0d840-159">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="0d840-160">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-160">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="0d840-161">A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-161">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="0d840-162">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-162">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="0d840-163">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-163">a.</span></span> <span data-ttu-id="0d840-164">a hello **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="0d840-164">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="0d840-165">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-165">b.</span></span> <span data-ttu-id="0d840-166">A hello **azonosító** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="0d840-166">In hello **Identifier** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="0d840-167">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-167">c.</span></span> <span data-ttu-id="0d840-168">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="0d840-168">Click **Next**</span></span>

4. <span data-ttu-id="0d840-169">az Azure AD toohave automatikusan ServiceNow beállítása az SAML-alapú hitelesítéshez, írja be a ServiceNow példány nevét, a rendszergazda felhasználónevét és a rendszergazdai jelszó hello **automatikus konfigurálása egyszeri bejelentkezéshez** kialakításához, és kattintson a  *Konfigurálása*.</span><span class="sxs-lookup"><span data-stu-id="0d840-169">toohave Azure AD automatically configure ServiceNow for SAML-based authentication, enter your ServiceNow instance name, admin username, and admin password in hello **Auto configure single sign-on** form and click *Configure*.</span></span> <span data-ttu-id="0d840-170">Vegye figyelembe, hogy hello rendszergazdai felhasználónevet kell rendelkeznie a hello **security_admin** a toowork a ServiceNow hozzárendelt szerepkör.</span><span class="sxs-lookup"><span data-stu-id="0d840-170">Note that hello admin username provided must have hello **security_admin** role assigned in ServiceNow for this toowork.</span></span> <span data-ttu-id="0d840-171">Ellenkező esetben toomanually ServiceNow toouse az Azure AD beállítása SAML-Identitásszolgáltatóként, kattintson a **manuálisan állítsa be a hello alkalmazását az egyszeri bejelentkezés**, majd kattintson a **következő** és teljes hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0d840-171">Otherwise, toomanually configure ServiceNow toouse Azure AD as a SAML identity provider, click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="0d840-172">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-172">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="0d840-173">A hello **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen hello tanúsítványfájlt.</span><span class="sxs-lookup"><span data-stu-id="0d840-173">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer.</span></span>
   
    <span data-ttu-id="0d840-174">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-174">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="0d840-175">Bejelentkezés tooyour ServiceNow alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0d840-175">Sign-on tooyour ServiceNow application as an administrator.</span></span>

7. <span data-ttu-id="0d840-176">Hello aktiválása *integrációs - több szolgáltató egyszeri bejelentkezés telepítő* beépülő modul következő hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0d840-176">Activate hello *Integration - Multiple Provider Single Sign-On Installer* plugin by following hello next steps:</span></span>
   
    <span data-ttu-id="0d840-177">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-177">a.</span></span> <span data-ttu-id="0d840-178">A bal oldalon hello hello navigációs ablaktábláján válassza túl**rendszer Definition** szakaszt, és kattintson a **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="0d840-178">In hello navigation pane on hello left side, go too**System Definition** section and then click **Plugins**.</span></span>
   
    <span data-ttu-id="0d840-179">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "beépülő modul aktiválása")</span><span class="sxs-lookup"><span data-stu-id="0d840-179">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")</span></span>
   
    <span data-ttu-id="0d840-180">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-180">b.</span></span> <span data-ttu-id="0d840-181">Keresse meg *integrációs - több szolgáltató egyszeri bejelentkezés telepítő*.</span><span class="sxs-lookup"><span data-stu-id="0d840-181">Search for *Integration - Multiple Provider Single Sign-On Installer*.</span></span>
   
    <span data-ttu-id="0d840-182">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "beépülő modul aktiválása")</span><span class="sxs-lookup"><span data-stu-id="0d840-182">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Activate plugin")</span></span>
   
    <span data-ttu-id="0d840-183">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-183">c.</span></span> <span data-ttu-id="0d840-184">Válassza ki a hello beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="0d840-184">Select hello plugin.</span></span> <span data-ttu-id="0d840-185">Rigth kattintson, és válassza ki **aktiválás/frissítése**.</span><span class="sxs-lookup"><span data-stu-id="0d840-185">Rigth click and select **Activate/Upgrade**.</span></span>
   
    <span data-ttu-id="0d840-186">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-186">d.</span></span> <span data-ttu-id="0d840-187">Kattintson a hello **aktiválás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-187">Click hello **Activate** button.</span></span>

8. <span data-ttu-id="0d840-188">A bal oldalon hello hello navigációs ablaktábláján kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="0d840-188">In hello navigation pane on hello left side, click **Properties**.</span></span>  
   
    <span data-ttu-id="0d840-189">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-189">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Configure app URL")</span></span>

9. <span data-ttu-id="0d840-190">A hello **több SSO tulajdonságokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-190">On hello **Multiple Provider SSO Properties** dialog, perform hello following steps:</span></span>
   
    <span data-ttu-id="0d840-191">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-191">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")</span></span>
   
    <span data-ttu-id="0d840-192">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-192">a.</span></span> <span data-ttu-id="0d840-193">Mint **több szolgáltató SSO engedélyezése**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="0d840-193">As **Enable multiple provider SSO**, select **Yes**.</span></span>
   
    <span data-ttu-id="0d840-194">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-194">b.</span></span> <span data-ttu-id="0d840-195">Mint **kapott hibakeresési naplózás engedélyezése hello több szolgáltató SSO integrációs**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="0d840-195">As **Enable debug logging got hello multiple provider SSO integration**, select **Yes**.</span></span>
   
    <span data-ttu-id="0d840-196">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-196">c.</span></span> <span data-ttu-id="0d840-197">A **hello felhasználói hello mezőjében táblázat...**  szövegmezőhöz típus **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="0d840-197">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
   
    <span data-ttu-id="0d840-198">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-198">d.</span></span> <span data-ttu-id="0d840-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-199">Click **Save**.</span></span>

10. <span data-ttu-id="0d840-200">A bal oldalon hello hello navigációs ablaktábláján kattintson **x509 tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="0d840-200">In hello navigation pane on hello left side, click **x509 Certificates**.</span></span>
    
     <span data-ttu-id="0d840-201">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-201">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Configure single sign-on")</span></span>

11. <span data-ttu-id="0d840-202">A hello **X.509-tanúsítványokat** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="0d840-202">On hello **X.509 Certificates** dialog, click **New**.</span></span>
    
     <span data-ttu-id="0d840-203">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-203">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")</span></span>

12. <span data-ttu-id="0d840-204">A hello **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-204">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="0d840-205">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-205">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
     <span data-ttu-id="0d840-206">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-206">a.</span></span> <span data-ttu-id="0d840-207">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="0d840-207">Click **New**.</span></span>
    
     <span data-ttu-id="0d840-208">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-208">b.</span></span> <span data-ttu-id="0d840-209">A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="0d840-209">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
     <span data-ttu-id="0d840-210">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-210">c.</span></span> <span data-ttu-id="0d840-211">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="0d840-211">Select **Active**.</span></span>
    
     <span data-ttu-id="0d840-212">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-212">d.</span></span> <span data-ttu-id="0d840-213">Mint **formátum**, jelölje be **PEM**.</span><span class="sxs-lookup"><span data-stu-id="0d840-213">As **Format**, select **PEM**.</span></span>
    
     <span data-ttu-id="0d840-214">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-214">e.</span></span> <span data-ttu-id="0d840-215">Mint **típus**, jelölje be **megbízható tároló Cert**.</span><span class="sxs-lookup"><span data-stu-id="0d840-215">As **Type**, select **Trust Store Cert**.</span></span>
    
     <span data-ttu-id="0d840-216">f.</span><span class="sxs-lookup"><span data-stu-id="0d840-216">f.</span></span> <span data-ttu-id="0d840-217">Nyissa meg a Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **PEM tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-217">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
     <span data-ttu-id="0d840-218">g.</span><span class="sxs-lookup"><span data-stu-id="0d840-218">g.</span></span> <span data-ttu-id="0d840-219">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="0d840-219">Click **Update**.</span></span>

13. <span data-ttu-id="0d840-220">A bal oldalon hello hello navigációs ablaktábláján kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="0d840-220">In hello navigation pane on hello left side, click **Identity Providers**.</span></span>
    
     <span data-ttu-id="0d840-221">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-221">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Configure single sign-on")</span></span>

14. <span data-ttu-id="0d840-222">A hello **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **új**:</span><span class="sxs-lookup"><span data-stu-id="0d840-222">On hello **Identity Providers** dialog, click **New**:</span></span>
    
     <span data-ttu-id="0d840-223">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-223">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")</span></span>

15. <span data-ttu-id="0d840-224">A hello **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **egy SAML2 1. frissítés?**:</span><span class="sxs-lookup"><span data-stu-id="0d840-224">On hello **Identity Providers** dialog, click **SAML2 Update1?**:</span></span>
    
     <span data-ttu-id="0d840-225">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-225">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")</span></span>

16. <span data-ttu-id="0d840-226">Hello egy SAML2 1. frissítés tulajdonságai párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-226">On hello SAML2 Update1 Properties dialog, perform hello following steps:</span></span>
    
     <span data-ttu-id="0d840-227">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-227">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")</span></span>

    <span data-ttu-id="0d840-228">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-228">a.</span></span> <span data-ttu-id="0d840-229">a hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="0d840-229">in hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="0d840-230">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-230">b.</span></span> <span data-ttu-id="0d840-231">A hello **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, hogy mely mezővel toouniquely azonosítsa azokat a felhasználókat a ServiceNow központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="0d840-231">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="0d840-232">E-mail cím hello hello SAML-jogkivonat szereplő egyedi azonosítóra hello által toohello állapotra vált, vagy az Azure AD configue tooemit vagy hello Azure AD felhasználói azonosító (egyszerű felhasználónév) is **ServiceNow > attribútumok > egyszeri bejelentkezés** szakasza klasszikus Azure portál és a leképezés szükséges hello mező toohello hello **nameidentifier** attribútum.</span><span class="sxs-lookup"><span data-stu-id="0d840-232">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="0d840-233">az Azure AD (pl. egyszerű felhasználónév) hello kiválasztott attribútumnak tárolt hello értékének meg kell felelnie a ServiceNow hello megadott mező (pl. felhasználónév) tárolt hello érték</span><span class="sxs-lookup"><span data-stu-id="0d840-233">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>

    <span data-ttu-id="0d840-234">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-234">c.</span></span> <span data-ttu-id="0d840-235">A klasszikus portálon hello Azure AD, másolja a hello **identitás Szolgáltatóazonosító** értékét, és illessze be hello **identitási szolgáltató URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-235">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="0d840-236">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-236">d.</span></span> <span data-ttu-id="0d840-237">A klasszikus portálon hello Azure AD, másolja a hello **hitelesítési kérelem URL-cím** értékét, és illessze be hello **identitásszolgáltató AuthnRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-237">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="0d840-238">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-238">e.</span></span> <span data-ttu-id="0d840-239">A klasszikus portálon hello Azure AD, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **identitásszolgáltató SingleLogoutRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-239">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="0d840-240">f.</span><span class="sxs-lookup"><span data-stu-id="0d840-240">f.</span></span> <span data-ttu-id="0d840-241">A hello **ServiceNow kezdőlap** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow példány kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="0d840-241">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="0d840-242">hello ServiceNow példány kezdőlap összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.:`https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="0d840-242">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.:`https://fabrikam.service-now.com/navpage.do`).</span></span>

    <span data-ttu-id="0d840-243">g.</span><span class="sxs-lookup"><span data-stu-id="0d840-243">g.</span></span> <span data-ttu-id="0d840-244">A hello **Entitásazonosító / kibocsátó** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.</span><span class="sxs-lookup"><span data-stu-id="0d840-244">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>

    <span data-ttu-id="0d840-245">h.</span><span class="sxs-lookup"><span data-stu-id="0d840-245">h.</span></span> <span data-ttu-id="0d840-246">A hello **célközönség URL-cím** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.</span><span class="sxs-lookup"><span data-stu-id="0d840-246">In hello **Audience URL** textbox, type hello URL of your ServiceNow tenant.</span></span> 

    <span data-ttu-id="0d840-247">i.</span><span class="sxs-lookup"><span data-stu-id="0d840-247">i.</span></span> <span data-ttu-id="0d840-248">A hello **protokoll kötése hello IDP tartozó SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.</span><span class="sxs-lookup"><span data-stu-id="0d840-248">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>

    <span data-ttu-id="0d840-249">j.</span><span class="sxs-lookup"><span data-stu-id="0d840-249">j.</span></span> <span data-ttu-id="0d840-250">Írja be a hello NameID házirend szövegmező, **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.</span><span class="sxs-lookup"><span data-stu-id="0d840-250">In hello NameID Policy textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>

    <span data-ttu-id="0d840-251">k.</span><span class="sxs-lookup"><span data-stu-id="0d840-251">k.</span></span> <span data-ttu-id="0d840-252">Kapcsolja ki **hozzon létre egy AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="0d840-252">Deselect **Create an AuthnContextClass**.</span></span>

    <span data-ttu-id="0d840-253">l.</span><span class="sxs-lookup"><span data-stu-id="0d840-253">l.</span></span> <span data-ttu-id="0d840-254">A hello **AuthnContextClassRef metódus**, típus `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span><span class="sxs-lookup"><span data-stu-id="0d840-254">In hello **AuthnContextClassRef Method**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span></span> <span data-ttu-id="0d840-255">Ez csak akkor szükséges, ha egyetlen szervezet felhőbeli.</span><span class="sxs-lookup"><span data-stu-id="0d840-255">This is only needed if you are cloud only organization.</span></span> <span data-ttu-id="0d840-256">Használata a helyszíni AD FS vagy MFA hitelesítés akkor ne konfigurálja ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="0d840-256">If you are using on-premises ADFS or MFA for authentication then you should not configure this value.</span></span> 

    <span data-ttu-id="0d840-257">m.</span><span class="sxs-lookup"><span data-stu-id="0d840-257">m.</span></span> <span data-ttu-id="0d840-258">A **óra döntés** szövegmezőhöz típus **60**.</span><span class="sxs-lookup"><span data-stu-id="0d840-258">In **Clock Skew** textbox, type **60**.</span></span>

    <span data-ttu-id="0d840-259">n.</span><span class="sxs-lookup"><span data-stu-id="0d840-259">n.</span></span> <span data-ttu-id="0d840-260">Mint **egyszeri bejelentkezést a parancsfájl**, jelölje be **MultiSSO_SAML2_Update1**.</span><span class="sxs-lookup"><span data-stu-id="0d840-260">As **Single Sign On Script**, select **MultiSSO_SAML2_Update1**.</span></span>

    <span data-ttu-id="0d840-261">o.</span><span class="sxs-lookup"><span data-stu-id="0d840-261">o.</span></span> <span data-ttu-id="0d840-262">Mint **x509 tanúsítvány**, jelölje be hello előző lépésben létrehozott hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="0d840-262">As **x509 Certificate**, select hello certificate you have created in hello previous step.</span></span>

    <span data-ttu-id="0d840-263">p.</span><span class="sxs-lookup"><span data-stu-id="0d840-263">p.</span></span> <span data-ttu-id="0d840-264">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="0d840-264">Click **Submit**.</span></span> 

1. <span data-ttu-id="0d840-265">Hello Azure AD-klasszikus portál hello egyszeri bejelentkezés konfigurációs megerősítő válassza ki, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0d840-265">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="0d840-266">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-266">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="0d840-267">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="0d840-267">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="0d840-268">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-268">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a><span data-ttu-id="0d840-269">A ServiceNow expressz az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0d840-269">Configuring Azure AD Single Sign-On for ServiceNow Express</span></span>
1. <span data-ttu-id="0d840-270">A klasszikus portálon hello Azure AD, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezés** párbeszédpanel .</span><span class="sxs-lookup"><span data-stu-id="0d840-270">In hello Azure AD classic portal, on hello **ServiceNow** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="0d840-271">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-271">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="0d840-272">A hello **hogyan szeretné tooServiceNow a felhasználók toosign** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="0d840-272">On hello **How would you like users toosign on tooServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="0d840-273">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-273">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="0d840-274">A hello **Alkalmazásbeállítások konfigurálása** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-274">On hello **Configure App Settings** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="0d840-275">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-275">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="0d840-276">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-276">a.</span></span> <span data-ttu-id="0d840-277">a hello **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-cím a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő használja: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="0d840-277">in hello **ServiceNow Sign On URL** textbox, type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="0d840-278">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-278">b.</span></span> <span data-ttu-id="0d840-279">A hello **kiállítójának URL-címe** szövegmező, írja be az URL-cím használják-e a felhasználók toosign tooyour ServiceNow alkalmazás hello mintát a következő `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="0d840-279">In hello **Issuer URL** textbox,type your URL used by your users toosign-on tooyour ServiceNow application following hello pattern `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="0d840-280">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-280">c.</span></span> <span data-ttu-id="0d840-281">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="0d840-281">Click **Next**</span></span>

4. <span data-ttu-id="0d840-282">Kattintson a **manuálisan állítsa be a hello alkalmazását az egyszeri bejelentkezés**, kattintson a **következő** és teljes hello a következő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="0d840-282">Click **Manually configure hello application for single sign-on**, then click **Next** and complete hello following steps.</span></span>
   
    <span data-ttu-id="0d840-283">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-283">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="0d840-284">A hello **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen hello tanúsítvány fájlt, és kattintson **tovább**.</span><span class="sxs-lookup"><span data-stu-id="0d840-284">On hello **Configure single sign-on at ServiceNow** page, click **Download certificate**, save hello certificate file locally on your computer, and then click **Next**.</span></span>
   
    <span data-ttu-id="0d840-285">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-285">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="0d840-286">Bejelentkezés tooyour ServiceNow Express alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="0d840-286">Sign-on tooyour ServiceNow Express application as an administrator.</span></span>

7. <span data-ttu-id="0d840-287">A bal oldalon hello hello navigációs ablaktábláján kattintson **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0d840-287">In hello navigation pane on hello left side, click **Single Sign-On**.</span></span>  
   
    <span data-ttu-id="0d840-288">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-288">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Configure app URL")</span></span>

8. <span data-ttu-id="0d840-289">A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson hello konfigurációs ikonjára hello felső, jobb és beállított hello következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="0d840-289">On hello **Single Sign-On** dialog, click hello configuration icon on hello upper right and set hello following properties:</span></span>
   
    <span data-ttu-id="0d840-290">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-290">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Configure app URL")</span></span>
   
    <span data-ttu-id="0d840-291">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-291">a.</span></span> <span data-ttu-id="0d840-292">Váltás **több szolgáltató SSO engedélyezése** toohello jobbra.</span><span class="sxs-lookup"><span data-stu-id="0d840-292">Toggle **Enable multiple provider SSO** toohello right.</span></span>
   
    <span data-ttu-id="0d840-293">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-293">b.</span></span> <span data-ttu-id="0d840-294">Váltás **naplózása hello több szolgáltató SSO-integráció engedélyezése hibakeresési** toohello jobbra.</span><span class="sxs-lookup"><span data-stu-id="0d840-294">Toggle **Enable debug logging for hello multiple provider SSO integration** toohello right.</span></span>
   
    <span data-ttu-id="0d840-295">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-295">c.</span></span> <span data-ttu-id="0d840-296">A **hello felhasználói hello mezőjében táblázat...**  szövegmezőhöz típus **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="0d840-296">In **hello field on hello user table that...** textbox, type **user_name**.</span></span>
9. <span data-ttu-id="0d840-297">A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson a **új tanúsítvány hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0d840-297">On hello **Single Sign-On** dialog, click **Add New Certificate**.</span></span>
   
    <span data-ttu-id="0d840-298">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-298">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Configure single sign-on")</span></span>
10. <span data-ttu-id="0d840-299">A hello **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-299">On hello **X.509 Certificates** dialog, perform hello following steps:</span></span>
    
    <span data-ttu-id="0d840-300">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-300">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
    <span data-ttu-id="0d840-301">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-301">a.</span></span> <span data-ttu-id="0d840-302">A hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="0d840-302">In hello **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
    <span data-ttu-id="0d840-303">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-303">b.</span></span> <span data-ttu-id="0d840-304">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="0d840-304">Select **Active**.</span></span>
    
    <span data-ttu-id="0d840-305">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-305">c.</span></span> <span data-ttu-id="0d840-306">Mint **formátum**, jelölje be **PEM**.</span><span class="sxs-lookup"><span data-stu-id="0d840-306">As **Format**, select **PEM**.</span></span>
    
    <span data-ttu-id="0d840-307">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-307">d.</span></span> <span data-ttu-id="0d840-308">Mint **típus**, jelölje be **megbízható tároló Cert**.</span><span class="sxs-lookup"><span data-stu-id="0d840-308">As **Type**, select **Trust Store Cert**.</span></span>
    
    <span data-ttu-id="0d840-309">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-309">e.</span></span> <span data-ttu-id="0d840-310">Hozzon létre egy Base64-kódolású fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="0d840-310">Create a Base64 encoded file from your downloaded certificate.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0d840-311">További részletekért lásd: [hogyan tooconvert bináris tanúsítvány egy szövegfájlba](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="0d840-311">For more details, see [How tooconvert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>
    > 
    > 
    
    <span data-ttu-id="0d840-312">f.</span><span class="sxs-lookup"><span data-stu-id="0d840-312">f.</span></span> <span data-ttu-id="0d840-313">Nyissa meg a Base64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **PEM tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-313">Open your Base64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **PEM Certificate** textbox.</span></span>
    
    <span data-ttu-id="0d840-314">g.</span><span class="sxs-lookup"><span data-stu-id="0d840-314">g.</span></span> <span data-ttu-id="0d840-315">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="0d840-315">Click **Update**.</span></span>
11. <span data-ttu-id="0d840-316">A hello **egyszeri bejelentkezés** párbeszédpanel, kattintson a **hozzáadása új IdP**.</span><span class="sxs-lookup"><span data-stu-id="0d840-316">On hello **Single Sign-On** dialog, click **Add New IdP**.</span></span>
    
    <span data-ttu-id="0d840-317">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-317">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Configure single sign-on")</span></span>
12. <span data-ttu-id="0d840-318">A hello **hozzáadása új identitásszolgáltató** párbeszédpanelen, a **identitásszolgáltató konfigurálása**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-318">On hello **Add New Identity Provider** dialog, under **Configure Identity Provider**, perform hello following steps:</span></span>
    
    <span data-ttu-id="0d840-319">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-319">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Configure single sign-on")</span></span>

    <span data-ttu-id="0d840-320">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-320">a.</span></span> <span data-ttu-id="0d840-321">a hello **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="0d840-321">In hello **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="0d840-322">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-322">b.</span></span> <span data-ttu-id="0d840-323">A klasszikus portálon hello Azure AD, másolja a hello **identitás Szolgáltatóazonosító** értékét, és illessze be hello **identitási szolgáltató URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-323">In hello Azure AD classic portal, copy hello **Identity Provider ID** value, and then paste it into hello **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="0d840-324">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-324">c.</span></span> <span data-ttu-id="0d840-325">A klasszikus portálon hello Azure AD, másolja a hello **hitelesítési kérelem URL-cím** értékét, és illessze be hello **identitásszolgáltató AuthnRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-325">In hello Azure AD classic portal, copy hello **Authentication Request URL** value, and then paste it into hello **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="0d840-326">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-326">d.</span></span> <span data-ttu-id="0d840-327">A klasszikus portálon hello Azure AD, másolja a hello **egyetlen Sign-Out URL-címe** értékét, és illessze be hello **identitásszolgáltató SingleLogoutRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="0d840-327">In hello Azure AD classic portal, copy hello **Single Sign-Out Service URL** value, and then paste it into hello **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="0d840-328">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-328">e.</span></span> <span data-ttu-id="0d840-329">Mint **szolgáltató Identitástanúsítvány**, jelölje be hello előző lépésben létrehozott hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="0d840-329">As **Identity Provider Certificate**, select hello certificate you have created in hello previous step.</span></span>


1. <span data-ttu-id="0d840-330">Kattintson a **speciális beállítások**, majd a **további identitás szolgáltató tulajdonságai**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-330">Click **Advanced Settings**, and under **Additional Identity Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="0d840-331">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-331">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="0d840-332">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-332">a.</span></span> <span data-ttu-id="0d840-333">A hello **protokoll kötése hello IDP tartozó SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.</span><span class="sxs-lookup"><span data-stu-id="0d840-333">In hello **Protocol Binding for hello IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>
   
    <span data-ttu-id="0d840-334">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-334">b.</span></span> <span data-ttu-id="0d840-335">A hello **NameID házirend** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.</span><span class="sxs-lookup"><span data-stu-id="0d840-335">In hello **NameID Policy** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>    
   
    <span data-ttu-id="0d840-336">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-336">c.</span></span> <span data-ttu-id="0d840-337">A hello **AuthnContextClassRef metódus**, típus **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span><span class="sxs-lookup"><span data-stu-id="0d840-337">In hello **AuthnContextClassRef Method**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span></span>
   
    <span data-ttu-id="0d840-338">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-338">d.</span></span> <span data-ttu-id="0d840-339">Kapcsolja ki **hozzon létre egy AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="0d840-339">Deselect **Create an AuthnContextClass**.</span></span>

2. <span data-ttu-id="0d840-340">A **további szolgáltató tulajdonságai**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-340">Under **Additional Service Provider Properties**, perform hello following steps:</span></span>
   
    <span data-ttu-id="0d840-341">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-341">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="0d840-342">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-342">a.</span></span> <span data-ttu-id="0d840-343">A hello **ServiceNow kezdőlap** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow példány kezdőlapján.</span><span class="sxs-lookup"><span data-stu-id="0d840-343">In hello **ServiceNow Homepage** textbox, type hello URL of your ServiceNow instance homepage.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="0d840-344">hello ServiceNow példány kezdőlap összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.: `https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="0d840-344">hello ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.: `https://fabrikam.service-now.com/navpage.do`).</span></span>
    > 
    > 
   
    <span data-ttu-id="0d840-345">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-345">b.</span></span> <span data-ttu-id="0d840-346">A hello **Entitásazonosító / kibocsátó** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.</span><span class="sxs-lookup"><span data-stu-id="0d840-346">In hello **Entity ID / Issuer** textbox, type hello URL of your ServiceNow tenant.</span></span>
   
    <span data-ttu-id="0d840-347">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-347">c.</span></span> <span data-ttu-id="0d840-348">A hello **célközönség URI** szövegmezőhöz típus hello URL-CÍMÉT a ServiceNow-bérlő.</span><span class="sxs-lookup"><span data-stu-id="0d840-348">In hello **Audience URI** textbox, type hello URL of your ServiceNow tenant.</span></span> 
   
    <span data-ttu-id="0d840-349">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-349">d.</span></span> <span data-ttu-id="0d840-350">A **óra döntés** szövegmezőhöz típus **60**.</span><span class="sxs-lookup"><span data-stu-id="0d840-350">In **Clock Skew** textbox, type **60**.</span></span>
   
    <span data-ttu-id="0d840-351">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-351">e.</span></span> <span data-ttu-id="0d840-352">A hello **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, hogy mely mezővel toouniquely azonosítsa azokat a felhasználókat a ServiceNow központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="0d840-352">In hello **User Field** textbox, type **email** or **user_name**, depending on which field is used toouniquely identify users in your ServiceNow deployment.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="0d840-353">E-mail cím hello hello SAML-jogkivonat szereplő egyedi azonosítóra hello által toohello állapotra vált, vagy az Azure AD configue tooemit vagy hello Azure AD felhasználói azonosító (egyszerű felhasználónév) is **ServiceNow > attribútumok > egyszeri bejelentkezés** szakasza klasszikus Azure portál és a leképezés szükséges hello mező toohello hello **nameidentifier** attribútum.</span><span class="sxs-lookup"><span data-stu-id="0d840-353">You can configue Azure AD tooemit either hello Azure AD user ID (user principal name) or hello email address as hello unique identifier in hello SAML token by going toohello **ServiceNow > Attributes > Single Sign-On** section of hello Azure classic portal and mapping hello desired field toohello **nameidentifier** attribute.</span></span> <span data-ttu-id="0d840-354">az Azure AD (pl. egyszerű felhasználónév) hello kiválasztott attribútumnak tárolt hello értékének meg kell felelnie a ServiceNow hello megadott mező (pl. felhasználónév) tárolt hello érték</span><span class="sxs-lookup"><span data-stu-id="0d840-354">hello value stored for hello selected attribute in Azure AD (e.g. user principal name) must match hello value stored in ServiceNow for hello entered field (e.g. user_name)</span></span>
    > 
    > 
   
    <span data-ttu-id="0d840-355">f.</span><span class="sxs-lookup"><span data-stu-id="0d840-355">f.</span></span> <span data-ttu-id="0d840-356">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-356">Click **Save**.</span></span> 

3. <span data-ttu-id="0d840-357">Hello Azure AD-klasszikus portál hello egyszeri bejelentkezés konfigurációs megerősítő válassza ki, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="0d840-357">On hello Azure AD classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="0d840-358">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-358">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

4. <span data-ttu-id="0d840-359">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="0d840-359">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="0d840-360">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="0d840-360">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="0d840-361">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="0d840-361">Configuring user provisioning</span></span>
<span data-ttu-id="0d840-362">hello ebben a szakaszban célja toooutline hogyan tooServiceNow tooenable a felhasználók átadása az Active Directory felhasználói fiókok.</span><span class="sxs-lookup"><span data-stu-id="0d840-362">hello objective of this section is toooutline how tooenable user provisioning of Active Directory user accounts tooServiceNow.</span></span>

### <a name="tooconfigure-user-provisioning-perform-hello-following-steps"></a><span data-ttu-id="0d840-363">tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-363">tooconfigure user provisioning, perform hello following steps:</span></span>
1. <span data-ttu-id="0d840-364">Hello Azure kezelési klasszikus portál, a hello **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálja, a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="0d840-364">In hello Azure Management classic portal, on hello **ServiceNow** application integration page, click **Configure user provisioning**.</span></span> 
   
    <span data-ttu-id="0d840-365">![A felhasználók átadása](./media/active-directory-saas-servicenow-tutorial/IC769498.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="0d840-365">![User provisioning](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")</span></span>

2. <span data-ttu-id="0d840-366">A hello **adja meg a ServiceNow hitelesítő adatok tooenable automatikus felhasználólétesítés** lapján adja meg a következő konfigurációs beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-366">On hello **Enter your ServiceNow credentials tooenable automatic user provisioning** page, provide hello following configuration settings:</span></span>
   
     <span data-ttu-id="0d840-367">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-367">a.</span></span> <span data-ttu-id="0d840-368">A hello **ServiceNow példánynév** szövegmezőhöz hello ServiceNow példány neve.</span><span class="sxs-lookup"><span data-stu-id="0d840-368">In hello **ServiceNow Instance Name** textbox, type hello ServiceNow instance name.</span></span>
   
     <span data-ttu-id="0d840-369">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-369">b.</span></span> <span data-ttu-id="0d840-370">A hello **ServiceNow rendszergazda felhasználóneve** szövegmezőhöz hello ServiceNow rendszergazdai fiók hello nevét.</span><span class="sxs-lookup"><span data-stu-id="0d840-370">In hello **ServiceNow Admin User Name** textbox, type hello name of hello ServiceNow admin account.</span></span>
   
     <span data-ttu-id="0d840-371">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-371">c.</span></span> <span data-ttu-id="0d840-372">A hello **ServiceNow rendszergazdai jelszó** szövegmezőhöz típus hello fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="0d840-372">In hello **ServiceNow Admin Password** textbox, type hello password for this account.</span></span>
   
     <span data-ttu-id="0d840-373">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-373">d.</span></span> <span data-ttu-id="0d840-374">Kattintson a **érvényesítése** tooverify a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0d840-374">Click **validate** tooverify your configuration.</span></span>
   
     <span data-ttu-id="0d840-375">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-375">e.</span></span> <span data-ttu-id="0d840-376">Kattintson a hello **következő** gomb tooopen hello **további lépések** lap.</span><span class="sxs-lookup"><span data-stu-id="0d840-376">Click hello **Next** button tooopen hello **Next steps** page.</span></span>
   
     <span data-ttu-id="0d840-377">f.</span><span class="sxs-lookup"><span data-stu-id="0d840-377">f.</span></span> <span data-ttu-id="0d840-378">Ha tooprovision felhasználók toothis összes alkalmazást, jelölje be "**hello directory toothis alkalmazás az összes felhasználói fiók automatikusan létesítsen**".</span><span class="sxs-lookup"><span data-stu-id="0d840-378">If you want tooprovision all users toothis application, select “**Automatically provision all user accounts in hello directory toothis application**”.</span></span> 
   
    <span data-ttu-id="0d840-379">![További lépések](./media/active-directory-saas-servicenow-tutorial/IC698804.png "további lépések")</span><span class="sxs-lookup"><span data-stu-id="0d840-379">![Next Steps](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")</span></span>
   
     <span data-ttu-id="0d840-380">g.</span><span class="sxs-lookup"><span data-stu-id="0d840-380">g.</span></span> <span data-ttu-id="0d840-381">A hello **további lépések** kattintson **Complete** toosave a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="0d840-381">On hello **Next steps** page, click **Complete** toosave your configuration.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="0d840-382">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d840-382">Creating an Azure AD test user</span></span>
<span data-ttu-id="0d840-383">Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.</span><span class="sxs-lookup"><span data-stu-id="0d840-383">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="0d840-385">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0d840-385">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d840-386">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0d840-386">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="0d840-388">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="0d840-388">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="0d840-389">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="0d840-389">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="0d840-391">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="0d840-391">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="0d840-393">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-393">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="0d840-395">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-395">a.</span></span> <span data-ttu-id="0d840-396">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="0d840-396">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="0d840-397">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-397">b.</span></span> <span data-ttu-id="0d840-398">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0d840-398">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="0d840-399">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-399">c.</span></span> <span data-ttu-id="0d840-400">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-400">Click **Next**.</span></span>

6. <span data-ttu-id="0d840-401">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-401">On hello **User Profile** dialog page, perform hello following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   <span data-ttu-id="0d840-403">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-403">a.</span></span> <span data-ttu-id="0d840-404">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="0d840-404">In hello **First Name** textbox, type **Britta**.</span></span>  
   
   <span data-ttu-id="0d840-405">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-405">b.</span></span> <span data-ttu-id="0d840-406">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d840-406">In hello **Last Name** textbox, type, **Simon**.</span></span>
   
   <span data-ttu-id="0d840-407">c.</span><span class="sxs-lookup"><span data-stu-id="0d840-407">c.</span></span> <span data-ttu-id="0d840-408">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d840-408">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="0d840-409">d.</span><span class="sxs-lookup"><span data-stu-id="0d840-409">d.</span></span> <span data-ttu-id="0d840-410">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="0d840-410">In hello **Role** list, select **User**.</span></span>
   
   <span data-ttu-id="0d840-411">e.</span><span class="sxs-lookup"><span data-stu-id="0d840-411">e.</span></span> <span data-ttu-id="0d840-412">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-412">Click **Next**.</span></span>

7. <span data-ttu-id="0d840-413">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="0d840-413">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="0d840-415">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0d840-415">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="0d840-417">a.</span><span class="sxs-lookup"><span data-stu-id="0d840-417">a.</span></span> <span data-ttu-id="0d840-418">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="0d840-418">Write down hello value of hello **New Password**.</span></span>
   
    <span data-ttu-id="0d840-419">b.</span><span class="sxs-lookup"><span data-stu-id="0d840-419">b.</span></span> <span data-ttu-id="0d840-420">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="0d840-420">Click **Complete**.</span></span>   

### <a name="creating-a-servicenow-test-user"></a><span data-ttu-id="0d840-421">A ServiceNow tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0d840-421">Creating a ServiceNow test user</span></span>
<span data-ttu-id="0d840-422">Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0d840-422">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="0d840-423">Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="0d840-423">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="0d840-424">Ha nem tudja, hogyan tooadd a ServiceNow vagy ServiceNow Express egy felhasználói fiók, lépjen kapcsolatba a terméktámogatással a ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="0d840-424">If you don't know how tooadd a user in your ServiceNow or ServiceNow Express account, contact ServiceNow support team.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="0d840-425">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="0d840-425">Assigning hello Azure AD test user</span></span>
<span data-ttu-id="0d840-426">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooServiceNow megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0d840-426">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceNow.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="0d840-428">**tooassign Britta Simon tooServiceNow, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="0d840-428">**tooassign Britta Simon tooServiceNow, perform hello following steps:**</span></span>

1. <span data-ttu-id="0d840-429">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="0d840-429">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0d840-431">Hello alkalmazások listában válassza ki a **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="0d840-431">In hello applications list, select **ServiceNow**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. <span data-ttu-id="0d840-433">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="0d840-433">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 

4. <span data-ttu-id="0d840-435">Hello minden felhasználó listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="0d840-435">In hello All Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="0d840-436">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="0d840-436">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="0d840-438">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0d840-438">Testing single sign-on</span></span>
<span data-ttu-id="0d840-439">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="0d840-439">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0d840-440">Hello ServiceNow csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ServiceNow alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="0d840-440">When you click hello ServiceNow tile in hello Access Panel, you should get automatically signed-on tooyour ServiceNow application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0d840-441">További források</span><span class="sxs-lookup"><span data-stu-id="0d840-441">Additional resources</span></span>
* [<span data-ttu-id="0d840-442">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0d840-442">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0d840-443">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0d840-443">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
