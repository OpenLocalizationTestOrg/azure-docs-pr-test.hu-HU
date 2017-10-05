---
title: "Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos |} Microsoft Docs"
description: "TOPdesk - használata az Azure Active Directoryval engedélyezése egyszeri bejelentkezéshez, automatizált üzembe helyezést és további biztonságos!."
services: active-directory
author: jeevansd
documentationcenter: na
manager: femila
ms.assetid: 8e149d2d-7849-48ec-9993-31f4ade5fdb4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/22/2017
ms.author: jeedes
ms.openlocfilehash: 28f0542dbe87bb34c83a7852db7c3a9fef055ce9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---secure"></a><span data-ttu-id="51544-103">Oktatóanyag: Azure Active Directoryval integrált TOPdesk - biztonságos</span><span class="sxs-lookup"><span data-stu-id="51544-103">Tutorial: Azure Active Directory integration with TOPdesk - Secure</span></span>
<span data-ttu-id="51544-104">Ez az oktatóanyag célja az Azure és TOPdesk - biztonságos integrálását megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="51544-104">The objective of this tutorial is to show the integration of Azure and TOPdesk - Secure.</span></span>  
<span data-ttu-id="51544-105">Ebben az oktatóanyagban leírt forgatókönyv feltételezi, hogy már rendelkezik a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="51544-105">The scenario outlined in this tutorial assumes that you already have the following items:</span></span>

* <span data-ttu-id="51544-106">Egy érvényes Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="51544-106">A valid Azure subscription</span></span>
* <span data-ttu-id="51544-107">A TOPdesk - biztonságos egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="51544-107">A TOPdesk - Secure single sign-on enabled subscription</span></span>

<span data-ttu-id="51544-108">Ez az oktatóanyag befejezése után az Azure AD-felhasználók TOPdesk - biztonságos rendelt fog tudni egyetlen jelentkezzen be az alkalmazást a TOPdesk - biztonságos vállalati helyhez (szolgáltatás szolgáltató által kezdeményezett bejelentkezés), vagy a [a hozzáférés bemutatása Panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="51544-108">After completing this tutorial, the Azure AD users you have assigned to TOPdesk - Secure will be able to single sign into the application at your TOPdesk - Secure company site (service provider initiated sign on), or using the [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

<span data-ttu-id="51544-109">Ebben az oktatóanyagban leírt forgatókönyv az alábbi építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="51544-109">The scenario outlined in this tutorial consists of the following building blocks:</span></span>

1. <span data-ttu-id="51544-110">A TOPdesk - biztonságos alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="51544-110">Enabling the application integration for TOPdesk - Secure</span></span>
2. <span data-ttu-id="51544-111">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="51544-111">Configuring single sign-on</span></span>
3. <span data-ttu-id="51544-112">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="51544-112">Configuring user provisioning</span></span>
4. <span data-ttu-id="51544-113">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51544-113">Assigning users</span></span>

<span data-ttu-id="51544-114">![A forgatókönyv](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "forgatókönyv")</span><span class="sxs-lookup"><span data-stu-id="51544-114">![Scenario](./media/active-directory-saas-topdesk-secure-tutorial/IC790596.png "Scenario")</span></span>

## <a name="enabling-the-application-integration-for-topdesk---secure"></a><span data-ttu-id="51544-115">A TOPdesk - biztonságos alkalmazás-integráció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="51544-115">Enabling the application integration for TOPdesk - Secure</span></span>
<span data-ttu-id="51544-116">Ez a szakasz célja a TOPdesk - biztonságos alkalmazás-integráció engedélyezése felvázoló.</span><span class="sxs-lookup"><span data-stu-id="51544-116">The objective of this section is to outline how to enable the application integration for TOPdesk - Secure.</span></span>

### <a name="to-enable-the-application-integration-for-topdesk---secure-perform-the-following-steps"></a><span data-ttu-id="51544-117">Ahhoz, hogy az alkalmazás-integráció TOPdesk - biztonságos, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-117">To enable the application integration for TOPdesk - Secure, perform the following steps:</span></span>
1. <span data-ttu-id="51544-118">A klasszikus Azure portálon, a bal oldali navigációs panelen kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="51544-118">In the Azure classic portal, on the left navigation pane, click **Active Directory**.</span></span>
   
    <span data-ttu-id="51544-119">![Az Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span><span class="sxs-lookup"><span data-stu-id="51544-119">![Active Directory](./media/active-directory-saas-topdesk-secure-tutorial/IC700993.png "Active Directory")</span></span>

2. <span data-ttu-id="51544-120">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="51544-120">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="51544-121">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="51544-121">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    <span data-ttu-id="51544-122">![Alkalmazások](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="51544-122">![Applications](./media/active-directory-saas-topdesk-secure-tutorial/IC700994.png "Applications")</span></span>

4. <span data-ttu-id="51544-123">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="51544-123">Click **Add** at the bottom of the page.</span></span>
   
    <span data-ttu-id="51544-124">![Alkalmazás hozzáadása](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="51544-124">![Add application](./media/active-directory-saas-topdesk-secure-tutorial/IC749321.png "Add application")</span></span>

5. <span data-ttu-id="51544-125">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="51544-125">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    <span data-ttu-id="51544-126">![Alkalmazás hozzáadása a gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "gallerry az alkalmazás hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="51544-126">![Add an application from gallerry](./media/active-directory-saas-topdesk-secure-tutorial/IC749322.png "Add an application from gallerry")</span></span>

6. <span data-ttu-id="51544-127">Az a **keresőmezőbe**, típus **TOPdesk - biztonságos**.</span><span class="sxs-lookup"><span data-stu-id="51544-127">In the **search box**, type **TOPdesk - Secure**.</span></span>
   
    <span data-ttu-id="51544-128">![Alkalmazáskatalógusában](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Alkalmazáskatalógusában")</span><span class="sxs-lookup"><span data-stu-id="51544-128">![Application Gallery](./media/active-directory-saas-topdesk-secure-tutorial/IC790597.png "Application Gallery")</span></span>

7. <span data-ttu-id="51544-129">Az eredmények ablaktáblájában válassza **TOPdesk - biztonságos**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="51544-129">In the results pane, select **TOPdesk - Secure**, and then click **Complete** to add the application.</span></span>
   
    <span data-ttu-id="51544-130">![TOPdesk - biztonságos](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - biztonságos")</span><span class="sxs-lookup"><span data-stu-id="51544-130">![TOPdesk - Secure](./media/active-directory-saas-topdesk-secure-tutorial/IC791933.png "TOPdesk - Secure")</span></span>

## <a name="configuring-single-sign-on"></a><span data-ttu-id="51544-131">Egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="51544-131">Configuring single sign-on</span></span>
<span data-ttu-id="51544-132">Ez a szakasz célja engedélyezése a felhasználók számára a hitelesítést a TOPdesk - vázlat biztonságos fiókkal az Azure AD összevonási alapján a SAML protokoll használatával.</span><span class="sxs-lookup"><span data-stu-id="51544-132">The objective of this section is to outline how to enable users to authenticate to TOPdesk - Secure with their account in Azure AD using federation based on the SAML protocol.</span></span>  
<span data-ttu-id="51544-133">Konfigurálása egyszeri bejelentkezéshez az TOPdesk - biztonságos kell embléma ikon fájl feltöltése.</span><span class="sxs-lookup"><span data-stu-id="51544-133">Configuring single sign-on for TOPdesk - Secure requires you to upload a logo icon file.</span></span> <span data-ttu-id="51544-134">Ahhoz, hogy az ikonfájl, lépjen kapcsolatba a TOPdesk támogatási csapatával.</span><span class="sxs-lookup"><span data-stu-id="51544-134">To get the icon file, contact the TOPdesk support team.</span></span>

### <a name="to-configure-single-sign-on-perform-the-following-steps"></a><span data-ttu-id="51544-135">Egyszeri bejelentkezés konfigurálásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-135">To configure single sign-on, perform the following steps:</span></span>
1. <span data-ttu-id="51544-136">Jelentkezzen be a **TOPdesk - biztonságos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="51544-136">Sign on to your **TOPdesk - Secure** company site as an administrator.</span></span>
2. <span data-ttu-id="51544-137">Az a **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="51544-137">In the **TOPdesk** menu, click **Settings**.</span></span>
   
    <span data-ttu-id="51544-138">![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="51544-138">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

3. <span data-ttu-id="51544-139">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="51544-139">Click **Login Settings**.</span></span>
   
    <span data-ttu-id="51544-140">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="51544-140">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

4. <span data-ttu-id="51544-141">Bontsa ki a **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="51544-141">Expand the **Login Settings** menu, and then click **General**.</span></span>
   
    <span data-ttu-id="51544-142">![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="51544-142">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

5. <span data-ttu-id="51544-143">Az a **biztonságos** szakasza a **SAML bejelentkezési** konfigurációs szakaszban, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-143">In the **Secure** section of the **SAML login** configuration section, perform the following steps:</span></span>
   
    <span data-ttu-id="51544-144">![Műszaki beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "műszaki beállításai")</span><span class="sxs-lookup"><span data-stu-id="51544-144">![Technical Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790855.png "Technical Settings")</span></span>
   
    <span data-ttu-id="51544-145">a.</span><span class="sxs-lookup"><span data-stu-id="51544-145">a.</span></span> <span data-ttu-id="51544-146">Kattintson a **letöltése** a nyilvános metaadatait tartalmazó fájl letöltéséhez, és mentse helyileg a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51544-146">Click **Download** to download the public metadata file, and then save it locally on your computer.</span></span>
   
    <span data-ttu-id="51544-147">b.</span><span class="sxs-lookup"><span data-stu-id="51544-147">b.</span></span> <span data-ttu-id="51544-148">Nyissa meg a metaadat-fájlt, és keresse meg a **AssertionConsumerService** csomópont.</span><span class="sxs-lookup"><span data-stu-id="51544-148">Open the metadata file, and then locate the **AssertionConsumerService** node.</span></span>
    
    <span data-ttu-id="51544-149">![Helyességi feltétel fogyasztói szolgáltatás](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "helyességi feltétel fogyasztói szolgáltatás")</span><span class="sxs-lookup"><span data-stu-id="51544-149">![Assertion Consumer Service](./media/active-directory-saas-topdesk-secure-tutorial/IC790856.png "Assertion Consumer Service")</span></span>
   
    <span data-ttu-id="51544-150">c.</span><span class="sxs-lookup"><span data-stu-id="51544-150">c.</span></span> <span data-ttu-id="51544-151">Másolás a **AssertionConsumerService** érték.</span><span class="sxs-lookup"><span data-stu-id="51544-151">Copy the **AssertionConsumerService** value.</span></span>  
      
    > [!NOTE]
    > <span data-ttu-id="51544-152">Szüksége lesz az értéket a **alkalmazás URL-cím konfigurálása** szakasz az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="51544-152">You will need the value in the **Configure App URL** section later in this tutorial.</span></span>
    > 
    > 

6. <span data-ttu-id="51544-153">Egy másik webes böngészőablakban, jelentkezzen be a **a klasszikus Azure portálon** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="51544-153">In a different web browser window, log into your **Azure classic portal** as an administrator.</span></span>

7. <span data-ttu-id="51544-154">Az a **TOPdesk - biztonságos** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a ** konfigurálása egyszeri bejelentkezés ** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51544-154">On the **TOPdesk - Secure** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On ** dialog.</span></span>
   
    <span data-ttu-id="51544-155">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="51544-155">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790602.png "Configure Single Sign-On")</span></span>

8. <span data-ttu-id="51544-156">Az a **hová bejelentkezni TOPdesk - biztonságos felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="51544-156">On the **How would you like users to sign on to TOPdesk - Secure** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="51544-157">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="51544-157">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790603.png "Configure Single Sign-On")</span></span>

9. <span data-ttu-id="51544-158">Az a **alkalmazás URL-cím konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="51544-158">On the **Configure App URL** page, perform the following steps:</span></span>
   
    <span data-ttu-id="51544-159">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="51544-159">![Configure App URL](./media/active-directory-saas-topdesk-secure-tutorial/IC790604.png "Configure App URL")</span></span>
   
    <span data-ttu-id="51544-160">a.</span><span class="sxs-lookup"><span data-stu-id="51544-160">a.</span></span> <span data-ttu-id="51544-161">Az a **TOPdesk - bejelentkezési biztonságos az URL-cím** szövegmező, írja be az URL-cím segítségével a felhasználók jelentkezzen be a TOPdesk - védett alkalmazás (pl.: "*https://qssolutions.topdesk.net*").</span><span class="sxs-lookup"><span data-stu-id="51544-161">In the **TOPdesk - Secure Sign On URL** textbox, type the URL used by your users to sign into your TOPdesk - Secure application (e.g.: "*https://qssolutions.topdesk.net*").</span></span>
   
    <span data-ttu-id="51544-162">b.</span><span class="sxs-lookup"><span data-stu-id="51544-162">b.</span></span> <span data-ttu-id="51544-163">Az a **TOPdesk – válasz URL-címe** szövegmező, illessze be a **TOPdesk - biztonságos AssertionConsumerService URL-** (pl.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span><span class="sxs-lookup"><span data-stu-id="51544-163">In the **TOPdesk – Public Reply URL** textbox, paste the **TOPdesk - Secure AssertionConsumerService URL** (e.g.: "*https://qssolutions.topdesk.net/tas/public/login/saml*")</span></span>
   
    <span data-ttu-id="51544-164">c.</span><span class="sxs-lookup"><span data-stu-id="51544-164">c.</span></span> <span data-ttu-id="51544-165">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="51544-165">Click **Next**.</span></span>

10. <span data-ttu-id="51544-166">A a **konfigurálhatja az egyszeri bejelentkezés TOPdesk - biztonságos** töltse le a metaadat-fájlt, kattintson **metaadatok letöltése**, és mentse helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51544-166">On the **Configure single sign-on at TOPdesk - Secure** page, to download your metadata file, click **Download metadata**, and then save the file locally on your computer.</span></span>
    
    <span data-ttu-id="51544-167">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="51544-167">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790605.png "Configure Single Sign-On")</span></span>

11. <span data-ttu-id="51544-168">A tanúsítványfájl létrehozásához hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-168">To create a certificate file, perform the following steps:</span></span>
    
    <span data-ttu-id="51544-169">![Tanúsítvány](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "tanúsítvány")</span><span class="sxs-lookup"><span data-stu-id="51544-169">![Certificate](./media/active-directory-saas-topdesk-secure-tutorial/IC790606.png "Certificate")</span></span>
    
    <span data-ttu-id="51544-170">a.</span><span class="sxs-lookup"><span data-stu-id="51544-170">a.</span></span> <span data-ttu-id="51544-171">Nyissa meg a letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="51544-171">Open the downloaded metadata file.</span></span>
    <span data-ttu-id="51544-172">b.</span><span class="sxs-lookup"><span data-stu-id="51544-172">b.</span></span> <span data-ttu-id="51544-173">Bontsa ki a **RoleDescriptor** csomópont egy **xsi: type** a **táplált: ApplicationServiceType**.</span><span class="sxs-lookup"><span data-stu-id="51544-173">Expand the **RoleDescriptor** node that has a **xsi:type** of **fed:ApplicationServiceType**.</span></span>
    <span data-ttu-id="51544-174">c.</span><span class="sxs-lookup"><span data-stu-id="51544-174">c.</span></span> <span data-ttu-id="51544-175">Másolja a értékének a **x.509** csomópont.</span><span class="sxs-lookup"><span data-stu-id="51544-175">Copy the value of the **X509Certificate** node.</span></span>
    <span data-ttu-id="51544-176">d.</span><span class="sxs-lookup"><span data-stu-id="51544-176">d.</span></span> <span data-ttu-id="51544-177">Mentse a másolt **x.509** érték helyileg a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51544-177">Save the copied **X509Certificate** value locally on your computer in a file.</span></span>

12. <span data-ttu-id="51544-178">A TOPdesk - a vállalati webhely biztonságos a **TOPdesk** menüben kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="51544-178">On your TOPdesk - Secure company site, in the **TOPdesk** menu, click **Settings**.</span></span>
    
    <span data-ttu-id="51544-179">![Beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "beállítások")</span><span class="sxs-lookup"><span data-stu-id="51544-179">![Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790598.png "Settings")</span></span>

13. <span data-ttu-id="51544-180">Kattintson a **bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="51544-180">Click **Login Settings**.</span></span>
    
    <span data-ttu-id="51544-181">![Bejelentkezési beállítások](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "bejelentkezési beállítások")</span><span class="sxs-lookup"><span data-stu-id="51544-181">![Login Settings](./media/active-directory-saas-topdesk-secure-tutorial/IC790599.png "Login Settings")</span></span>

14. <span data-ttu-id="51544-182">Bontsa ki a **bejelentkezési beállítások** menüben, majd kattintson **általános**.</span><span class="sxs-lookup"><span data-stu-id="51544-182">Expand the **Login Settings** menu, and then click **General**.</span></span>
    
    <span data-ttu-id="51544-183">![Általános](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "általános")</span><span class="sxs-lookup"><span data-stu-id="51544-183">![General](./media/active-directory-saas-topdesk-secure-tutorial/IC790600.png "General")</span></span>

15. <span data-ttu-id="51544-184">Az a **nyilvános** kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="51544-184">In the **Public** section, click **Add**.</span></span>
    
    <span data-ttu-id="51544-185">![Adja hozzá](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="51544-185">![Add](./media/active-directory-saas-topdesk-secure-tutorial/IC790607.png "Add")</span></span>

16. <span data-ttu-id="51544-186">Az a **SAML-alapú konfigurációs Segéd** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="51544-186">On the **SAML configuration assistant** dialog page, perform the following steps:</span></span>
    
    <span data-ttu-id="51544-187">![SAML-alapú konfigurációs Segéd](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML-alapú konfigurációs Segéd")</span><span class="sxs-lookup"><span data-stu-id="51544-187">![SAML Configuration Assistant](./media/active-directory-saas-topdesk-secure-tutorial/IC790608.png "SAML Configuration Assistant")</span></span>
    
    <span data-ttu-id="51544-188">a.</span><span class="sxs-lookup"><span data-stu-id="51544-188">a.</span></span> <span data-ttu-id="51544-189">A letöltött metaadatait tartalmazó fájl feltöltése a **összevonási metaadatok**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="51544-189">To upload your downloaded metadata file, under **Federation Metadata**, click **Browse**.</span></span>

    <span data-ttu-id="51544-190">b.</span><span class="sxs-lookup"><span data-stu-id="51544-190">b.</span></span> <span data-ttu-id="51544-191">Töltse fel a tanúsítványfájlt, a **tanúsítvány (RSA)**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="51544-191">To upload your certificate file, under **Certificate (RSA)**, click **Browse**.</span></span>

    <span data-ttu-id="51544-192">c.</span><span class="sxs-lookup"><span data-stu-id="51544-192">c.</span></span> <span data-ttu-id="51544-193">Fel kell töltenie az embléma fájlt során kapott azonosítóértékeket TOPdesk terméktámogatással a **embléma ikon**, kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="51544-193">To upload the logo file you got from the TOPdesk support team, under **Logo icon**, click **Browse**.</span></span>

    <span data-ttu-id="51544-194">d.</span><span class="sxs-lookup"><span data-stu-id="51544-194">d.</span></span> <span data-ttu-id="51544-195">Az a **felhasználói név attribútum** szövegmezőhöz típus **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="51544-195">In the **User name attribute** textbox, type **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**.</span></span>

    <span data-ttu-id="51544-196">e.</span><span class="sxs-lookup"><span data-stu-id="51544-196">e.</span></span> <span data-ttu-id="51544-197">Az a **megjelenített név** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="51544-197">In the **Display name** textbox, type a name for your configuration.</span></span>

    <span data-ttu-id="51544-198">f.</span><span class="sxs-lookup"><span data-stu-id="51544-198">f.</span></span> <span data-ttu-id="51544-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="51544-199">Click **Save**.</span></span>

17. <span data-ttu-id="51544-200">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="51544-200">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span>
    
    <span data-ttu-id="51544-201">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="51544-201">![Configure Single Sign-On](./media/active-directory-saas-topdesk-secure-tutorial/IC790609.png "Configure Single Sign-On")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="51544-202">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="51544-202">Configuring user provisioning</span></span>
<span data-ttu-id="51544-203">Ahhoz, hogy az Azure AD-felhasználók TOPdesk - be tudjon jelentkezni biztonságos, akkor ki kell építenie TOPdesk - biztonságos be.</span><span class="sxs-lookup"><span data-stu-id="51544-203">In order to enable Azure AD users to log into TOPdesk - Secure, they must be provisioned into TOPdesk - Secure.</span></span>  
<span data-ttu-id="51544-204">TOPdesk - esetén biztonságos, egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="51544-204">In the case of TOPdesk - Secure, provisioning is a manual task.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="51544-205">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-205">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="51544-206">Jelentkezzen be a **TOPdesk - biztonságos** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="51544-206">Sign on to your **TOPdesk - Secure** company site as administrator.</span></span>
2. <span data-ttu-id="51544-207">Kattintson a felső menüben **TOPdesk \> új \> támogatófájljait \> operátor**.</span><span class="sxs-lookup"><span data-stu-id="51544-207">In the menu on the top, click **TOPdesk \> New \> Support Files \> Operator**.</span></span>
   
    <span data-ttu-id="51544-208">![Operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "operátor")</span><span class="sxs-lookup"><span data-stu-id="51544-208">![Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790610.png "Operator")</span></span>

3. <span data-ttu-id="51544-209">Az a **új operátor** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-209">On the **New Operator** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="51544-210">![Új operátor](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "új operátor")</span><span class="sxs-lookup"><span data-stu-id="51544-210">![New Operator](./media/active-directory-saas-topdesk-secure-tutorial/IC790611.png "New Operator")</span></span>
   
    <span data-ttu-id="51544-211">a.</span><span class="sxs-lookup"><span data-stu-id="51544-211">a.</span></span> <span data-ttu-id="51544-212">Kattintson az Általános fülre.</span><span class="sxs-lookup"><span data-stu-id="51544-212">Click the General tab.</span></span>
   
    <span data-ttu-id="51544-213">b.</span><span class="sxs-lookup"><span data-stu-id="51544-213">b.</span></span> <span data-ttu-id="51544-214">Az a **vezetékneve** a szövegmező a **általános** területen írja be egy érvényes Azure Active Directory-fiókot rendelkezés kívánt utolsó nevét.</span><span class="sxs-lookup"><span data-stu-id="51544-214">In the **Surname** textbox of the **General** section, type the last name of a valid Azure Active Directory account you want to provision.</span></span>
   
    <span data-ttu-id="51544-215">c.</span><span class="sxs-lookup"><span data-stu-id="51544-215">c.</span></span> <span data-ttu-id="51544-216">Válassza ki a **hely** a fiók a **hely** szakasz.</span><span class="sxs-lookup"><span data-stu-id="51544-216">Select a **Site** for the account in the **Location** section.</span></span>
   
    <span data-ttu-id="51544-217">d.</span><span class="sxs-lookup"><span data-stu-id="51544-217">d.</span></span> <span data-ttu-id="51544-218">Az a **bejelentkezési név** a szövegmező a **TOPdesk bejelentkezési** területen írja be a felhasználó bejelentkezési nevét.</span><span class="sxs-lookup"><span data-stu-id="51544-218">In the **Login Name** textbox of the **TOPdesk Login** section, type a login name for your user.</span></span>
   
    <span data-ttu-id="51544-219">e.</span><span class="sxs-lookup"><span data-stu-id="51544-219">e.</span></span> <span data-ttu-id="51544-220">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="51544-220">Click **Save**.</span></span>

> [!NOTE]
> <span data-ttu-id="51544-221">Bármely más TOPdesk - biztonságos felhasználói fiók létrehozása eszközök vagy TOPdesk - felhasználói fiókok AAD kiépítése biztonságos által nyújtott API-kat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="51544-221">You can use any other TOPdesk - Secure user account creation tools or APIs provided by TOPdesk - Secure to provision AAD user accounts.</span></span>
> 
> 

## <a name="assigning-users"></a><span data-ttu-id="51544-222">Felhasználók hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="51544-222">Assigning users</span></span>
<span data-ttu-id="51544-223">A konfiguráció teszteléséhez kell biztosítania az Azure AD-felhasználók számára engedélyezni, használja az alkalmazás elérésére hozzárendelésével.</span><span class="sxs-lookup"><span data-stu-id="51544-223">To test your configuration, you need to grant the Azure AD users you want to allow using your application access to it by assigning them.</span></span>

### <a name="to-assign-users-to-topdesk---secure-perform-the-following-steps"></a><span data-ttu-id="51544-224">Felhasználók hozzárendelése TOPdesk - biztonságos, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="51544-224">To assign users to TOPdesk - Secure, perform the following steps:</span></span>
1. <span data-ttu-id="51544-225">A klasszikus Azure portálon hozzon létre egy olyan fiókot.</span><span class="sxs-lookup"><span data-stu-id="51544-225">In the Azure classic portal, create a test account.</span></span>
2. <span data-ttu-id="51544-226">Az a ** TOPdesk - biztonságos ** alkalmazás integráció lapján, kattintson a **felhasználók hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="51544-226">On the **TOPdesk - Secure **application integration page, click **Assign users**.</span></span>
   
    <span data-ttu-id="51544-227">![Felhasználók hozzárendelése](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "felhasználók hozzárendelése")</span><span class="sxs-lookup"><span data-stu-id="51544-227">![Assign Users](./media/active-directory-saas-topdesk-secure-tutorial/IC790612.png "Assign Users")</span></span>

3. <span data-ttu-id="51544-228">Adja meg a tesztfelhasználó számára, kattintson **hozzárendelése**, és kattintson a **Igen** a hozzárendelés megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="51544-228">Select your test user, click **Assign**, and then click **Yes** to confirm your assignment.</span></span>
   
    <span data-ttu-id="51544-229">![Igen](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Igen")</span><span class="sxs-lookup"><span data-stu-id="51544-229">![Yes](./media/active-directory-saas-topdesk-secure-tutorial/IC767830.png "Yes")</span></span>

<span data-ttu-id="51544-230">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="51544-230">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="51544-231">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="51544-231">For more details about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

