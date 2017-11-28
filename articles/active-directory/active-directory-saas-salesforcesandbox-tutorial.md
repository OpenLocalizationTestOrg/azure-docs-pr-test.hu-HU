---
title: "Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Salesforce védőfal között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: ee54c39e-ce20-42a4-8531-da7b5f40f57c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2017
ms.author: jeedes
ms.openlocfilehash: 7539f08356568a17ebfcee2764bbbefa129b0553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-salesforce-sandbox"></a><span data-ttu-id="08612-103">Oktatóanyag: Azure Active Directoryval integrált Salesforce védőfal</span><span class="sxs-lookup"><span data-stu-id="08612-103">Tutorial: Azure Active Directory integration with Salesforce Sandbox</span></span>

<span data-ttu-id="08612-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Salesforce védőfal az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="08612-104">In this tutorial, you learn how toointegrate Salesforce Sandbox with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="08612-105">Salesforce védőfal integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="08612-105">Integrating Salesforce Sandbox with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="08612-106">Megadhatja a hozzáférés tooSalesforce védőfal rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="08612-106">You can control in Azure AD who has access tooSalesforce Sandbox</span></span>
- <span data-ttu-id="08612-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSalesforce védőfal (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="08612-107">You can enable your users tooautomatically get signed-on tooSalesforce Sandbox (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="08612-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="08612-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="08612-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="08612-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="08612-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="08612-110">Prerequisites</span></span>

<span data-ttu-id="08612-111">az Azure AD-integráció a Salesforce védőfal tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="08612-111">tooconfigure Azure AD integration with Salesforce Sandbox, you need hello following items:</span></span>

- <span data-ttu-id="08612-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="08612-112">An Azure AD subscription</span></span>
- <span data-ttu-id="08612-113">A Salesforce védőfal egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="08612-113">A Salesforce Sandbox single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="08612-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="08612-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="08612-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="08612-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="08612-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="08612-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="08612-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="08612-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="08612-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="08612-118">Scenario description</span></span>
<span data-ttu-id="08612-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="08612-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="08612-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="08612-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="08612-121">Salesforce védőfal hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="08612-121">Adding Salesforce Sandbox from hello gallery</span></span>
2. <span data-ttu-id="08612-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="08612-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-salesforce-sandbox-from-hello-gallery"></a><span data-ttu-id="08612-123">Salesforce védőfal hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="08612-123">Adding Salesforce Sandbox from hello gallery</span></span>
<span data-ttu-id="08612-124">tooconfigure hello integrációja Salesforce védőfal az Azure AD-be, meg kell tooadd Salesforce védőfal hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="08612-124">tooconfigure hello integration of Salesforce Sandbox into Azure AD, you need tooadd Salesforce Sandbox from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="08612-125">**tooadd Salesforce védőfal hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="08612-125">**tooadd Salesforce Sandbox from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="08612-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="08612-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="08612-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="08612-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="08612-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="08612-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="08612-131">Kattintson a **új alkalmazás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="08612-131">Click **New application** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="08612-133">Hello keresési mezőbe, írja be a **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="08612-133">In hello search box, type **Salesforce Sandbox**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_search.png)

5. <span data-ttu-id="08612-135">A hello eredmények panelen válassza ki a **Salesforce védőfal**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="08612-135">In hello results panel, select **Salesforce Sandbox**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="08612-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="08612-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="08612-138">Ebben a szakaszban konfigurálhatja, és a vizsgálat az Azure AD egyszeri bejelentkezést a Salesforce védőfal "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="08612-138">In this section, you configure and test Azure AD single sign-on with Salesforce Sandbox based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="08612-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Salesforce védőfal tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="08612-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Salesforce Sandbox is tooa user in Azure AD.</span></span> <span data-ttu-id="08612-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello a Salesforce védőfal közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="08612-140">In other words, a link relationship between an Azure AD user and hello related user in Salesforce Sandbox needs toobe established.</span></span>

<span data-ttu-id="08612-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Salesforce védőfal.</span><span class="sxs-lookup"><span data-stu-id="08612-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Salesforce Sandbox.</span></span>

<span data-ttu-id="08612-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Salesforce védőfal-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="08612-142">tooconfigure and test Azure AD single sign-on with Salesforce Sandbox, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="08612-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="08612-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="08612-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="08612-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="08612-145">**[A Salesforce védőfal tesztfelhasználó létrehozása](#creating-a-salesforce-sandbox-test-user)**  -toohave egy megfelelője a Britta Simon a Salesforce védőfal, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="08612-145">**[Creating a Salesforce Sandbox test user](#creating-a-salesforce-sandbox-test-user)** - toohave a counterpart of Britta Simon in Salesforce Sandbox that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="08612-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="08612-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="08612-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="08612-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="08612-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="08612-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="08612-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Salesforce védőfal alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="08612-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Salesforce Sandbox application.</span></span>

<span data-ttu-id="08612-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Salesforce védőfal, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="08612-150">**tooconfigure Azure AD single sign-on with Salesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="08612-151">Az Azure portál, a hello hello **Salesforce védőfal** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="08612-151">In hello Azure portal, on hello **Salesforce Sandbox** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="08612-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="08612-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_samlbase.png)

3. <span data-ttu-id="08612-155">A hello **Salesforce védőfal tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="08612-155">On hello **Salesforce Sandbox Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_url.png)

     <span data-ttu-id="08612-157">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<subdomain>.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="08612-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<subdomain>.my.salesforce.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="08612-158">Ez az érték nincs valós hello.</span><span class="sxs-lookup"><span data-stu-id="08612-158">This value is not hello real.</span></span> <span data-ttu-id="08612-159">Ez az érték frissítése hello tényleges bejelentkezési URL-CÍMÉT. Ügyfél [Salesforce védőfal ügyfél-támogatási csoport](https://help.salesforce.com/support) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="08612-159">Update this value with hello actual Sign-on URL.Contact [Salesforce Sandbox Client support team](https://help.salesforce.com/support) tooget this value.</span></span>


4. <span data-ttu-id="08612-160">Ha már beállította egyszeri bejelentkezés Salesforce védőfal egy másik példány a könyvtárban, akkor is konfigurálnia kell hello **azonosító** toohave hello hello megegyező értékűnek **bejelentkezési URL-cím**.</span><span class="sxs-lookup"><span data-stu-id="08612-160">If you have already configured single sign-on for another Salesforce Sandbox instance in your directory, then you must also configure hello **Identifier** toohave hello same value as hello **Sign on URL**.</span></span> 
    
    >[!Note]
    ><span data-ttu-id="08612-161">Hello **azonosító** mező található hello ellenőrzésével **megjelenítése speciális beállítások** hello jelölőnégyzet **alkalmazás URL-cím konfigurálása** lap hello párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="08612-161">hello **Identifier** field can be found by checking hello **Show advanced settings** checkbox on hello **Configure App URL** page of hello dialog</span></span> 


5. <span data-ttu-id="08612-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="08612-162">On hello **SAML Signing Certificate** section, click **Certificate** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_certificate.png) 

6. <span data-ttu-id="08612-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="08612-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="08612-166">A hello **Salesforce védőfal konfigurációs** kattintson **Salesforce védőfal konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="08612-166">On hello **Salesforce Sandbox Configuration** section, click **Configure Salesforce Sandbox** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="08612-167">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="08612-167">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="08612-168">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span><span class="sxs-lookup"><span data-stu-id="08612-168">![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_configure.png) 
<CS></span></span>
8. <span data-ttu-id="08612-169">Új lap megnyitása a böngészőben, és jelentkezzen be tooyour Salesforce védőfal rendszergazdai fiókot.</span><span class="sxs-lookup"><span data-stu-id="08612-169">Open a new tab in your browser and log in tooyour Salesforce Sandbox administrator account.</span></span>

9. <span data-ttu-id="08612-170">Hello hello felső menüben kattintson a **telepítő**.</span><span class="sxs-lookup"><span data-stu-id="08612-170">In hello menu on hello top, click **Setup**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781024.png)
10. <span data-ttu-id="08612-172">Hello bal oldali hello navigációs ablaktábláján kattintson **biztonsági vezérlők**, és kattintson a **egyszeri bejelentkezési beállítások**.</span><span class="sxs-lookup"><span data-stu-id="08612-172">In hello navigation pane on hello left, click **Security Controls**, and then click **Single Sign-On Settings**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781025.png)
11. <span data-ttu-id="08612-174">Hello egyszeri bejelentkezési beállítások szakaszban, hajtsa végre a lépéseket követve hello: ![konfigurálása egyszeri bejelentkezéshez](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span><span class="sxs-lookup"><span data-stu-id="08612-174">On hello Single Sign-On Settings section, perform hello following steps:  ![Configure Single Sign-On](./media/active-directory-saas-salesforcesandbox-tutorial/IC781026.png)</span></span>
     
     <span data-ttu-id="08612-175">a.</span><span class="sxs-lookup"><span data-stu-id="08612-175">a.</span></span>  <span data-ttu-id="08612-176">Válassza ki **SAML engedélyezett**.</span><span class="sxs-lookup"><span data-stu-id="08612-176">Select **SAML Enabled**.</span></span> 

     <span data-ttu-id="08612-177">b.</span><span class="sxs-lookup"><span data-stu-id="08612-177">b.</span></span>  <span data-ttu-id="08612-178">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="08612-178">Click **New**.</span></span>

12. <span data-ttu-id="08612-179">Hello SAML-alapú egyszeri bejelentkezés beállítások szakaszban hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="08612-179">On hello SAML Single Sign-On Settings section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781027.png)

    <span data-ttu-id="08612-181">a.In hello szövegmezőben, hello konfigurációs hello nevét (pl.: *SPSSOWAAD\_teszt*).</span><span class="sxs-lookup"><span data-stu-id="08612-181">a.In hello Name textbox, type hello name of hello configuration (e.g.: *SPSSOWAAD\_Test*).</span></span> 

    <span data-ttu-id="08612-182">b.</span><span class="sxs-lookup"><span data-stu-id="08612-182">b.</span></span> <span data-ttu-id="08612-183">Beillesztés **SMAL Entitásazonosító** hello érték **kibocsátó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="08612-183">Paste **SMAL Entity ID** value into hello **Issuer** textbox.</span></span>

    <span data-ttu-id="08612-184">c.</span><span class="sxs-lookup"><span data-stu-id="08612-184">c.</span></span> <span data-ttu-id="08612-185">A hello **entitásazonosító** szövegmezőhöz típus **https://test.salesforce.com** , hogy a hozzáadandó tooyour directory első Salesforce védőfal példány hello esetén.</span><span class="sxs-lookup"><span data-stu-id="08612-185">In hello **Entity Id** textbox, type **https://test.salesforce.com** if it is hello first Salesforce Sandbox instance that you are adding tooyour directory.</span></span> <span data-ttu-id="08612-186">Ha már hozzáadott egy példányát, majd a Salesforce védőfal hello **Entitásazonosító** hello típusának **URL-cím bejelentkezési**, amely a következő formátumban kell lennie:`http://company.my.salesforce.com`</span><span class="sxs-lookup"><span data-stu-id="08612-186">If you have already added an instance of Salesforce Sandbox, then for hello **Entity ID** type in hello **Sign On URL**, which should be in this format: `http://company.my.salesforce.com`</span></span>  
 
    <span data-ttu-id="08612-187">d.</span><span class="sxs-lookup"><span data-stu-id="08612-187">d.</span></span> <span data-ttu-id="08612-188">Kattintson a **Tallózás** tooupload hello letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="08612-188">Click **Browse** tooupload hello downloaded certificate.</span></span>  

    <span data-ttu-id="08612-189">e.</span><span class="sxs-lookup"><span data-stu-id="08612-189">e.</span></span> <span data-ttu-id="08612-190">Mint **SAML identitástípus**, jelölje be **helyességi feltételt tartalmaz hello összevonási azonosító hello felhasználói objektum**.</span><span class="sxs-lookup"><span data-stu-id="08612-190">As **SAML Identity Type**, select **Assertion contains hello Federation ID from hello User object**.</span></span>
 
    <span data-ttu-id="08612-191">f.</span><span class="sxs-lookup"><span data-stu-id="08612-191">f.</span></span> <span data-ttu-id="08612-192">Mint **SAML-alapú identitás hely**, jelölje be **identitás hello tulajdonos utasítás hello NameIdentifier elemében van**.</span><span class="sxs-lookup"><span data-stu-id="08612-192">As **SAML Identity Location**, select **Identity is in hello NameIdentifier element of hello Subject statement**.</span></span>

    <span data-ttu-id="08612-193">g.</span><span class="sxs-lookup"><span data-stu-id="08612-193">g.</span></span> <span data-ttu-id="08612-194">Beillesztés **egyszeri bejelentkezési URL-címe** történő hello **Identity Provider bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="08612-194">Paste **Single Sign-On Service URL** into hello **Identity Provider Login URL** textbox.</span></span> 

    <span data-ttu-id="08612-195">h.</span><span class="sxs-lookup"><span data-stu-id="08612-195">h.</span></span> <span data-ttu-id="08612-196">SFDC nem támogatja az SAML jelentkezzen ki.</span><span class="sxs-lookup"><span data-stu-id="08612-196">SFDC does not support SAML logout.</span></span>  <span data-ttu-id="08612-197">A probléma megoldásához, illessze be a "https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0" hello be azt **Identity Provider kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="08612-197">As a workaround, paste 'https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0' it into hello **Identity Provider Logout URL** textbox.</span></span>

    <span data-ttu-id="08612-198">i.</span><span class="sxs-lookup"><span data-stu-id="08612-198">i.</span></span> <span data-ttu-id="08612-199">Mint **szolgáltató által kezdeményezett kérelem Szolgáltatáskötés**, jelölje be **HTTP POST**.</span><span class="sxs-lookup"><span data-stu-id="08612-199">As **Service Provider Initiated Request Binding**, select **HTTP POST**.</span></span> 

    <span data-ttu-id="08612-200">j.</span><span class="sxs-lookup"><span data-stu-id="08612-200">j.</span></span> <span data-ttu-id="08612-201">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="08612-201">Click **Save**.</span></span>

### <a name="enable-your-domain"></a><span data-ttu-id="08612-202">A tartomány</span><span class="sxs-lookup"><span data-stu-id="08612-202">Enable your domain</span></span>
<span data-ttu-id="08612-203">Jelen szakaszban feltételezzük, hogy már létrehozta a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="08612-203">This section assumes that you already have created a domain.</span></span>  <span data-ttu-id="08612-204">További információkért lásd: [meghatározása saját tartomány neve](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span><span class="sxs-lookup"><span data-stu-id="08612-204">For more information, see [Defining Your Domain Name](https://help.salesforce.com/HTViewHelpDoc?id=domain_name_define.htm&language=en_US).</span></span>

<span data-ttu-id="08612-205">**tooenable a tartományba, hajtsa végre az alábbi lépésekkel hello:**</span><span class="sxs-lookup"><span data-stu-id="08612-205">**tooenable your domain, perform hello following steps:**</span></span>

1. <span data-ttu-id="08612-206">Hello bal oldali navigációs ablaktábláján kattintson **tartományok**, és kattintson a **saját tartomány.**</span><span class="sxs-lookup"><span data-stu-id="08612-206">In hello left navigation pane, click **Domain Management**, and then click **My Domain.**</span></span>
   
     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781029.png)
   
   >[!NOTE]
   ><span data-ttu-id="08612-208">Győződjön meg arról, hogy a tartomány megfelelően van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="08612-208">Please make sure that your domain has been configured correctly.</span></span> 

2. <span data-ttu-id="08612-209">A hello **bejelentkezési lap beállításai** területen kattintson **szerkesztése**, később, **hitelesítési szolgáltatás**, jelölje be az előző hello SAML-alapú egyszeri bejelentkezés beállítása hello hello neve szakaszt, és végül kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="08612-209">In hello **Login Page Settings** section, click **Edit**, then, as **Authentication Service**, select hello name of hello SAML Single Sign-On Setting from hello previous section, and finally click **Save**.</span></span>
   
   ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/IC781030.png)

<span data-ttu-id="08612-211">Amint egy tartományhoz, konfigurálva van, a felhasználók hello tartomány URL-cím toologin toohello Salesforce védőfal használja.</span><span class="sxs-lookup"><span data-stu-id="08612-211">As soon as you have a domain configured, your users should use hello domain URL toologin toohello Salesforce sandbox.</span></span>  

<span data-ttu-id="08612-212">tooget hello értékének hello URL-t, kattintson a hello egyszeri bejelentkezési profil hello előző szakaszban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="08612-212">tooget hello value of hello URL, click hello SSO profile you have created in hello previous section.</span></span>    

> [!TIP]
> <span data-ttu-id="08612-213">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="08612-213">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="08612-214">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="08612-214">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="08612-215">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="08612-215">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="08612-216">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08612-216">Creating an Azure AD test user</span></span>
<span data-ttu-id="08612-217">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="08612-217">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="08612-219">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="08612-219">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="08612-220">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="08612-220">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="08612-222">felhasználók toodisplay hello listája túl Ugrás**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="08612-222">toodisplay hello list of users go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="08612-224">Hello hello párbeszédpanel, kattintson tetején **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08612-224">At hello top of hello dialog, click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="08612-226">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="08612-226">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-salesforcesandbox-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="08612-228">a.</span><span class="sxs-lookup"><span data-stu-id="08612-228">a.</span></span> <span data-ttu-id="08612-229">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="08612-229">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="08612-230">b.</span><span class="sxs-lookup"><span data-stu-id="08612-230">b.</span></span> <span data-ttu-id="08612-231">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="08612-231">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="08612-232">c.</span><span class="sxs-lookup"><span data-stu-id="08612-232">c.</span></span> <span data-ttu-id="08612-233">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="08612-233">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="08612-234">d.</span><span class="sxs-lookup"><span data-stu-id="08612-234">d.</span></span> <span data-ttu-id="08612-235">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="08612-235">Click **Create**.</span></span>
 
### <a name="creating-a-salesforce-sandbox-test-user"></a><span data-ttu-id="08612-236">A Salesforce védőfal tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="08612-236">Creating a Salesforce Sandbox test user</span></span>

<span data-ttu-id="08612-237">Ebben a szakaszban egy felhasználó Britta Simon nevű Salesforce védőfal jön létre.</span><span class="sxs-lookup"><span data-stu-id="08612-237">In this section, a user called Britta Simon is created in Salesforce Sandbox.</span></span> <span data-ttu-id="08612-238">Salesforce védőfal támogatja közvetlenül az időponthoz kötött kiosztást, amely alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="08612-238">Salesforce Sandbox supports just-in-time provisioning, which is enabled by default.</span></span>
<span data-ttu-id="08612-239">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="08612-239">There is no action item for you in this section.</span></span> <span data-ttu-id="08612-240">Ha a felhasználó nem létezik a Salesforce védőfal, egy új tooaccess Salesforce védőfal tett kísérlet során jön létre.</span><span class="sxs-lookup"><span data-stu-id="08612-240">If a user doesn't already exist in Salesforce Sandbox, a new one is created when you attempt tooaccess Salesforce Sandbox.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="08612-241">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="08612-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="08612-242">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSalesforce védőfal megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="08612-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSalesforce Sandbox.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="08612-244">**tooassign Britta Simon tooSalesforce védőfal, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="08612-244">**tooassign Britta Simon tooSalesforce Sandbox, perform hello following steps:**</span></span>

1. <span data-ttu-id="08612-245">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="08612-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="08612-247">Hello alkalmazások listában válassza ki a **Salesforce védőfal**.</span><span class="sxs-lookup"><span data-stu-id="08612-247">In hello applications list, select **Salesforce Sandbox**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_salesforcesandbox_app.png) 

3. <span data-ttu-id="08612-249">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="08612-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="08612-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="08612-251">Click **Add** button.</span></span> <span data-ttu-id="08612-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08612-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="08612-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="08612-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="08612-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08612-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="08612-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="08612-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="08612-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="08612-257">Testing single sign-on</span></span>

<span data-ttu-id="08612-258">Ha tootest az egyszeri Bejelentkezést a beállításokat, nyissa meg a hozzáférési Panel hello.</span><span class="sxs-lookup"><span data-stu-id="08612-258">If you want tootest your SSO settings, open hello Access Panel.</span></span> <span data-ttu-id="08612-259">Hozzáférési Panel hello kapcsolatos további tudnivalókért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="08612-259">For more details about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08612-260">További források</span><span class="sxs-lookup"><span data-stu-id="08612-260">Additional resources</span></span>

* [<span data-ttu-id="08612-261">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="08612-261">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="08612-262">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="08612-262">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)
* [<span data-ttu-id="08612-263">A felhasználók átadása konfigurálása</span><span class="sxs-lookup"><span data-stu-id="08612-263">Configure User Provisioning</span></span>](active-directory-saas-salesforce-sandbox-provisioning-tutorial.md)



<!--Image references-->

[1]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-salesforcesandbox-tutorial/tutorial_general_203.png

