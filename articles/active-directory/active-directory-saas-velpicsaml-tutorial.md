---
title: "Oktatóanyag: Azure Active Directoryval integrált Velpic SAML |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Velpic SAML között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 28acce3e-22a0-4a37-8b66-6e518d777350
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: jeedes
ms.openlocfilehash: 613947d8fe95113382a2cdc0f79ce9eda85a0127
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-velpic-saml"></a><span data-ttu-id="cda24-103">Oktatóanyag: Azure Active Directoryval integrált Velpic SAML</span><span class="sxs-lookup"><span data-stu-id="cda24-103">Tutorial: Azure Active Directory integration with Velpic SAML</span></span>

<span data-ttu-id="cda24-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Velpic SAML az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="cda24-104">In this tutorial, you learn how toointegrate Velpic SAML with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="cda24-105">Velpic SAML integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="cda24-105">Integrating Velpic SAML with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="cda24-106">Megadhatja a hozzáférés tooVelpic SAML rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="cda24-106">You can control in Azure AD who has access tooVelpic SAML</span></span>
- <span data-ttu-id="cda24-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooVelpic SAML (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="cda24-107">You can enable your users tooautomatically get signed-on tooVelpic SAML (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="cda24-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="cda24-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="cda24-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="cda24-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cda24-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="cda24-110">Prerequisites</span></span>

<span data-ttu-id="cda24-111">az Azure AD integrálása Velpic SAML tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="cda24-111">tooconfigure Azure AD integration with Velpic SAML, you need hello following items:</span></span>

- <span data-ttu-id="cda24-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="cda24-112">An Azure AD subscription</span></span>
- <span data-ttu-id="cda24-113">Egy Velpic SAML-alapú egyszeri bejelentkezést engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="cda24-113">A Velpic SAML single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="cda24-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="cda24-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="cda24-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="cda24-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="cda24-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="cda24-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="cda24-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="cda24-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="cda24-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="cda24-118">Scenario description</span></span>
<span data-ttu-id="cda24-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="cda24-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="cda24-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="cda24-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="cda24-121">Hello gyűjteményből Velpic SAML hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cda24-121">Adding Velpic SAML from hello gallery</span></span>
2. <span data-ttu-id="cda24-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cda24-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-velpic-saml-from-hello-gallery"></a><span data-ttu-id="cda24-123">Hello gyűjteményből Velpic SAML hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cda24-123">Adding Velpic SAML from hello gallery</span></span>
<span data-ttu-id="cda24-124">tooconfigure hello integrációja Velpic SAML az Azure AD-be, meg kell tooadd Velpic SAML hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="cda24-124">tooconfigure hello integration of Velpic SAML into Azure AD, you need tooadd Velpic SAML from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="cda24-125">**tooadd Velpic SAML hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cda24-125">**tooadd Velpic SAML from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="cda24-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cda24-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="cda24-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="cda24-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="cda24-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cda24-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="cda24-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="cda24-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="cda24-133">Hello keresési mezőbe, írja be a **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="cda24-133">In hello search box, type **Velpic SAML**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_search.png)

5. <span data-ttu-id="cda24-135">A hello eredmények panelen válassza ki a **Velpic SAML**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-135">In hello results panel, select **Velpic SAML**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="cda24-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="cda24-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="cda24-138">Ebben a szakaszban konfigurálhatja, és az Azure AD az egyszeri bejelentkezés Velpic SAML-teszthez "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="cda24-138">In this section, you configure and test Azure AD single sign-on with Velpic SAML based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="cda24-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Velpic SAML tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="cda24-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Velpic SAML is tooa user in Azure AD.</span></span> <span data-ttu-id="cda24-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Velpic SAML közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="cda24-140">In other words, a link relationship between an Azure AD user and hello related user in Velpic SAML needs toobe established.</span></span>

<span data-ttu-id="cda24-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Velpic SAML.</span><span class="sxs-lookup"><span data-stu-id="cda24-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Velpic SAML.</span></span>

<span data-ttu-id="cda24-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Velpic SAML-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="cda24-142">tooconfigure and test Azure AD single sign-on with Velpic SAML, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="cda24-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cda24-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="cda24-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cda24-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="cda24-145">**[Velpic SAML tesztfelhasználó létrehozása](#creating-a-velpic-saml-test-user)**  -toohave egy megfelelője a Britta Simon a csatolt toohello az Azure AD ábrázolása rá, hogy Velpic SAML-alapú.</span><span class="sxs-lookup"><span data-stu-id="cda24-145">**[Creating a Velpic SAML test user](#creating-a-velpic-saml-test-user)** - toohave a counterpart of Britta Simon in Velpic SAML that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="cda24-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="cda24-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="cda24-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="cda24-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="cda24-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cda24-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="cda24-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Velpic SAML-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="cda24-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Velpic SAML application.</span></span>

<span data-ttu-id="cda24-150">**tooconfigure az Azure AD egyszeri bejelentkezéshez a Velpic SAML, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="cda24-150">**tooconfigure Azure AD single sign-on with Velpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="cda24-151">Hello Azure felügyeleti portálon, a hello **Velpic SAML** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="cda24-151">In hello Azure Management portal, on hello **Velpic SAML** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="cda24-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="cda24-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_samlbase.png)

3. <span data-ttu-id="cda24-155">Adjon meg hello részletet hello **Velpic SAML-tartomány és az URL-címek** szakasz -</span><span class="sxs-lookup"><span data-stu-id="cda24-155">Enter hello details in hello **Velpic SAML Domain and URLs** section-</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_url.png)

    <span data-ttu-id="cda24-157">a.</span><span class="sxs-lookup"><span data-stu-id="cda24-157">a.</span></span> <span data-ttu-id="cda24-158">A hello **bejelentkezési URL-cím** szövegmezőhöz hello érték típusa:`https://<sub-domain>.velpicsaml.net`</span><span class="sxs-lookup"><span data-stu-id="cda24-158">In hello **Sign-on URL** textbox, type hello value as: `https://<sub-domain>.velpicsaml.net`</span></span>

    <span data-ttu-id="cda24-159">b.</span><span class="sxs-lookup"><span data-stu-id="cda24-159">b.</span></span> <span data-ttu-id="cda24-160">A hello **azonosító** szövegmezőhöz Beillesztés hello **"Egyszeri bejelentkezési URL-címe"** érték`https://auth.velpic.com/saml/v2/<entity-id>/login`</span><span class="sxs-lookup"><span data-stu-id="cda24-160">In hello **Identifier** textbox, paste hello **‘Single sign on URL’** value `https://auth.velpic.com/saml/v2/<entity-id>/login`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="cda24-161">Vegye figyelembe, hogy hello Velpic SAML team biztosítja hello bejelentkezési URL-címen, és azonosító értéket hello SSO beépülő modul Velpic SAML oldalon konfigurálásakor lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="cda24-161">Please note that hello Sign on URL will be provided by hello Velpic SAML team and Identifier value will be available when you configure hello SSO Plugin on Velpic SAML side.</span></span> <span data-ttu-id="cda24-162">Meg kell toocopy Velpic SAML alkalmazáslap értéket, és illessze be ide.</span><span class="sxs-lookup"><span data-stu-id="cda24-162">You need toocopy that value from Velpic SAML application  page and paste it here.</span></span>

4. <span data-ttu-id="cda24-163">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cda24-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_certificate.png) 

5. <span data-ttu-id="cda24-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="cda24-167">Hello Velpic SAML-alapú konfigurációs szakaszt kattintson a Velpic SAML konfigurálása tooopen konfigurálása bejelentkezés ablak.</span><span class="sxs-lookup"><span data-stu-id="cda24-167">On hello Velpic SAML Configuration section, click Configure Velpic SAML tooopen Configure sign-on window.</span></span> <span data-ttu-id="cda24-168">Rövid összefoglaló szakasz hello hello SAML Entitásazonosító másolja.</span><span class="sxs-lookup"><span data-stu-id="cda24-168">Copy hello SAML Entity ID from hello Quick Reference section.</span></span>

7. <span data-ttu-id="cda24-169">Egy másik webes böngészőablakban jelentkezzen be a Velpic SAML-alapú vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="cda24-169">In a different web browser window, log into your Velpic SAML company site as an administrator.</span></span>

8. <span data-ttu-id="cda24-170">Kattintson a **kezelése** lapra, és nyissa meg túl**integrációs** tooclick kell a szakasz **beépülő modulok** gomb toocreate új beépülő modul a bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="cda24-170">Click on **Manage** tab and go too**Integration** section where you need tooclick on **Plugins** button toocreate new plugin for Sign-In.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_1.png)

9. <span data-ttu-id="cda24-172">Kattintson a hello **"Beépülő modul hozzáadása"** gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-172">Click on hello **‘Add plugin’** button.</span></span>
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_2.png)

10. <span data-ttu-id="cda24-174">Kattintson a hello **SAML** hello hozzáadása beépülő modul oldal csempére.</span><span class="sxs-lookup"><span data-stu-id="cda24-174">Click on hello **SAML** tile in hello Add Plugin page.</span></span>
    
    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_3.png)

11. <span data-ttu-id="cda24-176">Írja be hello új SAML-alapú beépülő modul hello nevét, és kattintson a hello **"Add"** gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-176">Enter hello name of hello new SAML plugin and click hello **‘Add’** button.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_4.png)

12. <span data-ttu-id="cda24-178">Adja meg az alábbiak szerint hello részleteit:</span><span class="sxs-lookup"><span data-stu-id="cda24-178">Enter hello details as follows:</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_5.png)

    <span data-ttu-id="cda24-180">a.</span><span class="sxs-lookup"><span data-stu-id="cda24-180">a.</span></span> <span data-ttu-id="cda24-181">A hello **neve** szövegmezőhöz SAML-alapú beépülő modul hello nevét.</span><span class="sxs-lookup"><span data-stu-id="cda24-181">In hello **Name** textbox, type hello name of SAML plugin.</span></span>

    <span data-ttu-id="cda24-182">b.</span><span class="sxs-lookup"><span data-stu-id="cda24-182">b.</span></span> <span data-ttu-id="cda24-183">A hello **kiállítójának URL-címe** szövegmezőhöz Beillesztés hello **SAML Entitásazonosító** hello a másolt **bejelentkezés konfigurálása** ablakot, hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="cda24-183">In hello **Issuer URL** textbox, paste hello **SAML Entity ID** you copied from hello **Configure sign-on** window of hello Azure portal.</span></span>

    <span data-ttu-id="cda24-184">c.</span><span class="sxs-lookup"><span data-stu-id="cda24-184">c.</span></span> <span data-ttu-id="cda24-185">A hello **szolgáltató metaadatok Config** hello metaadatok XML-fájl, amely az Azure-portálról letöltött feltöltése.</span><span class="sxs-lookup"><span data-stu-id="cda24-185">In hello **Provider Metadata Config** upload hello Metadata XML file which you downloaded from Azure portal.</span></span>

    <span data-ttu-id="cda24-186">d.</span><span class="sxs-lookup"><span data-stu-id="cda24-186">d.</span></span> <span data-ttu-id="cda24-187">Másik lehetőségként tooenable SAML csak az idő kiépítés engedélyezése hello **"Automatikus hozzon létre új felhasználók"** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="cda24-187">You can also choose tooenable SAML just in time provisioning by enabling hello **‘Auto create new users’** checkbox.</span></span> <span data-ttu-id="cda24-188">Ha egy felhasználó Velpic nem létezik, és ez a jelző nincs engedélyezve, az Azure-ból hello bejelentkezési sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="cda24-188">If a user doesn’t exist in Velpic and this flag is not enabled, hello login from Azure will fail.</span></span> <span data-ttu-id="cda24-189">Ha hello jelző engedélyezve hello felhasználói lesz automatikusan üzembe Velpic történő bejelentkezési ideje hello.</span><span class="sxs-lookup"><span data-stu-id="cda24-189">If hello flag is enabled hello user will automatically be provisioned into Velpic at hello time of login.</span></span> 

    <span data-ttu-id="cda24-190">e.</span><span class="sxs-lookup"><span data-stu-id="cda24-190">e.</span></span> <span data-ttu-id="cda24-191">Másolás hello **egyszeri bejelentkezés URL-címen** hello szövegmezőbe, és illessze be azt a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="cda24-191">Copy hello **Single sign on URL** from hello text box and paste it in hello Azure portal.</span></span>
    
    <span data-ttu-id="cda24-192">f.</span><span class="sxs-lookup"><span data-stu-id="cda24-192">f.</span></span> <span data-ttu-id="cda24-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-193">Click **Save**.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="cda24-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cda24-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="cda24-195">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="cda24-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="cda24-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="cda24-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="cda24-198">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="cda24-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="cda24-200">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="cda24-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="cda24-202">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cda24-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="cda24-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="cda24-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-velpicsaml-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="cda24-206">a.</span><span class="sxs-lookup"><span data-stu-id="cda24-206">a.</span></span> <span data-ttu-id="cda24-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="cda24-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="cda24-208">b.</span><span class="sxs-lookup"><span data-stu-id="cda24-208">b.</span></span> <span data-ttu-id="cda24-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="cda24-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="cda24-210">c.</span><span class="sxs-lookup"><span data-stu-id="cda24-210">c.</span></span> <span data-ttu-id="cda24-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="cda24-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="cda24-212">d.</span><span class="sxs-lookup"><span data-stu-id="cda24-212">d.</span></span> <span data-ttu-id="cda24-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-213">Click **Create**.</span></span>
 
### <a name="creating-a-velpic-saml-test-user"></a><span data-ttu-id="cda24-214">Velpic SAML tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cda24-214">Creating a Velpic SAML test user</span></span>

<span data-ttu-id="cda24-215">Ebben a lépésben esetén általában nincs szükség hello alkalmazás támogatja a csak az idő a felhasználók átadása.</span><span class="sxs-lookup"><span data-stu-id="cda24-215">This step is usually not required as hello application supports just in time user provisioning.</span></span> <span data-ttu-id="cda24-216">Ha nincs engedélyezve a hello automatikus felhasználó-átadási majd kézi felhasználó létrehozása teheti az alább ismertetett.</span><span class="sxs-lookup"><span data-stu-id="cda24-216">If hello automatic user provisioning is not enabled then manual user creation can be done as described below.</span></span>

<span data-ttu-id="cda24-217">Jelentkezzen be rendszergazdaként egy vállalati Velpic SAML-alapú webhelyekhez, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="cda24-217">Log into your Velpic SAML company site as an administrator and perform following steps:</span></span>
    
1. <span data-ttu-id="cda24-218">Kattintson a kezelés lapon tooUsers szakasz lépjen, majd kattintson az új gomb tooadd felhasználók.</span><span class="sxs-lookup"><span data-stu-id="cda24-218">Click on Manage tab and go tooUsers section, then click on New button tooadd users.</span></span>

    ![felhasználó hozzáadása](./media/active-directory-saas-velpicsaml-tutorial/velpic_7.png)

2. <span data-ttu-id="cda24-220">A hello **"Az új felhasználó létrehozása"** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello lapján.</span><span class="sxs-lookup"><span data-stu-id="cda24-220">On hello **“Create New User”** dialog page, perform hello following steps.</span></span>

    ![Felhasználó](./media/active-directory-saas-velpicsaml-tutorial/velpic_8.png)
    
    <span data-ttu-id="cda24-222">a.</span><span class="sxs-lookup"><span data-stu-id="cda24-222">a.</span></span> <span data-ttu-id="cda24-223">A hello **Keresztnév** szövegmezőhöz Britta Simon hello első nevét.</span><span class="sxs-lookup"><span data-stu-id="cda24-223">In hello **First Name** textbox, type hello first name of Britta Simon.</span></span>

    <span data-ttu-id="cda24-224">b.</span><span class="sxs-lookup"><span data-stu-id="cda24-224">b.</span></span> <span data-ttu-id="cda24-225">A hello **Vezetéknév** szövegmezőhöz típus hello vezetékneve Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="cda24-225">In hello **Last Name** textbox, type hello last name of Britta Simon.</span></span>

    <span data-ttu-id="cda24-226">c.</span><span class="sxs-lookup"><span data-stu-id="cda24-226">c.</span></span> <span data-ttu-id="cda24-227">A hello **felhasználónév** szövegmezőhöz Britta Simon hello felhasználói nevét.</span><span class="sxs-lookup"><span data-stu-id="cda24-227">In hello **User Name** textbox, type hello user name of Britta Simon.</span></span>

    <span data-ttu-id="cda24-228">d.</span><span class="sxs-lookup"><span data-stu-id="cda24-228">d.</span></span> <span data-ttu-id="cda24-229">A hello **E-mail** szövegmezőhöz típus hello Britta Simon fiókhoz tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="cda24-229">In hello **Email** textbox, type hello email address of Britta Simon account.</span></span>

    <span data-ttu-id="cda24-230">e.</span><span class="sxs-lookup"><span data-stu-id="cda24-230">e.</span></span> <span data-ttu-id="cda24-231">Hello információk többi nem kötelező, megadhatja, hogy szükség esetén.</span><span class="sxs-lookup"><span data-stu-id="cda24-231">Rest of hello information is optional, you can fill it if needed.</span></span>
    
    <span data-ttu-id="cda24-232">f.</span><span class="sxs-lookup"><span data-stu-id="cda24-232">f.</span></span> <span data-ttu-id="cda24-233">Kattintson a **SAVE** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-233">Click **SAVE**.</span></span>  

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="cda24-234">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="cda24-234">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="cda24-235">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooVelpic SAML megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="cda24-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooVelpic SAML.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="cda24-237">**tooassign Britta Simon tooVelpic SAML, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="cda24-237">**tooassign Britta Simon tooVelpic SAML, perform hello following steps:**</span></span>

1. <span data-ttu-id="cda24-238">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="cda24-238">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="cda24-240">Hello alkalmazások listában válassza ki a **Velpic SAML**.</span><span class="sxs-lookup"><span data-stu-id="cda24-240">In hello applications list, select **Velpic SAML**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-velpicsaml-tutorial/tutorial_velpicsaml_app.png) 

3. <span data-ttu-id="cda24-242">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="cda24-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="cda24-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="cda24-244">Click **Add** button.</span></span> <span data-ttu-id="cda24-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cda24-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="cda24-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="cda24-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="cda24-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cda24-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="cda24-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="cda24-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="cda24-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="cda24-250">Testing single sign-on</span></span>

<span data-ttu-id="cda24-251">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="cda24-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

1. <span data-ttu-id="cda24-252">Hello Velpic SAML hello hozzáférési Panel csempére kattintva Velpic SAML alkalmazás bejelentkezési oldalán szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="cda24-252">When you click hello Velpic SAML tile in hello Access Panel, you should get login page of Velpic SAML application.</span></span> <span data-ttu-id="cda24-253">Megtekintheti az hello **"Jelentkezzen be az Azure AD"** hello bejelentkezési oldal gombjára.</span><span class="sxs-lookup"><span data-stu-id="cda24-253">You should see hello **‘Log In With Azure AD’** button on hello sign in page.</span></span>

    ![Beépülő modul](./media/active-directory-saas-velpicsaml-tutorial/velpic_6.png)

2. <span data-ttu-id="cda24-255">Kattintson a hello **"Jelentkezzen be az Azure AD"** gomb toolog a tooVelpic az Azure AD-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="cda24-255">Click on hello **‘Log In With Azure AD’** button toolog in tooVelpic using your Azure AD account.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="cda24-256">További források</span><span class="sxs-lookup"><span data-stu-id="cda24-256">Additional resources</span></span>

* [<span data-ttu-id="cda24-257">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="cda24-257">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="cda24-258">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="cda24-258">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-velpicsaml-tutorial/tutorial_general_203.png

