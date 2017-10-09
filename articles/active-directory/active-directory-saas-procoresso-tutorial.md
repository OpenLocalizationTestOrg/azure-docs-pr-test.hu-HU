---
title: "Oktatóanyag: Azure Active Directoryval integrált Procore SSO |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Procore SSO között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9818edd3-48c0-411d-b05a-3ec805eafb2e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/07/2017
ms.author: jeedes
ms.openlocfilehash: bb918617f18ba3f44ddde469e6fc411977752f15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-procore-sso"></a><span data-ttu-id="76d57-103">Oktatóanyag: Azure Active Directoryval integrált Procore egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="76d57-103">Tutorial: Azure Active Directory integration with Procore SSO</span></span>

<span data-ttu-id="76d57-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Procore egyszeri bejelentkezés az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="76d57-104">In this tutorial, you learn how toointegrate Procore SSO with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="76d57-105">Procore SSO integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="76d57-105">Integrating Procore SSO with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="76d57-106">Megadhatja a hozzáférés tooProcore SSO rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="76d57-106">You can control in Azure AD who has access tooProcore SSO</span></span>
- <span data-ttu-id="76d57-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooProcore egyszeri Bejelentkezést (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="76d57-107">You can enable your users tooautomatically get signed-on tooProcore SSO (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="76d57-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="76d57-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="76d57-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="76d57-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Procore SSO, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Procore SSO.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="76d57-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="76d57-110">Prerequisites</span></span>

<span data-ttu-id="76d57-111">tooconfigure Procore egyszeri bejelentkezés az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="76d57-111">tooconfigure Azure AD integration with Procore SSO, you need hello following items:</span></span>

- <span data-ttu-id="76d57-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="76d57-112">An Azure AD subscription</span></span>
- <span data-ttu-id="76d57-113">Egy Procore SSO egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="76d57-113">A Procore SSO single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="76d57-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="76d57-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="76d57-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="76d57-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="76d57-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="76d57-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="76d57-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="76d57-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="76d57-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="76d57-118">Scenario description</span></span>
<span data-ttu-id="76d57-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="76d57-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="76d57-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="76d57-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="76d57-121">Hello gyűjteményből Procore SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="76d57-121">Adding Procore SSO from hello gallery</span></span>
2. <span data-ttu-id="76d57-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="76d57-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-procore-sso-from-hello-gallery"></a><span data-ttu-id="76d57-123">Hello gyűjteményből Procore SSO hozzáadása</span><span class="sxs-lookup"><span data-stu-id="76d57-123">Adding Procore SSO from hello gallery</span></span>
<span data-ttu-id="76d57-124">tooconfigure hello integrációja Procore egyszeri bejelentkezés az Azure AD-be, meg kell tooadd Procore SSO hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="76d57-124">tooconfigure hello integration of Procore SSO into Azure AD, you need tooadd Procore SSO from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="76d57-125">**tooadd Procore SSO hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="76d57-125">**tooadd Procore SSO from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="76d57-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="76d57-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="76d57-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="76d57-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="76d57-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="76d57-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="76d57-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="76d57-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="76d57-133">Hello keresési mezőbe, írja be a **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="76d57-133">In hello search box, type **Procore SSO**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_search.png)

5. <span data-ttu-id="76d57-135">A hello eredmények panelen válassza a **Procore SSO**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="76d57-135">In hello results panel, select **Procore SSO**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="76d57-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="76d57-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="76d57-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Procore SSO "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="76d57-138">In this section, you configure and test Azure AD single sign-on with Procore SSO based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="76d57-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Procore SSO tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="76d57-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Procore SSO is tooa user in Azure AD.</span></span> <span data-ttu-id="76d57-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Procore SSO közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="76d57-140">In other words, a link relationship between an Azure AD user and hello related user in Procore SSO needs toobe established.</span></span>

<span data-ttu-id="76d57-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Procore egyszeri Bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76d57-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Procore SSO.</span></span>

<span data-ttu-id="76d57-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Procore SSO-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="76d57-142">tooconfigure and test Azure AD single sign-on with Procore SSO, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="76d57-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="76d57-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="76d57-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="76d57-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="76d57-145">**[Procore SSO tesztfelhasználó létrehozása](#creating-a-procore-sso-test-user)**  -toohave Britta Simon Procore egyszeri Bejelentkezést, amelyek az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="76d57-145">**[Creating a Procore SSO test user](#creating-a-procore-sso-test-user)** - toohave a counterpart of Britta Simon in Procore SSO that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="76d57-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="76d57-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="76d57-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="76d57-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="76d57-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="76d57-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="76d57-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Procore SSO-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="76d57-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Procore SSO application.</span></span>

<span data-ttu-id="76d57-150">**az Azure AD tooconfigure egyszeri bejelentkezés Procore egyszeri bejelentkezési modellel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="76d57-150">**tooconfigure Azure AD single sign-on with Procore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="76d57-151">Hello Azure felügyeleti portálon, a hello **Procore SSO** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="76d57-151">In hello Azure Management portal, on hello **Procore SSO** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="76d57-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="76d57-153">On hello **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_samlbase.png)

3. <span data-ttu-id="76d57-155">A hello **Procore SSO-tartomány és az URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="76d57-155">On hello **Procore SSO Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_url.png)

4. <span data-ttu-id="76d57-157">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="76d57-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_certificate.png) 

5. <span data-ttu-id="76d57-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="76d57-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="76d57-161">A hello **Procore SSO konfigurációs** kattintson **Procore SSO konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="76d57-161">On hello **Procore SSO Configuration** section, click **Configure Procore SSO** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="76d57-162">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="76d57-162">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_configure.png) 

7. <span data-ttu-id="76d57-164">tooconfigure egyszeri bejelentkezést a **Procore SSO** oldalon, a bejelentkezési tooyour procore vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="76d57-164">tooconfigure single sign-on on **Procore SSO** side, login tooyour procore company site as an administrator.</span></span>

8. <span data-ttu-id="76d57-165">Lefelé hello eszközkészlet legördülő menüből, kattintson a **Admin** tooopen hello egyszeri bejelentkezési beállítások lapon.</span><span class="sxs-lookup"><span data-stu-id="76d57-165">From hello toolbox drop down, click on **Admin** tooopen hello SSO settings page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_tool_admin.png)

9. <span data-ttu-id="76d57-167">Beillesztés hello értékek hello mezőiben lásd az alábbi-</span><span class="sxs-lookup"><span data-stu-id="76d57-167">Paste hello values in hello boxes as described below-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/procore_setting_admin.png)    

    <span data-ttu-id="76d57-169">a.</span><span class="sxs-lookup"><span data-stu-id="76d57-169">a.</span></span> <span data-ttu-id="76d57-170">A hello **egyszeri bejelentkezési kibocsátó URL-cím** mezőbe illessze be a hello SAML Entitásazonosító átmásolva hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="76d57-170">In hello **Single Sign On Issuer URL** box, paste hello SAML Entity ID copied from hello Azure portal.</span></span>

    <span data-ttu-id="76d57-171">b.</span><span class="sxs-lookup"><span data-stu-id="76d57-171">b.</span></span> <span data-ttu-id="76d57-172">A hello **SAML bejelentkezési cél URL-cím** mezőbe illessze be a hello SAML-alapú egyszeri bejelentkezési URL-címe átmásolva hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="76d57-172">In hello **SAML Sign On Target URL** box, paste hello SAML Single Sign-On Service URL copied from hello Azure portal.</span></span>

    <span data-ttu-id="76d57-173">c.</span><span class="sxs-lookup"><span data-stu-id="76d57-173">c.</span></span> <span data-ttu-id="76d57-174">Most nyissa meg a hello **metaadatainak XML-kódja** hello Azure-portál és a példány hello tanúsítvány nevű hello címkében fent letöltött **x.509**.</span><span class="sxs-lookup"><span data-stu-id="76d57-174">Now open hello **Metadata XML** downloaded above from hello Azure portal and copy hello certficate in hello tag named **X509Certificate**.</span></span> <span data-ttu-id="76d57-175">Beillesztés hello értéket másol a hello **egyszeri bejelentkezés x509 tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="76d57-175">Paste hello copied value into hello **Single Sign On x509 Certificate** box.</span></span>

10. <span data-ttu-id="76d57-176">Kattintson a **módosítások mentése**.</span><span class="sxs-lookup"><span data-stu-id="76d57-176">Click on **Save Changes**.</span></span>

11. <span data-ttu-id="76d57-177">Után ezek a beállítások toosend hello kell **tartománynév** (pl. **contoso.com**) keresztül, amely jelentkezik be Procore toohello [Procore támogatási csoport](https://support.procore.com/) és össze lesz aktiválja az összevont egyszeri Bejelentkezéses ehhez a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="76d57-177">After these settings, you needs toosend hello **domain name** (e.g **contoso.com**) through which you are logging into Procore toohello [Procore Support team](https://support.procore.com/) and they will activate federated SSO for that domain.</span></span>

<!--### Next steps

tooensure users can sign-in tooProcore SSO after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Procore SSO prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooProcore SSO in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Procore SSO users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="76d57-178">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="76d57-178">Creating an Azure AD test user</span></span>
<span data-ttu-id="76d57-179">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="76d57-179">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="76d57-181">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="76d57-181">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="76d57-182">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="76d57-182">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="76d57-184">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="76d57-184">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="76d57-186">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76d57-186">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="76d57-188">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="76d57-188">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-procoresso-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="76d57-190">a.</span><span class="sxs-lookup"><span data-stu-id="76d57-190">a.</span></span> <span data-ttu-id="76d57-191">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="76d57-191">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="76d57-192">b.</span><span class="sxs-lookup"><span data-stu-id="76d57-192">b.</span></span> <span data-ttu-id="76d57-193">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="76d57-193">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="76d57-194">c.</span><span class="sxs-lookup"><span data-stu-id="76d57-194">c.</span></span> <span data-ttu-id="76d57-195">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="76d57-195">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="76d57-196">d.</span><span class="sxs-lookup"><span data-stu-id="76d57-196">d.</span></span> <span data-ttu-id="76d57-197">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="76d57-197">Click **Create**.</span></span>
 
### <a name="creating-a-procore-sso-test-user"></a><span data-ttu-id="76d57-198">Procore SSO tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="76d57-198">Creating a Procore SSO test user</span></span>

<span data-ttu-id="76d57-199">Az oldalon hajtsa végre a lépéseket toocreate Procore tesztfelhasználó alatt hello.</span><span class="sxs-lookup"><span data-stu-id="76d57-199">Please follow hello below steps toocreate a Procore test user on their side.</span></span>

1. <span data-ttu-id="76d57-200">Bejelentkezési tooyour procore vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="76d57-200">Login tooyour procore company site as an administrator.</span></span>  

2. <span data-ttu-id="76d57-201">Lefelé hello eszközkészlet legördülő menüből, kattintson a **Directory** tooopen hello vállalati könyvtár lapon.</span><span class="sxs-lookup"><span data-stu-id="76d57-201">From hello toolbox drop down, click on **Directory** tooopen hello company directory page.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_sso_directory.png)

3. <span data-ttu-id="76d57-203">Kattintson a **hozzáadása egy személy** tooopen hello űrlap lehetőséget, majd adja meg hajtsa végre a következő beállítások -</span><span class="sxs-lookup"><span data-stu-id="76d57-203">Click on **Add a Person** option tooopen hello form and enter perform following options -</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_add.png)

    <span data-ttu-id="76d57-205">a.</span><span class="sxs-lookup"><span data-stu-id="76d57-205">a.</span></span> <span data-ttu-id="76d57-206">A hello **Utónév** szövegmezőhöz típus felhasználó utónevét például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="76d57-206">In hello **First Name** textbox, type user's first name like **Britta**.</span></span>

    <span data-ttu-id="76d57-207">b.</span><span class="sxs-lookup"><span data-stu-id="76d57-207">b.</span></span> <span data-ttu-id="76d57-208">A hello **Vezetéknév** szövegmező, például típus felhasználó vezetékneve **Simon**.</span><span class="sxs-lookup"><span data-stu-id="76d57-208">In hello **Last name** textbox, type user's last name like **Simon**.</span></span>

    <span data-ttu-id="76d57-209">c.</span><span class="sxs-lookup"><span data-stu-id="76d57-209">c.</span></span> <span data-ttu-id="76d57-210">A hello **E-mail cím** szövegmezőhöz típusa a felhasználó e-mail címét, például  **BrittaSimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="76d57-210">In hello **Email Address** textbox, type user's email address like **BrittaSimon@contoso.com**.</span></span>

    <span data-ttu-id="76d57-211">d.</span><span class="sxs-lookup"><span data-stu-id="76d57-211">d.</span></span> <span data-ttu-id="76d57-212">Válassza ki **engedély sablon** , **engedély sablon alkalmazása később**.</span><span class="sxs-lookup"><span data-stu-id="76d57-212">Select **Permission Template** as **Apply Permission Template Later**.</span></span>

    <span data-ttu-id="76d57-213">e.</span><span class="sxs-lookup"><span data-stu-id="76d57-213">e.</span></span> <span data-ttu-id="76d57-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="76d57-214">Click **Create**.</span></span>

4. <span data-ttu-id="76d57-215">Ellenőrizze, és frissítése a hello részletek hello újonnan hozzáadott forduljon.</span><span class="sxs-lookup"><span data-stu-id="76d57-215">Check and update hello details for hello newly added contact.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_check.png)

5. <span data-ttu-id="76d57-217">Kattintson a **mentése és küldése Invitiation** (ha szükséges egy összehíváshoz mail keresztül) vagy **mentése** (közvetlenül mentése) toocomplete hello felhasználói regisztráció.</span><span class="sxs-lookup"><span data-stu-id="76d57-217">Click on **Save and Send Invitiation** (if an invite through mail is required) or **Save** (Save directly) toocomplete hello user registration.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/Procore_user_save.png)    

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="76d57-219">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="76d57-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="76d57-220">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooProcore SSO megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="76d57-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooProcore SSO.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="76d57-222">**tooassign Britta Simon tooProcore SSO, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="76d57-222">**tooassign Britta Simon tooProcore SSO, perform hello following steps:**</span></span>

1. <span data-ttu-id="76d57-223">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="76d57-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="76d57-225">Hello alkalmazások listában válassza ki a **Procore SSO**.</span><span class="sxs-lookup"><span data-stu-id="76d57-225">In hello applications list, select **Procore SSO**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-procoresso-tutorial/tutorial_procoresso_app.png) 

3. <span data-ttu-id="76d57-227">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="76d57-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="76d57-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="76d57-229">Click **Add** button.</span></span> <span data-ttu-id="76d57-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76d57-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="76d57-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="76d57-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="76d57-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76d57-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="76d57-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="76d57-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="76d57-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="76d57-235">Testing single sign-on</span></span>

<span data-ttu-id="76d57-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="76d57-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="76d57-237">Ha azt szeretné, az egyszeri bejelentkezés beállításainak ellenőrzéséhez nyissa meg a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="76d57-237">If you want to test your single sign-on settings, open the Access Panel.</span></span> <span data-ttu-id="76d57-238">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="76d57-238">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> <span data-ttu-id="76d57-239">Ha a hozzáférési Panel hello hello Procore SSO csempe gombra kattint, automatikusan bejelentkezett tooyour Procore SSO alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="76d57-239">When you click hello Procore SSO tile in hello Access Panel, you should get automatically signed-on tooyour Procore SSO application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="76d57-240">További források</span><span class="sxs-lookup"><span data-stu-id="76d57-240">Additional resources</span></span>

* [<span data-ttu-id="76d57-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="76d57-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="76d57-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="76d57-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-procoresso-tutorial/tutorial_general_203.png

