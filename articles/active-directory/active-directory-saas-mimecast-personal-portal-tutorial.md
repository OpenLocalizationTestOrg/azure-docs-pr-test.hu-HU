---
title: "Oktatóanyag: Azure Active Directory-integráció Mimecast személyes portállal |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a személyes Portal Mimecast között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 345b22be-d87e-45a4-b4c0-70a67eaf9bfd
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: ee2a8edcab36f295732ac1ebe641ed7fcfc1f2a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-personal-portal"></a><span data-ttu-id="8f20f-103">Oktatóanyag: Azure Active Directory-integráció Mimecast személyes portállal</span><span class="sxs-lookup"><span data-stu-id="8f20f-103">Tutorial: Azure Active Directory integration with Mimecast Personal Portal</span></span>

<span data-ttu-id="8f20f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mimecast személyes portálon az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8f20f-104">In this tutorial, you learn how toointegrate Mimecast Personal Portal with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8f20f-105">Mimecast személyes Portal integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8f20f-105">Integrating Mimecast Personal Portal with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8f20f-106">Megadhatja a hozzáférés tooMimecast személyes Portal rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8f20f-106">You can control in Azure AD who has access tooMimecast Personal Portal</span></span>
- <span data-ttu-id="8f20f-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMimecast személyes portál (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="8f20f-107">You can enable your users tooautomatically get signed-on tooMimecast Personal Portal (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8f20f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8f20f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8f20f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8f20f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8f20f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8f20f-110">Prerequisites</span></span>

<span data-ttu-id="8f20f-111">tooconfigure Mimecast személyes portálon az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="8f20f-111">tooconfigure Azure AD integration with Mimecast Personal Portal, you need hello following items:</span></span>

- <span data-ttu-id="8f20f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8f20f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8f20f-113">Egy Mimecast személyes Portal egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="8f20f-113">A Mimecast Personal Portal single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8f20f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8f20f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8f20f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8f20f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8f20f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8f20f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8f20f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8f20f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8f20f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8f20f-118">Scenario description</span></span>
<span data-ttu-id="8f20f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8f20f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8f20f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8f20f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8f20f-121">Mimecast személyes portál hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8f20f-121">Adding Mimecast Personal Portal from hello gallery</span></span>
2. <span data-ttu-id="8f20f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8f20f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-personal-portal-from-hello-gallery"></a><span data-ttu-id="8f20f-123">Mimecast személyes portál hozzáadását hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="8f20f-123">Adding Mimecast Personal Portal from hello gallery</span></span>
<span data-ttu-id="8f20f-124">tooconfigure hello integrációs Mimecast személyes portál, az Azure AD-be, meg kell tooadd Mimecast személyes Portal hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8f20f-124">tooconfigure hello integration of Mimecast Personal Portal into Azure AD, you need tooadd Mimecast Personal Portal from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8f20f-125">**tooadd Mimecast személyes Portal hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8f20f-125">**tooadd Mimecast Personal Portal from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f20f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8f20f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8f20f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8f20f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="8f20f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8f20f-133">Hello keresési mezőbe, írja be a **Mimecast személyes Portal**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-133">In hello search box, type **Mimecast Personal Portal**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_search.png)

5. <span data-ttu-id="8f20f-135">A hello eredmények panelen válassza a **Mimecast személyes Portal**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-135">In hello results panel, select **Mimecast Personal Portal**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8f20f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8f20f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8f20f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Mimecast személyes Portal "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="8f20f-138">In this section, you configure and test Azure AD single sign-on with Mimecast Personal Portal based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="8f20f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Mimecast személyes portálon tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8f20f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Personal Portal is tooa user in Azure AD.</span></span> <span data-ttu-id="8f20f-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Mimecast személyes portálon közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="8f20f-140">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Personal Portal needs toobe established.</span></span>

<span data-ttu-id="8f20f-141">A személyes portál Mimecast, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="8f20f-141">In Mimecast Personal Portal, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="8f20f-142">tooconfigure és Mimecast személyes portálon az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8f20f-142">tooconfigure and test Azure AD single sign-on with Mimecast Personal Portal, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8f20f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8f20f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8f20f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8f20f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8f20f-145">**[Mimecast személyes Portal tesztfelhasználó létrehozása](#creating-a-mimecast-personal-portal-test-user)**  -toohave egy megfelelője a Britta Simon Mimecast személyes portálon, hogy a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="8f20f-145">**[Creating a Mimecast Personal Portal test user](#creating-a-mimecast-personal-portal-test-user)** - toohave a counterpart of Britta Simon in Mimecast Personal Portal that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8f20f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8f20f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8f20f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8f20f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8f20f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8f20f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8f20f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Mimecast személyes portál alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8f20f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Personal Portal application.</span></span>

<span data-ttu-id="8f20f-150">**az Azure AD az egyszeri bejelentkezés tooconfigure Mimecast személyes portállal, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="8f20f-150">**tooconfigure Azure AD single sign-on with Mimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f20f-151">Az Azure portál, a hello hello **Mimecast személyes Portal** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-151">In hello Azure portal, on hello **Mimecast Personal Portal** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8f20f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8f20f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_samlbase.png)

3. <span data-ttu-id="8f20f-155">A hello **Mimecast személyes Portal tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8f20f-155">On hello **Mimecast Personal Portal Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_url.png)

    <span data-ttu-id="8f20f-157">a.</span><span class="sxs-lookup"><span data-stu-id="8f20f-157">a.</span></span> <span data-ttu-id="8f20f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="8f20f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern:</span></span> 
    | |     
    | ----------------------------------------|
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|
    | |
   
    <span data-ttu-id="8f20f-159">b.</span><span class="sxs-lookup"><span data-stu-id="8f20f-159">b.</span></span> <span data-ttu-id="8f20f-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="8f20f-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>

    | |     
    | --- |
    | `https://webmail-us.mimecast.com/sso/<companyname>`|
    | `https://webmail-uk.mimecast.com/sso/<companyname>`|    
    | `https://webmail-za.mimecast.com/sso/<companyname>`|
    | `https://webmail.mimecast-offshore.com/sso/<companyname>`|
    ||                                                 
    
    > [!NOTE] 
    > <span data-ttu-id="8f20f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="8f20f-161">These values are not real.</span></span> <span data-ttu-id="8f20f-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="8f20f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="8f20f-163">Ügyfél [Mimecast személyes portál ügyfél-támogatási csoport](https://www.mimecast.com/customer-success/technical-support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="8f20f-163">Contact [Mimecast Personal Portal Client support team](https://www.mimecast.com/customer-success/technical-support/) tooget these values.</span></span> 
 


4. <span data-ttu-id="8f20f-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8f20f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_certificate.png) 

5. <span data-ttu-id="8f20f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8f20f-168">A hello **Mimecast személyes portál konfigurációs** kattintson **Mimecast személyes portál konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="8f20f-168">On hello **Mimecast Personal Portal Configuration** section, click **Configure Mimecast Personal Portal** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="8f20f-169">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="8f20f-169">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_configure.png) 

7. <span data-ttu-id="8f20f-171">Egy másik webes böngészőablakban jelentkezzen be a Mimecast személyes portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f20f-171">In a different web browser window, log into your Mimecast Personal Portal as an administrator.</span></span>

8. <span data-ttu-id="8f20f-172">Nyissa meg túl**szolgáltatások \> alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-172">Go too**Services \> Applications**.</span></span>
   
    <span data-ttu-id="8f20f-173">![Alkalmazások](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "alkalmazások")</span><span class="sxs-lookup"><span data-stu-id="8f20f-173">![Applications](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794998.png "Applications")</span></span>

9. <span data-ttu-id="8f20f-174">Kattintson a **hitelesítési profilok**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-174">Click **Authentication Profiles**.</span></span>
   
    <span data-ttu-id="8f20f-175">![Hitelesítési profilok](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="8f20f-175">![Authentication Profiles](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic794999.png "Authentication Profiles")</span></span>

10. <span data-ttu-id="8f20f-176">Kattintson a **új hitelesítési profil**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-176">Click **New Authentication Profile**.</span></span>
   
    <span data-ttu-id="8f20f-177">![Új hitelesítési profil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "új hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="8f20f-177">![New Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795000.png "New Authentication Profile")</span></span>

11. <span data-ttu-id="8f20f-178">A hello **hitelesítési profil** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8f20f-178">In hello **Authentication Profile** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="8f20f-179">![Hitelesítési profil](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="8f20f-179">![Authentication Profile](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795001.png "Authentication Profile")</span></span>
   
    <span data-ttu-id="8f20f-180">a.</span><span class="sxs-lookup"><span data-stu-id="8f20f-180">a.</span></span> <span data-ttu-id="8f20f-181">A hello **leírás** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="8f20f-181">In hello **Description** textbox, type a name for your configuration.</span></span>
   
    <span data-ttu-id="8f20f-182">b.</span><span class="sxs-lookup"><span data-stu-id="8f20f-182">b.</span></span> <span data-ttu-id="8f20f-183">Válassza ki **Mimecast személyes portál SAML-alapú hitelesítés kényszerítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-183">Select **Enforce SAML Authentication for Mimecast Personal Portal**.</span></span>
   
    <span data-ttu-id="8f20f-184">c.</span><span class="sxs-lookup"><span data-stu-id="8f20f-184">c.</span></span> <span data-ttu-id="8f20f-185">Mint **szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-185">As **Provider**, select **Azure Active Directory**.</span></span>
   
    <span data-ttu-id="8f20f-186">d.</span><span class="sxs-lookup"><span data-stu-id="8f20f-186">d.</span></span> <span data-ttu-id="8f20f-187">A **kiállítójának URL-címe** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8f20f-187">In **Issuer URL** textbox, paste hello value of **SAML Entity ID** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="8f20f-188">e.</span><span class="sxs-lookup"><span data-stu-id="8f20f-188">e.</span></span> <span data-ttu-id="8f20f-189">A **bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8f20f-189">In **Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="8f20f-190">f.</span><span class="sxs-lookup"><span data-stu-id="8f20f-190">f.</span></span> <span data-ttu-id="8f20f-191">A **kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="8f20f-191">In **Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="8f20f-192">g.</span><span class="sxs-lookup"><span data-stu-id="8f20f-192">g.</span></span> <span data-ttu-id="8f20f-193">Nyissa meg a **base-64** kódolású tanúsítvány a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **Identitástanúsítvány szolgáltató (Metadata)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="8f20f-193">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>

    <span data-ttu-id="8f20f-194">h.</span><span class="sxs-lookup"><span data-stu-id="8f20f-194">h.</span></span> <span data-ttu-id="8f20f-195">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-195">Select **Allow Single Sign On**.</span></span>
   
    <span data-ttu-id="8f20f-196">i.</span><span class="sxs-lookup"><span data-stu-id="8f20f-196">i.</span></span> <span data-ttu-id="8f20f-197">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-197">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="8f20f-198">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8f20f-198">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8f20f-199">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8f20f-199">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8f20f-200">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8f20f-200">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8f20f-201">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f20f-201">Creating an Azure AD test user</span></span>
<span data-ttu-id="8f20f-202">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8f20f-202">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8f20f-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8f20f-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f20f-205">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-205">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8f20f-207">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-207">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8f20f-209">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f20f-209">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8f20f-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8f20f-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mimecast-personal-portal-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8f20f-213">a.</span><span class="sxs-lookup"><span data-stu-id="8f20f-213">a.</span></span> <span data-ttu-id="8f20f-214">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-214">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8f20f-215">b.</span><span class="sxs-lookup"><span data-stu-id="8f20f-215">b.</span></span> <span data-ttu-id="8f20f-216">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="8f20f-216">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="8f20f-217">c.</span><span class="sxs-lookup"><span data-stu-id="8f20f-217">c.</span></span> <span data-ttu-id="8f20f-218">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-218">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8f20f-219">d.</span><span class="sxs-lookup"><span data-stu-id="8f20f-219">d.</span></span> <span data-ttu-id="8f20f-220">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-220">Click **Create**.</span></span>
 
### <a name="creating-a-mimecast-personal-portal-test-user"></a><span data-ttu-id="8f20f-221">Mimecast személyes Portal tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8f20f-221">Creating a Mimecast Personal Portal test user</span></span>

<span data-ttu-id="8f20f-222">A sorrend tooenable az Azure AD felhasználók toolog Mimecast személyes portált akkor ki kell építenie Mimecast személyes portált.</span><span class="sxs-lookup"><span data-stu-id="8f20f-222">In order tooenable Azure AD users toolog into Mimecast Personal Portal, they must be provisioned into Mimecast Personal Portal.</span></span> <span data-ttu-id="8f20f-223">Mimecast személyes Portal hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8f20f-223">In hello case of Mimecast Personal Portal, provisioning is a manual task.</span></span>

<span data-ttu-id="8f20f-224">Felhasználók létrehozása előtt kell tooregister egy tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="8f20f-224">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="8f20f-225">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8f20f-225">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f20f-226">Jelentkezzen be tooyour **Mimecast személyes Portal** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8f20f-226">Sign on tooyour **Mimecast Personal Portal** as administrator.</span></span>

2. <span data-ttu-id="8f20f-227">Nyissa meg túl**könyvtárak \> belső**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-227">Go too**Directories \> Internal**.</span></span>
   
    <span data-ttu-id="8f20f-228">![Könyvtárak](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "könyvtárak")</span><span class="sxs-lookup"><span data-stu-id="8f20f-228">![Directories](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795003.png "Directories")</span></span>

3. <span data-ttu-id="8f20f-229">Kattintson a **regisztrálni az új tartomány**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-229">Click **Register New Domain**.</span></span>
   
    <span data-ttu-id="8f20f-230">![Új tartomány regisztrálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "új tartomány regisztrálása")</span><span class="sxs-lookup"><span data-stu-id="8f20f-230">![Register New Domain](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795004.png "Register New Domain")</span></span>

4. <span data-ttu-id="8f20f-231">Az új tartomány létrehozása után kattintson **új cím**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-231">After your new domain has been created, click **New Address**.</span></span>
   
    <span data-ttu-id="8f20f-232">![Új cím](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "új cím")</span><span class="sxs-lookup"><span data-stu-id="8f20f-232">![New Address](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795005.png "New Address")</span></span>

5. <span data-ttu-id="8f20f-233">Hello új cím párbeszédpanelen hajtsa végre a következő lépéseket egy érvényes Azure hello tooprovision kívánt AD-fiókot:</span><span class="sxs-lookup"><span data-stu-id="8f20f-233">In hello new address dialog, perform hello following steps of a valid Azure AD account you want tooprovision:</span></span>
   
    <span data-ttu-id="8f20f-234">![Mentés](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="8f20f-234">![Save](./media/active-directory-saas-mimecast-personal-portal-tutorial/ic795006.png "Save")</span></span>
   
    <span data-ttu-id="8f20f-235">a.</span><span class="sxs-lookup"><span data-stu-id="8f20f-235">a.</span></span> <span data-ttu-id="8f20f-236">A hello **E-mail cím** szövegmezőhöz típus **E-mail cím** hello felhasználó mint  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="8f20f-236">In hello **Email Address** textbox, type **Email Address** of hello user as **BrittaSimon@contoso.com**.</span></span>
    
    <span data-ttu-id="8f20f-237">b.</span><span class="sxs-lookup"><span data-stu-id="8f20f-237">b.</span></span> <span data-ttu-id="8f20f-238">A hello **globális név** szövegmezőhöz típus hello **felhasználónév** , **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-238">In hello **Global Name** textbox, type hello **username** as **BrittaSimon**.</span></span>

    <span data-ttu-id="8f20f-239">c.</span><span class="sxs-lookup"><span data-stu-id="8f20f-239">c.</span></span> <span data-ttu-id="8f20f-240">A hello **jelszó**, és **jelszó megerősítése** szövegmezőből, típus hello **jelszó** hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="8f20f-240">In hello **Password**, and **Confirm Password** textboxes, type hello **Password** of hello user.</span></span>
   
    <span data-ttu-id="8f20f-241">b.</span><span class="sxs-lookup"><span data-stu-id="8f20f-241">b.</span></span> <span data-ttu-id="8f20f-242">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-242">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="8f20f-243">Egyéb Mimecast személyes portál felhasználói fiók létrehozása eszközök és személyes Portal Mimecast tooprovision az Azure AD felhasználói fiókok által nyújtott API-kat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8f20f-243">You can use any other Mimecast Personal Portal user account creation tools or APIs provided by Mimecast Personal Portal tooprovision Azure AD user accounts.</span></span> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8f20f-244">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8f20f-244">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8f20f-245">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMimecast személyes Portal megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8f20f-245">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Personal Portal.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8f20f-247">**tooassign Britta Simon tooMimecast személyes Portal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8f20f-247">**tooassign Britta Simon tooMimecast Personal Portal, perform hello following steps:**</span></span>

1. <span data-ttu-id="8f20f-248">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-248">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8f20f-250">Hello alkalmazások listában válassza ki a **Mimecast személyes Portal**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-250">In hello applications list, select **Mimecast Personal Portal**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_mimecastpersonalportal_app.png) 

3. <span data-ttu-id="8f20f-252">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8f20f-252">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8f20f-254">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8f20f-254">Click **Add** button.</span></span> <span data-ttu-id="8f20f-255">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f20f-255">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8f20f-257">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8f20f-257">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8f20f-258">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f20f-258">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8f20f-259">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8f20f-259">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8f20f-260">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8f20f-260">Testing single sign-on</span></span>
<span data-ttu-id="8f20f-261">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8f20f-261">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8f20f-262">Ha a hozzáférési Panel hello hello Mimecast személyes Portal csempe gombra kattint, automatikusan bejelentkezett tooyour Mimecast személyes Portal alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="8f20f-262">When you click hello Mimecast Personal Portal tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Personal Portal application.</span></span> <span data-ttu-id="8f20f-263">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8f20f-263">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8f20f-264">További források</span><span class="sxs-lookup"><span data-stu-id="8f20f-264">Additional resources</span></span>

* [<span data-ttu-id="8f20f-265">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8f20f-265">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8f20f-266">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8f20f-266">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-personal-portal-tutorial/tutorial_general_203.png

