---
title: "Oktatóanyag: Azure Active Directoryval integrált Teamphoria |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Teamphoria között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: f32be9742b76f7fe464036dadc108c62e4a787a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-teamphoria"></a><span data-ttu-id="6efc2-103">Oktatóanyag: Azure Active Directoryval integrált Teamphoria</span><span class="sxs-lookup"><span data-stu-id="6efc2-103">Tutorial: Azure Active Directory integration with Teamphoria</span></span>

<span data-ttu-id="6efc2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Teamphoria az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6efc2-104">In this tutorial, you learn how toointegrate Teamphoria with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6efc2-105">Teamphoria integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6efc2-105">Integrating Teamphoria with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6efc2-106">Megadhatja a hozzáférés tooTeamphoria rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6efc2-106">You can control in Azure AD who has access tooTeamphoria</span></span>
- <span data-ttu-id="6efc2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooTeamphoria (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6efc2-107">You can enable your users tooautomatically get signed-on tooTeamphoria (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6efc2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="6efc2-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="6efc2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6efc2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Teamphoria, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Teamphoria.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="6efc2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6efc2-110">Prerequisites</span></span>

<span data-ttu-id="6efc2-111">az Azure AD integrálása Teamphoria tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6efc2-111">tooconfigure Azure AD integration with Teamphoria, you need hello following items:</span></span>

- <span data-ttu-id="6efc2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6efc2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6efc2-113">Egy Teamphoria egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="6efc2-113">A Teamphoria single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6efc2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6efc2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6efc2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6efc2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6efc2-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6efc2-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="6efc2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6efc2-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6efc2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6efc2-118">Scenario description</span></span>
<span data-ttu-id="6efc2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6efc2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6efc2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6efc2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6efc2-121">Hello gyűjteményből Teamphoria hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6efc2-121">Adding Teamphoria from hello gallery</span></span>
2. <span data-ttu-id="6efc2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6efc2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-teamphoria-from-hello-gallery"></a><span data-ttu-id="6efc2-123">Hello gyűjteményből Teamphoria hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6efc2-123">Adding Teamphoria from hello gallery</span></span>
<span data-ttu-id="6efc2-124">tooconfigure hello integrációja Teamphoria az Azure AD-be, meg kell tooadd Teamphoria hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6efc2-124">tooconfigure hello integration of Teamphoria into Azure AD, you need tooadd Teamphoria from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6efc2-125">**tooadd Teamphoria hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6efc2-125">**tooadd Teamphoria from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6efc2-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6efc2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6efc2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6efc2-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6efc2-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6efc2-133">Hello keresési mezőbe, írja be a **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-133">In hello search box, type **Teamphoria**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_search.png)

5. <span data-ttu-id="6efc2-135">A hello eredmények panelen válassza ki a **Teamphoria**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-135">In hello results panel, select **Teamphoria**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6efc2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6efc2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6efc2-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="6efc2-138">In this section, you configure and test Azure AD single sign-on with Teamphoria based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6efc2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Teamphoria tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6efc2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Teamphoria is tooa user in Azure AD.</span></span> <span data-ttu-id="6efc2-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Teamphoria közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6efc2-140">In other words, a link relationship between an Azure AD user and hello related user in Teamphoria needs toobe established.</span></span>

<span data-ttu-id="6efc2-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Teamphoria.</span><span class="sxs-lookup"><span data-stu-id="6efc2-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Teamphoria.</span></span>

<span data-ttu-id="6efc2-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Teamphoria-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6efc2-142">tooconfigure and test Azure AD single sign-on with Teamphoria, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6efc2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6efc2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6efc2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6efc2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6efc2-145">**[Teamphoria tesztfelhasználó létrehozása](#creating-a-teamphoria-test-user)**  -toohave Britta Simon Teamphoria, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="6efc2-145">**[Creating a Teamphoria test user](#creating-a-teamphoria-test-user)** - toohave a counterpart of Britta Simon in Teamphoria that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="6efc2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6efc2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6efc2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6efc2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6efc2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6efc2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6efc2-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Teamphoria alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6efc2-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Teamphoria application.</span></span>

<span data-ttu-id="6efc2-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Teamphoria, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6efc2-150">**tooconfigure Azure AD single sign-on with Teamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="6efc2-151">Hello Azure felügyeleti portálon, a hello **Teamphoria** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-151">In hello Azure Management portal, on hello **Teamphoria** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6efc2-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="6efc2-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_samlbase.png)

3. <span data-ttu-id="6efc2-155">A hello **Teamphoria tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6efc2-155">On hello **Teamphoria Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_url.png)

    <span data-ttu-id="6efc2-157">a.</span><span class="sxs-lookup"><span data-stu-id="6efc2-157">a.</span></span> <span data-ttu-id="6efc2-158">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-cím a következő mintát hello használata:`https://<sub-domain>.teamphoria.com/login`</span><span class="sxs-lookup"><span data-stu-id="6efc2-158">In hello **Sign-on URL** textbox, type hello URL using hello following pattern: `https://<sub-domain>.teamphoria.com/login`</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="6efc2-159">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="6efc2-159">Please note that these are not hello real values.</span></span> <span data-ttu-id="6efc2-160">Ezeket az értékeket a hello rendelkezik tooupdate tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6efc2-160">You have tooupdate these values with hello actual Sign-On URL.</span></span> <span data-ttu-id="6efc2-161">Ügyfél [Teamphoria ügyfél-támogatási csoport](https://www.teamphoria.com/) tooget hello bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6efc2-161">Contact [Teamphoria Client support team](https://www.teamphoria.com/) tooget hello Sign-on URL.</span></span> 

4. <span data-ttu-id="6efc2-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítvány a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6efc2-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_certificate.png) 

5. <span data-ttu-id="6efc2-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6efc2-166">A hello **Teamphoria konfigurációs** kattintson **konfigurálása Teamphoria** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6efc2-166">On hello **Teamphoria Configuration** section, click **Configure Teamphoria** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6efc2-167">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6efc2-167">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_configure.png) 

7. <span data-ttu-id="6efc2-169">tooconfigure egyszeri bejelentkezést a **Teamphoria** oldalon, a bejelentkezési tooyour Teamphoria alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6efc2-169">tooconfigure single sign-on on **Teamphoria** side, Login tooyour Teamphoria application as an administrator.</span></span>

8. <span data-ttu-id="6efc2-170">Nyissa meg túl**rendszergazdai beállítások** hello bal eszköztár és mellett hello hello konfigurálása lapon kattintson a beállítás **egyetlen SIGN-ON** tooopen hello SSO konfigurációs ablak.</span><span class="sxs-lookup"><span data-stu-id="6efc2-170">Go too**ADMIN SETTINGS** option in hello left toolbar and under hello hello Configure Tab click on **SINGLE SIGN-ON** tooopen hello SSO configuration window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/admin_sso_configure.png)

9. <span data-ttu-id="6efc2-172">Kattintson a **hozzáadása új IDENTITÁSSZOLGÁLTATÓ** hello jobb felső sarokban található tooopen hello űrlapot hello-beállítások az egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="6efc2-172">Click on **ADD NEW IDENTITY PROVIDER** option in hello top right corner tooopen hello form for adding hello settings for SSO.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/add_new_identity_provider.png)

10. <span data-ttu-id="6efc2-174">Írja be a hello adatait hello mezőket lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="6efc2-174">Enter hello details in hello fields as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/Teamphoria_sso_save.png)

    <span data-ttu-id="6efc2-176">a.</span><span class="sxs-lookup"><span data-stu-id="6efc2-176">a.</span></span> <span data-ttu-id="6efc2-177">**MEGJELENÍTENDŐ név** : hello felügyelet lapon adja meg a hello megjelenített neve a hello beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="6efc2-177">**DISPLAY NAME** : Enter hello display name of hello plugin on hello admin page.</span></span>

    <span data-ttu-id="6efc2-178">b.</span><span class="sxs-lookup"><span data-stu-id="6efc2-178">b.</span></span> <span data-ttu-id="6efc2-179">**GOMB neve** : hello lap hello bejelentkezési oldalán használatával történő Egyszeri bejelentkezéshez megjelenítő hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6efc2-179">**BUTTON NAME** : hello name of hello tab which will display on hello login page for logging in via SSO.</span></span>

    <span data-ttu-id="6efc2-180">c.</span><span class="sxs-lookup"><span data-stu-id="6efc2-180">c.</span></span> <span data-ttu-id="6efc2-181">**TANÚSÍTVÁNY** : nyitott hello tanúsítvány korábban letöltött hello Azure-portálon a Jegyzettömbben hello hello tartalmának másolása a azonos, és illessze be ide a hello mezőbe.</span><span class="sxs-lookup"><span data-stu-id="6efc2-181">**CERTIFICATE** : Open hello Certificate downloaded earlier from hello Azure portal in notepad, copy hello contents of hello same and paste it here in hello box.</span></span>

    <span data-ttu-id="6efc2-182">d.</span><span class="sxs-lookup"><span data-stu-id="6efc2-182">d.</span></span> <span data-ttu-id="6efc2-183">**A belépési pont** : Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** korábbi űrlap hello Azure-portálra másolja.</span><span class="sxs-lookup"><span data-stu-id="6efc2-183">**ENTRY POINT** : Paste hello **SAML Single Sign-On Service URL** copied earlier form hello Azure portal.</span></span>

    <span data-ttu-id="6efc2-184">e.</span><span class="sxs-lookup"><span data-stu-id="6efc2-184">e.</span></span> <span data-ttu-id="6efc2-185">Hello beállítás túl kapcsoló**ON** , majd kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-185">Switch hello option too**ON** and click on **SAVE**.</span></span> 

<!--### Next steps

tooensure users can sign-in tooTeamphoria after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Teamphoria prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooTeamphoria in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Teamphoria users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6efc2-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6efc2-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="6efc2-187">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="6efc2-187">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6efc2-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6efc2-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6efc2-190">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-190">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6efc2-192">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="6efc2-192">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6efc2-194">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6efc2-194">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6efc2-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6efc2-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-teamphoria-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6efc2-198">a.</span><span class="sxs-lookup"><span data-stu-id="6efc2-198">a.</span></span> <span data-ttu-id="6efc2-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6efc2-200">b.</span><span class="sxs-lookup"><span data-stu-id="6efc2-200">b.</span></span> <span data-ttu-id="6efc2-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6efc2-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6efc2-202">c.</span><span class="sxs-lookup"><span data-stu-id="6efc2-202">c.</span></span> <span data-ttu-id="6efc2-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6efc2-204">d.</span><span class="sxs-lookup"><span data-stu-id="6efc2-204">d.</span></span> <span data-ttu-id="6efc2-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-205">Click **Create**.</span></span>
 
### <a name="creating-a-teamphoria-test-user"></a><span data-ttu-id="6efc2-206">Teamphoria tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6efc2-206">Creating a Teamphoria test user</span></span>

<span data-ttu-id="6efc2-207">A sorrend tooenable az Azure AD felhasználók toolog Teamphoria be azok ki kell építenie Teamphoria be.</span><span class="sxs-lookup"><span data-stu-id="6efc2-207">In order tooenable Azure AD users toolog into Teamphoria, they must be provisioned into Teamphoria.</span></span> <span data-ttu-id="6efc2-208">Teamphoria hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="6efc2-208">In hello case of Teamphoria, provisioning is a manual task.</span></span>

<span data-ttu-id="6efc2-209">**tooprovision felhasználói fiókok, hajtsa végre hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="6efc2-209">**tooprovision a user accounts, perform hello following steps:**</span></span>

1. <span data-ttu-id="6efc2-210">Jelentkezzen be tooyour Teamphoria vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6efc2-210">Log in tooyour Teamphoria company site as an administrator.</span></span>

2. <span data-ttu-id="6efc2-211">Kattintson a **ADMIN** beállítások hello bal eszköztáron és a hello **kezelése** lapon kattintson a **felhasználók** tooopen hello felügyeleti lapot a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="6efc2-211">Click on **ADMIN** settings on hello left toolbar and under hello **MANAGE** tab Click on **USERS** tooopen hello admin page for users.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-teamphoria-tutorial/admin_manage_users.png)

3. <span data-ttu-id="6efc2-213">Kattintson a hello **manuális MEGHÍVÁSA** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6efc2-213">Click on hello **MANUAL INVITE** option.</span></span>

    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/admin_manage_add_users.png)  

4. <span data-ttu-id="6efc2-215">Ezen a lapon hajtsa végre a következő művelet.</span><span class="sxs-lookup"><span data-stu-id="6efc2-215">On this page, perform following action.</span></span> 
    
    ![Felkérése](./media/active-directory-saas-teamphoria-tutorial/manual_user_invite.png)  

    <span data-ttu-id="6efc2-217">a.</span><span class="sxs-lookup"><span data-stu-id="6efc2-217">a.</span></span> <span data-ttu-id="6efc2-218">A hello **E-mail cím** szövegmezőhöz hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6efc2-218">In hello **EMAIL ADDRESS** textbox, hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6efc2-219">b.</span><span class="sxs-lookup"><span data-stu-id="6efc2-219">b.</span></span> <span data-ttu-id="6efc2-220">A hello **UTÓNÉV** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-220">In hello **FIRST NAME** textbox, type **Britta**.</span></span>

    <span data-ttu-id="6efc2-221">c.</span><span class="sxs-lookup"><span data-stu-id="6efc2-221">c.</span></span> <span data-ttu-id="6efc2-222">A hello **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-222">In hello **LAST NAME** textbox, type **Simon**.</span></span>

    <span data-ttu-id="6efc2-223">d.</span><span class="sxs-lookup"><span data-stu-id="6efc2-223">d.</span></span> <span data-ttu-id="6efc2-224">Kattintson a **MEGHÍVÁSA 1 felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-224">Click **INVITE 1 USER**.</span></span> <span data-ttu-id="6efc2-225">Felhasználói tooaccept hello a meghívás tooget hello rendszerben létrehozott kell.</span><span class="sxs-lookup"><span data-stu-id="6efc2-225">User needs tooaccept hello invite tooget created in hello system.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6efc2-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6efc2-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6efc2-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooTeamphoria megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6efc2-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooTeamphoria.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6efc2-229">**tooassign Britta Simon tooTeamphoria, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6efc2-229">**tooassign Britta Simon tooTeamphoria, perform hello following steps:**</span></span>

1. <span data-ttu-id="6efc2-230">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-230">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6efc2-232">Hello alkalmazások listában válassza ki a **Teamphoria**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-232">In hello applications list, select **Teamphoria**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-teamphoria-tutorial/tutorial_teamphoria_app.png) 

3. <span data-ttu-id="6efc2-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6efc2-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6efc2-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6efc2-236">Click **Add** button.</span></span> <span data-ttu-id="6efc2-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6efc2-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6efc2-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6efc2-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6efc2-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6efc2-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6efc2-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6efc2-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6efc2-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6efc2-242">Testing single sign-on</span></span>

<span data-ttu-id="6efc2-243">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="6efc2-243">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6efc2-244">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="6efc2-244">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="6efc2-245">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="6efc2-245">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="6efc2-246">További források</span><span class="sxs-lookup"><span data-stu-id="6efc2-246">Additional resources</span></span>

* [<span data-ttu-id="6efc2-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6efc2-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6efc2-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6efc2-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-teamphoria-tutorial/tutorial_general_203.png

