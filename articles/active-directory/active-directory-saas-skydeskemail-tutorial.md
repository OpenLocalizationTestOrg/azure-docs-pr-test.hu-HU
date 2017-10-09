---
title: "Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SkyDesk E-mail között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9d0bbcb-ddb5-473f-a4aa-028ae88ced1a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 19c670a60f581a2be55b74eacdb5393a36e38be3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-skydesk-email"></a><span data-ttu-id="25165-103">Oktatóanyag: Azure Active Directory-integráció SkyDesk e-mail</span><span class="sxs-lookup"><span data-stu-id="25165-103">Tutorial: Azure Active Directory integration with SkyDesk Email</span></span>

<span data-ttu-id="25165-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SkyDesk e-mailek Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="25165-104">In this tutorial, you learn how toointegrate SkyDesk Email with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="25165-105">SkyDesk E-mail integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="25165-105">Integrating SkyDesk Email with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="25165-106">Megadhatja a hozzáférés tooSkyDesk E-mail rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="25165-106">You can control in Azure AD who has access tooSkyDesk Email</span></span>
- <span data-ttu-id="25165-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSkyDesk E-mail (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="25165-107">You can enable your users tooautomatically get signed-on tooSkyDesk Email (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="25165-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="25165-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="25165-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="25165-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="25165-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="25165-110">Prerequisites</span></span>

<span data-ttu-id="25165-111">tooconfigure SkyDesk e-mailek az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="25165-111">tooconfigure Azure AD integration with SkyDesk Email, you need hello following items:</span></span>

- <span data-ttu-id="25165-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="25165-112">An Azure AD subscription</span></span>
- <span data-ttu-id="25165-113">Egy e-mailek SkyDesk egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="25165-113">A SkyDesk Email single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="25165-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="25165-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="25165-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="25165-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="25165-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="25165-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="25165-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="25165-117">If you don't have an Azure AD trial environment, you can get a one-month trial here [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="25165-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="25165-118">Scenario description</span></span>
<span data-ttu-id="25165-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="25165-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="25165-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="25165-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="25165-121">Hello gyűjteményből SkyDesk E-mail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="25165-121">Adding SkyDesk Email from hello gallery</span></span>
2. <span data-ttu-id="25165-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="25165-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-skydesk-email-from-hello-gallery"></a><span data-ttu-id="25165-123">Hello gyűjteményből SkyDesk E-mail hozzáadása</span><span class="sxs-lookup"><span data-stu-id="25165-123">Adding SkyDesk Email from hello gallery</span></span>
<span data-ttu-id="25165-124">tooconfigure hello integrációs SkyDesk e-mailek, az Azure AD-be, meg kell tooadd SkyDesk E-mail hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="25165-124">tooconfigure hello integration of SkyDesk Email into Azure AD, you need tooadd SkyDesk Email from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="25165-125">**tooadd SkyDesk E-mail hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="25165-125">**tooadd SkyDesk Email from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="25165-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="25165-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="25165-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="25165-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="25165-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="25165-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="25165-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="25165-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="25165-133">Hello keresési mezőbe, írja be a **SkyDesk E-mail**.</span><span class="sxs-lookup"><span data-stu-id="25165-133">In hello search box, type **SkyDesk Email**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_search.png)

5. <span data-ttu-id="25165-135">A hello eredmények panelen válassza a **SkyDesk E-mail**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="25165-135">In hello results panel, select **SkyDesk Email**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="25165-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="25165-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="25165-138">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés SkyDesk e-mail "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="25165-138">In this section, you configure and test Azure AD single sign-on with SkyDesk Email based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="25165-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói SkyDesk e-mailben az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="25165-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SkyDesk Email is tooa user in Azure AD.</span></span> <span data-ttu-id="25165-140">Ez azt jelenti közötti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello SkyDesk e-mailben szereplő hivatkozásra kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="25165-140">In other words, a link relationship between an Azure AD user and hello related user in SkyDesk Email needs toobe established.</span></span>

<span data-ttu-id="25165-141">SkyDesk e-mailben, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="25165-141">In SkyDesk Email, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="25165-142">tooconfigure és az Azure AD az egyszeri bejelentkezés SkyDesk E-mail-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="25165-142">tooconfigure and test Azure AD single sign-on with SkyDesk Email, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="25165-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="25165-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="25165-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="25165-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="25165-145">**[SkyDesk E-mail tesztfelhasználó létrehozása](#creating-a-skydesk-email-test-user)**  -toohave egy megfelelője a Britta Simon csatolt toohello az Azure AD felhasználói ábrázolása SkyDesk e-mailben.</span><span class="sxs-lookup"><span data-stu-id="25165-145">**[Creating a SkyDesk Email test user](#creating-a-skydesk-email-test-user)** - toohave a counterpart of Britta Simon in SkyDesk Email that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="25165-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="25165-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="25165-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="25165-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="25165-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="25165-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="25165-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a SkyDesk levelezőprogramban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="25165-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SkyDesk Email application.</span></span>

<span data-ttu-id="25165-150">**az Azure AD tooconfigure egyszeri bejelentkezés SkyDesk e-mail, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="25165-150">**tooconfigure Azure AD single sign-on with SkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="25165-151">Az Azure portál, a hello hello **SkyDesk E-mail** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="25165-151">In hello Azure portal, on hello **SkyDesk Email** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="25165-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="25165-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_samlbase.png)

3. <span data-ttu-id="25165-155">A hello **SkyDesk E-mail tartománya és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="25165-155">On hello **SkyDesk Email Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_url.png)

    <span data-ttu-id="25165-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://mail.skydesk.jp/portal/<companyname>`</span><span class="sxs-lookup"><span data-stu-id="25165-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://mail.skydesk.jp/portal/<companyname>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="25165-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="25165-158">hello value is not real.</span></span> <span data-ttu-id="25165-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="25165-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="25165-160">Ügyfél [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="25165-160">Contact [SkyDesk Email Client support team](https://www.skydesk.sg/support/) tooget hello value.</span></span> 
 
4. <span data-ttu-id="25165-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="25165-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_certificate.png) 

5. <span data-ttu-id="25165-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="25165-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="25165-165">A hello **SkyDesk E-mail-konfigurációjának** kattintson **konfigurálása SkyDesk E-mail** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="25165-165">On hello **SkyDesk Email Configuration** section, click **Configure SkyDesk Email** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="25165-166">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="25165-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_configure.png) 

7. <span data-ttu-id="25165-168">egyszeri bejelentkezés tooenable a **SkyDesk E-mail**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="25165-168">tooenable SSO in **SkyDesk Email**, perform hello following steps:</span></span>

    <span data-ttu-id="25165-169">a.</span><span class="sxs-lookup"><span data-stu-id="25165-169">a.</span></span> <span data-ttu-id="25165-170">Bejelentkezés tooyour SkyDesk E-mail fiók rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="25165-170">Sign-on tooyour SkyDesk Email account as administrator.</span></span>

    <span data-ttu-id="25165-171">b.</span><span class="sxs-lookup"><span data-stu-id="25165-171">b.</span></span> <span data-ttu-id="25165-172">Hello hello felső menüben kattintson a **telepítő**, és válassza ki **szervezeti**.</span><span class="sxs-lookup"><span data-stu-id="25165-172">In hello menu on hello top, click **Setup**, and select **Org**.</span></span> 
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_51.png)
  
    <span data-ttu-id="25165-174">c.</span><span class="sxs-lookup"><span data-stu-id="25165-174">c.</span></span> <span data-ttu-id="25165-175">Kattintson a **tartományok** hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="25165-175">Click on **Domains** from hello left panel.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_53.png)

    <span data-ttu-id="25165-177">d.</span><span class="sxs-lookup"><span data-stu-id="25165-177">d.</span></span> <span data-ttu-id="25165-178">Kattintson a **tartomány hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="25165-178">Click on **Add Domain**.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_54.png)

    <span data-ttu-id="25165-180">e.</span><span class="sxs-lookup"><span data-stu-id="25165-180">e.</span></span> <span data-ttu-id="25165-181">Adja meg a tartomány nevét, és ellenőrizze a tartomány hello.</span><span class="sxs-lookup"><span data-stu-id="25165-181">Enter your Domain name, and then verify hello Domain.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_55.png)

    <span data-ttu-id="25165-183">f.</span><span class="sxs-lookup"><span data-stu-id="25165-183">f.</span></span> <span data-ttu-id="25165-184">Kattintson a **SAML-alapú hitelesítés** hello bal oldali panelen.</span><span class="sxs-lookup"><span data-stu-id="25165-184">Click on **SAML Authentication** from hello left panel.</span></span>
    
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_52.png)

8. <span data-ttu-id="25165-186">A hello **SAML-alapú hitelesítés** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="25165-186">On hello **SAML Authentication** dialog page, perform hello following steps:</span></span>
   
      ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_56.png)
   
    >[!NOTE]
    ><span data-ttu-id="25165-188">toouse SAML-alapú hitelesítést, vagy rendelkeznie kell **ellenőrzött tartományához** vagy **portál URL-cím** beállítása.</span><span class="sxs-lookup"><span data-stu-id="25165-188">toouse SAML based authentication, you should either have **verified domain** or **portal URL** setup.</span></span> <span data-ttu-id="25165-189">Beállíthat hello portál URL-cím elé hello egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="25165-189">You can set hello portal URL with hello unique name.</span></span>
    > 
    > 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_57.png)

    <span data-ttu-id="25165-191">a.</span><span class="sxs-lookup"><span data-stu-id="25165-191">a.</span></span> <span data-ttu-id="25165-192">A hello **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="25165-192">In hello **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="25165-193">b.</span><span class="sxs-lookup"><span data-stu-id="25165-193">b.</span></span> <span data-ttu-id="25165-194">A hello **kijelentkezési** URL-cím beviteli mező, Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="25165-194">In hello **Logout** URL textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="25165-195">c.</span><span class="sxs-lookup"><span data-stu-id="25165-195">c.</span></span> <span data-ttu-id="25165-196">**Módosítsa jelszavát URL-címet** nem kötelező megadni, hagyja üresen a mezőt.</span><span class="sxs-lookup"><span data-stu-id="25165-196">**Change Password URL** is optional so leave it blank.</span></span>

    <span data-ttu-id="25165-197">d.</span><span class="sxs-lookup"><span data-stu-id="25165-197">d.</span></span> <span data-ttu-id="25165-198">Kattintson a **fájl kulcs beszerzése** tooselect az Azure-portálon, és kattintson a letöltött tanúsítvány **nyitott** tooupload hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="25165-198">Click on **Get Key From File** tooselect your downloaded certificate from Azure portal, and then click **Open** tooupload hello certificate.</span></span>

    <span data-ttu-id="25165-199">e.</span><span class="sxs-lookup"><span data-stu-id="25165-199">e.</span></span> <span data-ttu-id="25165-200">Mint **algoritmus**, jelölje be **RSA**.</span><span class="sxs-lookup"><span data-stu-id="25165-200">As **Algorithm**, select **RSA**.</span></span>

    <span data-ttu-id="25165-201">f.</span><span class="sxs-lookup"><span data-stu-id="25165-201">f.</span></span> <span data-ttu-id="25165-202">Kattintson a **Ok** toosave hello módosításokat.</span><span class="sxs-lookup"><span data-stu-id="25165-202">Click **Ok** toosave hello changes.</span></span>

> [!TIP]
> <span data-ttu-id="25165-203">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="25165-203">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="25165-204">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="25165-204">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="25165-205">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="25165-205">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="25165-206">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="25165-206">Creating an Azure AD test user</span></span>
<span data-ttu-id="25165-207">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="25165-207">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="25165-209">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="25165-209">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="25165-210">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="25165-210">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="25165-212">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="25165-212">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="25165-214">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="25165-214">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="25165-216">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="25165-216">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-skydeskemail-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="25165-218">a.</span><span class="sxs-lookup"><span data-stu-id="25165-218">a.</span></span> <span data-ttu-id="25165-219">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="25165-219">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="25165-220">b.</span><span class="sxs-lookup"><span data-stu-id="25165-220">b.</span></span> <span data-ttu-id="25165-221">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="25165-221">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="25165-222">c.</span><span class="sxs-lookup"><span data-stu-id="25165-222">c.</span></span> <span data-ttu-id="25165-223">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="25165-223">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="25165-224">d.</span><span class="sxs-lookup"><span data-stu-id="25165-224">d.</span></span> <span data-ttu-id="25165-225">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="25165-225">Click **Create**.</span></span>
 
### <a name="creating-a-skydesk-email-test-user"></a><span data-ttu-id="25165-226">SkyDesk E-mail tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="25165-226">Creating a SkyDesk Email test user</span></span>

<span data-ttu-id="25165-227">Ebben a szakaszban hoz létre a felhasználó Britta Simon nevű SkyDesk e-mailben.</span><span class="sxs-lookup"><span data-stu-id="25165-227">In this section, you create a user called Britta Simon in SkyDesk Email.</span></span>

1. <span data-ttu-id="25165-228">Kattintson a **felhasználói hozzáférés** hello bal oldali panelen SkyDesk e-mailben, és írja be a felhasználónevet.</span><span class="sxs-lookup"><span data-stu-id="25165-228">Click on **User Access** from hello left panel in SkyDesk Email and then enter your username.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_58.png)

>[!NOTE] 
><span data-ttu-id="25165-230">Toocreate tömeges felhasználók van szüksége, ha szüksége van-e toocontact hello [SkyDesk levelezőprogram támogatási csoport](https://www.skydesk.sg/support/).</span><span class="sxs-lookup"><span data-stu-id="25165-230">If you need toocreate bulk users, you need toocontact hello [SkyDesk Email Client support team](https://www.skydesk.sg/support/).</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="25165-231">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="25165-231">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="25165-232">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés E-mail hozzáférés tooSkyDesk megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="25165-232">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSkyDesk Email.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="25165-234">**tooassign Britta Simon tooSkyDesk e-mailek, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="25165-234">**tooassign Britta Simon tooSkyDesk Email, perform hello following steps:**</span></span>

1. <span data-ttu-id="25165-235">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="25165-235">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="25165-237">Hello alkalmazások listában válassza ki a **SkyDesk E-mail**.</span><span class="sxs-lookup"><span data-stu-id="25165-237">In hello applications list, select **SkyDesk Email**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-skydeskemail-tutorial/tutorial_skydeskemail_app.png) 

3. <span data-ttu-id="25165-239">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="25165-239">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="25165-241">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="25165-241">Click **Add** button.</span></span> <span data-ttu-id="25165-242">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="25165-242">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="25165-244">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="25165-244">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="25165-245">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="25165-245">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="25165-246">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="25165-246">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="25165-247">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="25165-247">Testing single sign-on</span></span>

<span data-ttu-id="25165-248">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="25165-248">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="25165-249">Hello SkyDesk e-mailek hozzáférési Panel hello csempére kattintva kapja meg automatikusan bejelentkezett tooyour SkyDesk E-mail alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="25165-249">When you click hello SkyDesk Email tile in hello Access Panel, you should get automatically signed-on tooyour SkyDesk Email application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="25165-250">További források</span><span class="sxs-lookup"><span data-stu-id="25165-250">Additional resources</span></span>

* [<span data-ttu-id="25165-251">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="25165-251">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="25165-252">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="25165-252">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-skydeskemail-tutorial/tutorial_general_203.png

