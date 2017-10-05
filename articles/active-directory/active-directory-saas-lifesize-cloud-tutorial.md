---
title: "Oktatóanyag: Azure Active Directory-integráció a Lifesize felhőalapú |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a Lifesize felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 75fab335-fdcd-4066-b42c-cc738fcb6513
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 7542360f9c75786bf400553090ba0a891d9c2fcc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-lifesize-cloud"></a><span data-ttu-id="a7a8f-103">Oktatóanyag: Azure Active Directoryval integrált Lifesize felhő</span><span class="sxs-lookup"><span data-stu-id="a7a8f-103">Tutorial: Azure Active Directory integration with Lifesize Cloud</span></span>

<span data-ttu-id="a7a8f-104">Ebben az oktatóanyagban elsajátíthatja Lifesize felhő integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a7a8f-104">In this tutorial, you learn how to integrate Lifesize Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a7a8f-105">Lifesize felhő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-105">Integrating Lifesize Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a7a8f-106">Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik Lifesize felhőbe</span><span class="sxs-lookup"><span data-stu-id="a7a8f-106">You can control in Azure AD who has access to Lifesize Cloud</span></span>
- <span data-ttu-id="a7a8f-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett Lifesize felhőbe (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="a7a8f-107">You can enable your users to automatically get signed-on to Lifesize Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a7a8f-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a7a8f-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a7a8f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a7a8f-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a7a8f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a7a8f-110">Prerequisites</span></span>

<span data-ttu-id="a7a8f-111">Az Azure AD-integráció konfigurálása a Lifesize felhőalapú, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-111">To configure Azure AD integration with Lifesize Cloud, you need the following items:</span></span>

- <span data-ttu-id="a7a8f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a7a8f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a7a8f-113">Egy Lifesize felhő egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a7a8f-113">A Lifesize Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a7a8f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a7a8f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a7a8f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a7a8f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a7a8f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a7a8f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-118">Scenario description</span></span>
<span data-ttu-id="a7a8f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a7a8f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a7a8f-121">A gyűjteményből Lifesize felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-121">Adding Lifesize Cloud from the gallery</span></span>
2. <span data-ttu-id="a7a8f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a7a8f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-lifesize-cloud-from-the-gallery"></a><span data-ttu-id="a7a8f-123">A gyűjteményből Lifesize felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-123">Adding Lifesize Cloud from the gallery</span></span>
<span data-ttu-id="a7a8f-124">Az Azure AD integrálása a Lifesize felhő konfigurálásához kell hozzáadnia Lifesize felhőalapú a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-124">To configure the integration of Lifesize Cloud into Azure AD, you need to add Lifesize Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a7a8f-125">**A gyűjteményből Lifesize felhő hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-125">**To add Lifesize Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a7a8f-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a7a8f-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a7a8f-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a7a8f-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** párbeszédpanel tetején gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-131">To add new application, click **New application** button on the top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a7a8f-133">Írja be a keresőmezőbe, **Lifesize felhő**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-133">In the search box, type **Lifesize Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_search.png)

5. <span data-ttu-id="a7a8f-135">Az eredmények panelen válassza ki a **Lifesize felhő**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-135">In the results panel, select **Lifesize Cloud**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a7a8f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a7a8f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a7a8f-138">Ebben a szakaszban konfigurálhatja, és a Lifesize felhőalapú Azure AD az egyszeri bejelentkezés teszt "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-138">In this section, you configure and test Azure AD single sign-on with Lifesize Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a7a8f-139">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Lifesize felhőben Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-139">For single sign-on to work, Azure AD needs to know what the counterpart user in Lifesize Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="a7a8f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó Lifesize felhőben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-140">In other words, a link relationship between an Azure AD user and the related user in Lifesize Cloud needs to be established.</span></span>

<span data-ttu-id="a7a8f-141">Lifesize felhőben, rendelje az értékét a **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a hivatkozás kapcsolat létrehozására.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-141">In Lifesize Cloud, assign the value of the **user name** in Azure AD as the value of the **Username** to establish the link relationship.</span></span>

<span data-ttu-id="a7a8f-142">Az Azure AD egyszeri bejelentkezést a Lifesize felhőalapú tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-142">To configure and test Azure AD single sign-on with Lifesize Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a7a8f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a7a8f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a7a8f-145">**[Lifesize felhő tesztfelhasználó létrehozása](#creating-a-lifesize-cloud-test-user)**  – egy megfelelője a Britta Simon rendelkezik, amely csatolva van a felhasználó az Azure AD-ábrázolását Lifesize felhőben.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-145">**[Creating a Lifesize Cloud test user](#creating-a-lifesize-cloud-test-user)** - to have a counterpart of Britta Simon in Lifesize Cloud that is linked to the Azure AD representation of user.</span></span>
4. <span data-ttu-id="a7a8f-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a7a8f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a7a8f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a7a8f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az Lifesize felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your Lifesize Cloud application.</span></span>

<span data-ttu-id="a7a8f-150">**Lifesize a felhő konfigurálása az Azure AD egyszeri bejelentkezést, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-150">**To configure Azure AD single sign-on with Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a7a8f-151">Az Azure portálon a a **Lifesize felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-151">In the Azure portal, on the **Lifesize Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a7a8f-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_samlbase.png)

3. <span data-ttu-id="a7a8f-155">Az a **Lifesize felhőalapú tartományt és URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-155">On the **Lifesize Cloud Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url.png)

    <span data-ttu-id="a7a8f-157">a.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-157">a.</span></span> <span data-ttu-id="a7a8f-158">Az a **bejelentkezési URL-cím** szövegmező, adja meg a következő minta használatával URL-címe:`https://login.lifesizecloud.com/ls/?acs`</span><span class="sxs-lookup"><span data-stu-id="a7a8f-158">In the **Sign-on URL** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/ls/?acs`</span></span>

    <span data-ttu-id="a7a8f-159">b.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-159">b.</span></span> <span data-ttu-id="a7a8f-160">Az a **azonosító** szövegmező, adja meg a következő minta használatával URL-címe:`https://login.lifesizecloud.com/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="a7a8f-160">In the **Identifier** textbox, type a URL using the following pattern: `https://login.lifesizecloud.com/<companyname>`</span></span>

     
4. <span data-ttu-id="a7a8f-161">Ellenőrizze **megjelenítése speciális URL-beállításainak**, hajtsa végre a következő lépést:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-161">Check **Show advanced URL settings**, perform the following step:</span></span>    
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_url1.png)

    <span data-ttu-id="a7a8f-163">Az a **állapot továbbítása** szövegmező, adja meg a következő minta használatával URL-címe:`https://webapp.lifesizecloud.com/?ent=<identifier>`</span><span class="sxs-lookup"><span data-stu-id="a7a8f-163">In the **Relay state** textbox, type a URL using the following pattern: `https://webapp.lifesizecloud.com/?ent=<identifier>`</span></span>
   
   > [!NOTE] 
   ><span data-ttu-id="a7a8f-164">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-164">Please note that these are not the real values.</span></span> <span data-ttu-id="a7a8f-165">akkor frissítheti ezeket az értékeket a tényleges bejelentkezési URL-cím, továbbítási állapotot és azonosítója.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-165">you have to update these values with the actual Sign-On URL, Relay State, and Identifier.</span></span> <span data-ttu-id="a7a8f-166">Ügyfél [Lifesize felhőalapú ügyfél-támogatási csoport](https://www.lifesize.com/support) bejelentkezési URL-címet, és azonosítóértékek, és lekérheti továbbítási állapotérték SSO-konfiguráció esetén, tekintse meg az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-166">Contact [Lifesize Cloud Client support team](https://www.lifesize.com/support) to get Sign-On URL, and Identifier values and you can get Relay State  value from SSO Configuration that is explained later in the tutorial.</span></span>

4. <span data-ttu-id="a7a8f-167">Az a **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-167">On the **SAML Signing Certificate** section, click **Certificate(Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_certificate.png) 

5. <span data-ttu-id="a7a8f-169">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-169">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a7a8f-171">A a **Lifesize Felhőkonfiguráció** kattintson **Lifesize felhő konfigurálása** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-171">On the **Lifesize Cloud Configuration** section, click **Configure Lifesize Cloud** to open **Configure sign-on** window.</span></span> <span data-ttu-id="a7a8f-172">Másolás a **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a a **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-172">Copy the **SAML Entity ID, and SAML Single Sign-On Service URL** from the **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_configure.png) 

7. <span data-ttu-id="a7a8f-174">Az alkalmazáshoz, a bejelentkezés a Lifesize felhőalapú alkalmazásnál rendszergazda jogosultságokkal a beállított SSO eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-174">To get SSO configured for your application, login into the Lifesize Cloud application with Admin privileges.</span></span>

8. <span data-ttu-id="a7a8f-175">A jobb felső sarokban kattintson a nevére, és kattintson a a **speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-175">In the top right corner click on your name and then click on the **Advance Settings**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_06.png)

9. <span data-ttu-id="a7a8f-177">A Speciális beállítások most kattintson a a **SSO konfigurációs** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-177">In the Advance Settings now click on the **SSO Configuration** link.</span></span> <span data-ttu-id="a7a8f-178">Az ekkor megnyílik az egyszeri bejelentkezés konfigurálása lapon a példány.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-178">It will open the SSO Configuration page for your instance.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_07.png)

10. <span data-ttu-id="a7a8f-180">A következő értékeket konfigurálhatja a SSO konfigurációs felhasználói felületen.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-180">Now configure the following values in the SSO configuration UI.</span></span>    
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesizecloud_08.png)
    
    <span data-ttu-id="a7a8f-182">a.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-182">a.</span></span> <span data-ttu-id="a7a8f-183">A **Identity Provider kibocsátó** szövegmezőhöz illessze be az értékét **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-183">In **Identity Provider Issuer** textbox, paste the value of **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a7a8f-184">b.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-184">b.</span></span>  <span data-ttu-id="a7a8f-185">A **bejelentkezési URL-cím** szövegmezőhöz illessze be az értékét **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-185">In **Login URL** textbox, paste the value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a7a8f-186">c.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-186">c.</span></span> <span data-ttu-id="a7a8f-187">A base-64 kódolású tanúsítvány megnyitása a Jegyzettömbben az Azure portálról letöltött, a tartalmának másolása a vágólapra és illessze be azt a **X.509 tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-187">Open your base-64 encoded certificate in notepad downloaded from Azure portal, copy the content of it into your clipboard, and then paste it to the **X.509 Certificate** textbox.</span></span>
  
    <span data-ttu-id="a7a8f-188">d.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-188">d.</span></span> <span data-ttu-id="a7a8f-189">A SAML attribútum Keresztnév szövegmező hozzárendelések adja meg a értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-189">In the SAML Attribute mappings for the First Name text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname**</span></span>
    
    <span data-ttu-id="a7a8f-190">e.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-190">e.</span></span> <span data-ttu-id="a7a8f-191">A SAML attribútum társítását a a **Vezetéknév** szövegmezőbe írja be a értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-191">In the SAML Attribute mapping for the **Last Name** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname**</span></span>
    
    <span data-ttu-id="a7a8f-192">f.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-192">f.</span></span> <span data-ttu-id="a7a8f-193">A SAML attribútum társítását a a **E-mail** szövegmezőbe írja be a értékével megegyező **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-193">In the SAML Attribute mapping for the **Email** text box enter the value as **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress**</span></span>

11. <span data-ttu-id="a7a8f-194">Ellenőrizze a konfigurációt, kattintson a **teszt** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-194">To check the configuration you can click on the **Test** button.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="a7a8f-195">A sikeres vizsgálat kell a konfiguráció varázsló az Azure ad-ben, és hozzáférést is biztosít a felhasználókhoz vagy csoportokhoz a tesztet hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-195">For successful testing you need to complete the configuration wizard in Azure AD and also provide access to users or groups who can perform the test.</span></span>

12. <span data-ttu-id="a7a8f-196">Engedélyezze az egyszeri bejelentkezés ellenőrzése a következőn: a **SSO engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-196">Enable the SSO by checking on the **Enable SSO** button.</span></span>

13. <span data-ttu-id="a7a8f-197">Most kattintson a a **frissítés** gombra, hogy a beállítások lesznek mentve.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-197">Now click on the **Update** button so that all the settings are saved.</span></span> <span data-ttu-id="a7a8f-198">Ezzel a RelayState értéket hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-198">This will generate the RelayState value.</span></span> <span data-ttu-id="a7a8f-199">Másolás a RelayState érték, amely a szövegmezőben jön létre, illessze be a **továbbítási állapotot** szövegmező alatt **Lifesize felhőalapú tartományt és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-199">Copy the RelayState value, which is generated in the text box, paste it in the **Relay State** textbox under **Lifesize Cloud Domain and URLs** section.</span></span> 

> [!TIP]
> <span data-ttu-id="a7a8f-200">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a7a8f-200">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a7a8f-201">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-201">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a7a8f-202">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a7a8f-202">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a7a8f-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-203">Creating an Azure AD test user</span></span>

<span data-ttu-id="a7a8f-204">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-204">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a7a8f-206">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-206">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a7a8f-207">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-207">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a7a8f-209">Azon felhasználók listájának megtekintéséhez keresse fel **felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-209">To display the list of users, go to **Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a7a8f-211">Lehetőségre a **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-211">To open the **User** dialog, click **Add** on the top of the dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a7a8f-213">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a7a8f-213">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-lifesize-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a7a8f-215">a.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-215">a.</span></span> <span data-ttu-id="a7a8f-216">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-216">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a7a8f-217">b.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-217">b.</span></span> <span data-ttu-id="a7a8f-218">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-218">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a7a8f-219">c.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-219">c.</span></span> <span data-ttu-id="a7a8f-220">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-220">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a7a8f-221">d.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-221">d.</span></span> <span data-ttu-id="a7a8f-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-222">Click **Create**.</span></span>
 
### <a name="creating-a-lifesize-cloud-test-user"></a><span data-ttu-id="a7a8f-223">Lifesize felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-223">Creating a Lifesize Cloud test user</span></span>

<span data-ttu-id="a7a8f-224">Ebben a szakaszban egy felhasználó Britta Simon nevű Lifesize felhőben hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-224">In this section, you create a user called Britta Simon in Lifesize Cloud.</span></span> <span data-ttu-id="a7a8f-225">Lifesize felhő támogatja, a felhasználók automatikus átadása.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-225">Lifesize cloud does support automatic user provisioning.</span></span> <span data-ttu-id="a7a8f-226">Az Azure AD a sikeres hitelesítést követően a felhasználó automatikusan megkapják az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-226">After successful authentication at Azure AD, the user will be automatically provisioned in the application.</span></span> 

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a7a8f-227">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a7a8f-227">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a7a8f-228">Ebben a szakaszban engedélyezze Britta Simon által biztosított hozzáférés Lifesize felhőalapú Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-228">In this section, you enable Britta Simon to use Azure single sign-on by granting access to Lifesize Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a7a8f-230">**Britta Simon hozzárendelése Lifesize felhő, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a7a8f-230">**To assign Britta Simon to Lifesize Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="a7a8f-231">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-231">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a7a8f-233">Az alkalmazások listában válassza ki a **Lifesize felhő**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-233">In the applications list, select **Lifesize Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_lifesize-cloud_app.png) 

3. <span data-ttu-id="a7a8f-235">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-235">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a7a8f-237">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-237">Click **Add** button.</span></span> <span data-ttu-id="a7a8f-238">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-238">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a7a8f-240">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-240">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a7a8f-241">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-241">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a7a8f-242">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-242">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a7a8f-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a7a8f-243">Testing single sign-on</span></span>

<span data-ttu-id="a7a8f-244">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-244">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a7a8f-245">Ha a hozzáférési panelen Lifesize felhő csempére kattint, bejelentkezési oldalán Lifesize felhőalapú alkalmazásnál szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a7a8f-245">When you click the Lifesize Cloud tile in the Access Panel, you should get login page of Lifesize Cloud application.</span></span>
<span data-ttu-id="a7a8f-246">A hozzáférési Panel kapcsolatos további információkért lásd: [a hozzáférési Panel bemutatása](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a7a8f-246">For more information about the Access Panel, see [Introduction to the Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7a8f-247">További források</span><span class="sxs-lookup"><span data-stu-id="a7a8f-247">Additional resources</span></span>

* [<span data-ttu-id="a7a8f-248">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a7a8f-248">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a7a8f-249">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a7a8f-249">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-lifesize-cloud-tutorial/tutorial_general_203.png

