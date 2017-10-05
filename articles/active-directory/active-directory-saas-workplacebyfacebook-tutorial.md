---
title: "Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a munkahely által Facebook között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: 1590a66f215f0c093d24ff602c0ad951ba1e1eea
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a><span data-ttu-id="3f2ea-103">Oktatóanyag: Azure Active Directoryval integrált munkahelyi által Facebook-on</span><span class="sxs-lookup"><span data-stu-id="3f2ea-103">Tutorial: Azure Active Directory integration with Workplace by Facebook</span></span>

<span data-ttu-id="3f2ea-104">Ebben az oktatóanyagban elsajátíthatja által Facebook munkahelyi integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="3f2ea-104">In this tutorial, you learn how to integrate Workplace by Facebook with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="3f2ea-105">Munkahelyi által Facebook integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-105">Integrating Workplace by Facebook with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="3f2ea-106">Az Azure AD, aki hozzáféréssel rendelkezik a munkahelyi Facebook által szabályozható</span><span class="sxs-lookup"><span data-stu-id="3f2ea-106">You can control in Azure AD who has access to Workplace by Facebook</span></span>
- <span data-ttu-id="3f2ea-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett munkahelyi Facebook (egyszeri bejelentkezés) által az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="3f2ea-107">You can enable your users to automatically get signed-on to Workplace by Facebook (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="3f2ea-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3f2ea-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="3f2ea-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="3f2ea-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3f2ea-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3f2ea-110">Prerequisites</span></span>

<span data-ttu-id="3f2ea-111">Konfigurálása az Azure AD-integrációs munkahelyi által Facebook, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-111">To configure Azure AD integration with Workplace by Facebook, you need the following items:</span></span>

- <span data-ttu-id="3f2ea-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="3f2ea-112">An Azure AD subscription</span></span>
- <span data-ttu-id="3f2ea-113">A munkahelyi Facebook egyszeri bejelentkezés által engedélyezett előfizetés</span><span class="sxs-lookup"><span data-stu-id="3f2ea-113">A Workplace by Facebook single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="3f2ea-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="3f2ea-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="3f2ea-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="3f2ea-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3f2ea-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="3f2ea-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-118">Scenario description</span></span>
<span data-ttu-id="3f2ea-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="3f2ea-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="3f2ea-121">Munkahelyi által Facebook hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3f2ea-121">Adding Workplace by Facebook from the gallery</span></span>
2. <span data-ttu-id="3f2ea-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f2ea-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-workplace-by-facebook-from-the-gallery"></a><span data-ttu-id="3f2ea-123">Munkahelyi által Facebook hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="3f2ea-123">Adding Workplace by Facebook from the gallery</span></span>
<span data-ttu-id="3f2ea-124">Az Azure AD integrálása a munkahely által Facebook konfigurálásához kell hozzáadnia munkahelyi Facebook által a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-124">To configure the integration of Workplace by Facebook into Azure AD, you need to add Workplace by Facebook from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="3f2ea-125">**Adja hozzá a munkahelyi Facebook által a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2ea-125">**To add Workplace by Facebook from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2ea-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="3f2ea-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="3f2ea-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="3f2ea-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="3f2ea-133">Írja be a keresőmezőbe, **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-133">In the search box, type **Workplace by Facebook**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_search.png)

5. <span data-ttu-id="3f2ea-135">Az eredmények panelen válassza ki a **által Facebook munkahelyi**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-135">In the results panel, select **Workplace by Facebook**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="3f2ea-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="3f2ea-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="3f2ea-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD az egyszeri bejelentkezés a munkahelyi által Facebook "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="3f2ea-138">In this section, you configure and test Azure AD single sign-on with Workplace by Facebook based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="3f2ea-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó által Facebook munkahelyi Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Workplace by Facebook is to a user in Azure AD.</span></span> <span data-ttu-id="3f2ea-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a munkahelyi Facebook által közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-140">In other words, a link relationship between an Azure AD user and the related user in Workplace by Facebook needs to be established.</span></span>

<span data-ttu-id="3f2ea-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** munkahelyi Facebook által.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Workplace by Facebook.</span></span>

<span data-ttu-id="3f2ea-142">Az Azure AD az egyszeri bejelentkezés a munkahelyi által Facebook tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-142">To configure and test Azure AD single sign-on with Workplace by Facebook, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="3f2ea-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="3f2ea-144">**[Újrahitelesítés gyakoriságának beállítása](#configuring-reauthentication-frequency)**  - bekéri a SAML-ellenőrzés a munkahelyi konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-144">**[Configuring Reauthentication Frequency](#configuring-reauthentication-frequency)** - to configure Workplace to prompt for a SAML check.</span></span>
3. <span data-ttu-id="3f2ea-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="3f2ea-146">**[A munkahelyi Facebook teszt felhasználó létrehozása](#creating-a-workplace-by-facebook-test-user)**  - való Britta Simon egy megfelelője a Facebook, a felhasználó az Azure AD-ábrázolását kapcsolódó által munkahelyi.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-146">**[Creating a Workplace by Facebook test user](#creating-a-workplace-by-facebook-test-user)** - to have a counterpart of Britta Simon in Workplace by Facebook that is linked to the Azure AD representation of user.</span></span>
5. <span data-ttu-id="3f2ea-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="3f2ea-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-148">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="3f2ea-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="3f2ea-150">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és egyszeri bejelentkezés konfigurálása a munkahelyi Facebook-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-150">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Workplace by Facebook application.</span></span>

<span data-ttu-id="3f2ea-151">**Konfigurálása az Azure AD az egyszeri bejelentkezés munkahelyi által Facebook, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="3f2ea-151">**To configure Azure AD single sign-on with Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2ea-152">Az Azure portálon a a **által Facebook munkahelyi** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-152">In the Azure portal, on the **Workplace by Facebook** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="3f2ea-154">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-154">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_samlbase.png)

3. <span data-ttu-id="3f2ea-156">Az a **Facebook-tartomány és az URL-címek munkahelyi** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-156">On the **Workplace by Facebook Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_url.png)

    <span data-ttu-id="3f2ea-158">a.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-158">a.</span></span> <span data-ttu-id="3f2ea-159">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://<instancename>.facebook.com`</span><span class="sxs-lookup"><span data-stu-id="3f2ea-159">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://<instancename>.facebook.com`</span></span>

    <span data-ttu-id="3f2ea-160">b.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-160">b.</span></span> <span data-ttu-id="3f2ea-161">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://www.facebook.com/company/<instancename>`</span><span class="sxs-lookup"><span data-stu-id="3f2ea-161">In the **Identifier** textbox, type a URL using the following pattern: `https://www.facebook.com/company/<instancename>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="3f2ea-162">Ezek az értékek nincsenek tényleges.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-162">These values are not the real.</span></span> <span data-ttu-id="3f2ea-163">Frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-163">Update these values with the actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="3f2ea-164">Ügyfél [Facebook ügyfél-támogatási csoport által munkahelyi](https://workplace.fb.com/faq/) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-164">Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/) to get these values.</span></span> 

4. <span data-ttu-id="3f2ea-165">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-165">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_certificate.png) 

5. <span data-ttu-id="3f2ea-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-167">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="3f2ea-169">Az a **Facebook konfigurációja munkahelyi** területen kattintson **munkahelyi konfigurálása által a Facebook** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-169">On the **Workplace by Facebook Configuration** section, click **Configure Workplace by Facebook** to open **Configure sign-on** window.</span></span> <span data-ttu-id="3f2ea-170">Másolás a **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="3f2ea-170">Copy the **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/config.png) 

7. <span data-ttu-id="3f2ea-172">Egy másik webes böngészőablakban, jelentkezzen be a munkahelyi Facebook vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-172">In a different web browser window, login to your Workplace by Facebook company site as an administrator.</span></span>
  
   > [!NOTE] 
   > <span data-ttu-id="3f2ea-173">A SAML-hitelesítési folyamat részeként munkahelyi előfordulhat, hogy kihasználhassák a lekérdezési karakterláncok legfeljebb 2,5 kilobájt méretű ahhoz, hogy az Azure AD-paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-173">As part of the SAML authentication process, Workplace may utilize query strings of up to 2.5 kilobytes in size in order to pass parameters to Azure AD.</span></span>

8. <span data-ttu-id="3f2ea-174">Az a **vállalati irányítópult**, navigáljon a **hitelesítési** fülre.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-174">In the **Company Dashboard**, go to the **Authentication** tab.</span></span>

9. <span data-ttu-id="3f2ea-175">A **SAML-alapú hitelesítés**, jelölje be **SSO csak** a legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-175">Under **SAML Authentication**, select **SSO Only** from the drop-down list.</span></span>

10. <span data-ttu-id="3f2ea-176">Adjon meg az értékeket, átmásolva **Facebook konfigurációja munkahelyi** szakasza a megfelelő mezőkbe az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-176">Input the values copied from **Workplace by Facebook Configuration** section of the Azure portal into the corresponding fields:</span></span>

    *   <span data-ttu-id="3f2ea-177">A **SAML-alapú URL-cím** szövegmezőhöz illessze be az értékét **egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-177">In **SAML URL** textbox, paste the value of **Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="3f2ea-178">A **SAML kiállítójának URL-címe szövegmező**, illessze be az értékét **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-178">In **SAML Issuer URL textbox**, paste the value of **SAML Entity ID**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="3f2ea-179">A **SAML kijelentkezési átirányítási** (nem kötelező), illessze be az értékét **Sign-Out URL-cím**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-179">In **SAML Logout Redirect** (Optional), paste the value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
    *   <span data-ttu-id="3f2ea-180">Nyissa meg a **base-64 kódolású tanúsítvány** Azure portálról letöltött fájlt, másolja a vágólapra a tartalmát, és illessze be azt a **SAML tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-180">Open your **base-64 encoded certificate** in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **SAML Certificate** textbox.</span></span>

11. <span data-ttu-id="3f2ea-181">Szükség lehet a célközönség URL-címet, címzett URL-címet, és ACS (helyességi feltétel fogyasztói szolgáltatás) URL-cím alatt felsorolva a **SAML-alapú konfigurációs** szakasz.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-181">You may need to enter the Audience URL, Recipient URL, and ACS (Assertion Consumer Service) URL listed under the **SAML Configuration** section.</span></span>

12. <span data-ttu-id="3f2ea-182">A szakasz alján görgessen, majd kattintson a **teszt SSO** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-182">Scroll to the bottom of the section and click the **Test SSO** button.</span></span> <span data-ttu-id="3f2ea-183">Ennek eredményeképp a egy előugró ablakban jelenik meg az Azure AD bejelentkezési oldal jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-183">This results in a popup window appearing with Azure AD login page presented.</span></span> <span data-ttu-id="3f2ea-184">Adja meg a hitelesítő adatait hitelesítéséhez szokásos módon.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-184">Enter your credentials in as normal to authenticate.</span></span> 

    <span data-ttu-id="3f2ea-185">**Hibaelhárítás:** ellenőrizze az e-mail cím, az Azure AD vissza, megegyezik a munkahelyi fiókkal jelentkezett be az ad vissza.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-185">**Troubleshooting:** Ensure the email address being returned back from Azure AD is the same as the Workplace account you are logged in with.</span></span>

13. <span data-ttu-id="3f2ea-186">Ha a vizsgálat sikeresen befejeződött, görgessen a lap alján, és kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-186">Once the test has been completed successfully, scroll to the bottom of the page and click the **Save** button.</span></span>

14. <span data-ttu-id="3f2ea-187">Minden felhasználó használja a munkahelyi most számára jelenik meg az Azure AD bejelentkezési oldalt a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-187">All users using Workplace will now be presented with Azure AD login page for authentication.</span></span>

15. <span data-ttu-id="3f2ea-188">**SAML kijelentkezési átirányítási (nem kötelező)** -</span><span class="sxs-lookup"><span data-stu-id="3f2ea-188">**SAML Logout Redirect (optional)** -</span></span> 

    <span data-ttu-id="3f2ea-189">Ha szeretné, opcionálisan egy SAML kijelentkezési URL-címek konfigurálása, mutasson az Azure AD kijelentkezési oldalon használható.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-189">You can choose to optionally configure a SAML Logout Url, which can be used to point at Azure AD's logout page.</span></span> <span data-ttu-id="3f2ea-190">Ha ezt a beállítást engedélyezve és konfigurálva van, a felhasználó már nem jutnak a munkahelyi kijelentkezési lapra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-190">When this setting is enabled and configured, the user will no longer be directed to the Workplace logout page.</span></span> <span data-ttu-id="3f2ea-191">Ehelyett a felhasználó hozzá lett adva a SAML kijelentkezési átirányítási beállítást a URL-címre irányítja.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-191">Instead, the user will be redirected to the url that was added in the SAML Logout Redirect setting.</span></span>


> [!TIP]
> <span data-ttu-id="3f2ea-192">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="3f2ea-192">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="3f2ea-193">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-193">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="3f2ea-194">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="3f2ea-194">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="configuring-reauthentication-frequency"></a><span data-ttu-id="3f2ea-195">Újrahitelesítés gyakoriság beállítása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-195">Configuring Reauthentication Frequency</span></span>

<span data-ttu-id="3f2ea-196">Beállíthatja a munkahelyi kérjen egy SAML-ellenőrzést minden nap, három nap, hét, két hét, hónap vagy egyáltalán nem.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-196">You can configure Workplace to prompt for a SAML check every day, three days, week, two weeks, month or never.</span></span>

> [!NOTE] 
><span data-ttu-id="3f2ea-197">A mobilalkalmazás az SAML-ellenőrzését minimális értéke egy hét.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-197">The minimum value for the SAML check on mobile applications is set to one week.</span></span>

<span data-ttu-id="3f2ea-198">A SAML alaphelyzetbe állítja az összes felhasználó számára a gombra kattintva is kényszeríthető: szükséges SAML-hitelesítés az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-198">You can also force a SAML reset for all users using the button: Require SAML authentication for all users now.</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="3f2ea-199">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-199">Creating an Azure AD test user</span></span>
<span data-ttu-id="3f2ea-200">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-200">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="3f2ea-202">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="3f2ea-202">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2ea-203">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-203">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="3f2ea-205">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-205">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="3f2ea-207">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-207">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="3f2ea-209">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="3f2ea-209">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-workplacebyfacebook-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="3f2ea-211">a.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-211">a.</span></span> <span data-ttu-id="3f2ea-212">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-212">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="3f2ea-213">b.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-213">b.</span></span> <span data-ttu-id="3f2ea-214">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-214">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="3f2ea-215">c.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-215">c.</span></span> <span data-ttu-id="3f2ea-216">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-216">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="3f2ea-217">d.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-217">d.</span></span> <span data-ttu-id="3f2ea-218">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-218">Click **Create**.</span></span>
 
### <a name="creating-a-workplace-by-facebook-test-user"></a><span data-ttu-id="3f2ea-219">A munkahelyi Facebook teszt felhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-219">Creating a Workplace by Facebook test user</span></span>

<span data-ttu-id="3f2ea-220">Ebben a szakaszban Britta Simon nevű felhasználó létrehozta a munkaterületen Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-220">In this section, a user called Britta Simon is created in Workplace by Facebook.</span></span> <span data-ttu-id="3f2ea-221">Munkahelyi Facebook által támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-221">Workplace by Facebook supports just-in-time provisioning, which is enabled by default.</span></span>

<span data-ttu-id="3f2ea-222">Nincs olyan művelet, ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-222">There is no action for you in this section.</span></span> <span data-ttu-id="3f2ea-223">Ha a felhasználó nem létezik a munkahely által Facebook, egy új hozta létre munkahelyi elérésére tett kísérlet során Facebook-on.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-223">If a user doesn't exist in Workplace by Facebook, a new one is created when you attempt to access Workplace by Facebook.</span></span>

>[!Note]
><span data-ttu-id="3f2ea-224">Ha a felhasználót manuálisan kell létrehozni, lépjen kapcsolatba kell [munkahelyi Facebook ügyfél-támogatási csoport szerint](https://workplace.fb.com/faq/)</span><span class="sxs-lookup"><span data-stu-id="3f2ea-224">If you need to create a user manually, Contact [Workplace by Facebook Client support team](https://workplace.fb.com/faq/)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="3f2ea-225">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="3f2ea-225">Assigning the Azure AD test user</span></span>

<span data-ttu-id="3f2ea-226">Ebben a szakaszban Britta Simon munkahelyi Facebook által biztosított hozzáférés szerint használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-226">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Workplace by Facebook.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="3f2ea-228">**Britta Simon hozzárendelése munkahelyi által Facebook, a következő lépésekkel:**</span><span class="sxs-lookup"><span data-stu-id="3f2ea-228">**To assign Britta Simon to Workplace by Facebook, perform the following steps:**</span></span>

1. <span data-ttu-id="3f2ea-229">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-229">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="3f2ea-231">Az alkalmazások listában válassza ki a **által Facebook munkahelyi**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-231">In the applications list, select **Workplace by Facebook**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_workplacebyfacebook_app.png) 

3. <span data-ttu-id="3f2ea-233">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-233">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="3f2ea-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-235">Click **Add** button.</span></span> <span data-ttu-id="3f2ea-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="3f2ea-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-238">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="3f2ea-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="3f2ea-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="3f2ea-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="3f2ea-241">Testing single sign-on</span></span>

<span data-ttu-id="3f2ea-242">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="3f2ea-242">If you want to test your single sign-on settings, open the Access Panel.</span></span>
<span data-ttu-id="3f2ea-243">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="3f2ea-243">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>


## <a name="additional-resources"></a><span data-ttu-id="3f2ea-244">További források</span><span class="sxs-lookup"><span data-stu-id="3f2ea-244">Additional resources</span></span>

* [<span data-ttu-id="3f2ea-245">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-245">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="3f2ea-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="3f2ea-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="3f2ea-247">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="3f2ea-247">Configure User Provisioning</span></span>](active-directory-saas-workplacebyfacebook-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-workplacebyfacebook-tutorial/tutorial_general_203.png

