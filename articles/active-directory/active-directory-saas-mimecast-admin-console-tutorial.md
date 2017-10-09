---
title: "Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a felügyeleti konzol Mimecast között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 81c50614-f49b-4bbc-97d5-3cf77154305f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: jeedes
ms.openlocfilehash: 5a04a5abd9ff30d484bce0a5c97a1d3e48b69e4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mimecast-admin-console"></a><span data-ttu-id="42e36-103">Oktatóanyag: Azure Active Directoryval integrált Mimecast felügyeleti konzol</span><span class="sxs-lookup"><span data-stu-id="42e36-103">Tutorial: Azure Active Directory integration with Mimecast Admin Console</span></span>

<span data-ttu-id="42e36-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mimecast felügyeleti konzol az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="42e36-104">In this tutorial, you learn how toointegrate Mimecast Admin Console with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="42e36-105">Felügyeleti konzol Mimecast integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="42e36-105">Integrating Mimecast Admin Console with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="42e36-106">Az Azure AD hozzáférési tooMimecast felügyeleti konzol rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="42e36-106">You can control in Azure AD who has access tooMimecast Admin Console.</span></span>
- <span data-ttu-id="42e36-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMimecast (egyszeri bejelentkezés) felügyeleti konzol az Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="42e36-107">You can enable your users tooautomatically get signed-on tooMimecast Admin Console (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="42e36-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="42e36-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="42e36-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="42e36-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="42e36-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="42e36-110">Prerequisites</span></span>

<span data-ttu-id="42e36-111">tooconfigure Mimecast felügyeleti konzol az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="42e36-111">tooconfigure Azure AD integration with Mimecast Admin Console, you need hello following items:</span></span>

- <span data-ttu-id="42e36-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="42e36-112">An Azure AD subscription</span></span>
- <span data-ttu-id="42e36-113">A felügyeleti konzol Mimecast egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="42e36-113">A Mimecast Admin Console single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="42e36-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="42e36-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="42e36-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="42e36-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="42e36-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="42e36-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="42e36-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="42e36-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="42e36-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="42e36-118">Scenario description</span></span>
<span data-ttu-id="42e36-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="42e36-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="42e36-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="42e36-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="42e36-121">Hello gyűjteményből Mimecast felügyeleti konzol hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42e36-121">Adding Mimecast Admin Console from hello gallery</span></span>
2. <span data-ttu-id="42e36-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="42e36-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mimecast-admin-console-from-hello-gallery"></a><span data-ttu-id="42e36-123">Hello gyűjteményből Mimecast felügyeleti konzol hozzáadása</span><span class="sxs-lookup"><span data-stu-id="42e36-123">Adding Mimecast Admin Console from hello gallery</span></span>
<span data-ttu-id="42e36-124">tooconfigure hello integrációja Mimecast felügyeleti konzol az Azure AD-be, meg kell tooadd Mimecast felügyeleti konzol hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="42e36-124">tooconfigure hello integration of Mimecast Admin Console into Azure AD, you need tooadd Mimecast Admin Console from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="42e36-125">**tooadd Mimecast felügyeleti konzol hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="42e36-125">**tooadd Mimecast Admin Console from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="42e36-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="42e36-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="42e36-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="42e36-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="42e36-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42e36-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="42e36-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="42e36-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="42e36-133">Hello keresési mezőbe, írja be a **Mimecast felügyeleti konzol**, jelölje be **Mimecast felügyeleti konzol** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-133">In hello search box, type **Mimecast Admin Console**, select **Mimecast Admin Console** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában Mimecast felügyeleti konzol](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="42e36-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42e36-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="42e36-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Mimecast felügyeleti konzol "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="42e36-136">In this section, you configure and test Azure AD single sign-on with Mimecast Admin Console based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="42e36-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Mimecast felügyeleti konzolon az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="42e36-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mimecast Admin Console is tooa user in Azure AD.</span></span> <span data-ttu-id="42e36-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Mimecast felügyeleti konzol közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="42e36-138">In other words, a link relationship between an Azure AD user and hello related user in Mimecast Admin Console needs toobe established.</span></span>

<span data-ttu-id="42e36-139">Mimecast felügyeleti konzolon, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="42e36-139">In Mimecast Admin Console, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="42e36-140">tooconfigure és Mimecast felügyeleti konzol az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="42e36-140">tooconfigure and test Azure AD single sign-on with Mimecast Admin Console, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="42e36-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="42e36-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="42e36-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="42e36-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="42e36-143">**[Felügyeleti konzol Mimecast tesztfelhasználó létrehozása](#create-a-mimecast-admin-console-test-user)**  -toohave egy megfelelője a Britta Simon Mimecast felügyeleti konzolon láthatja, hogy felhasználói csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="42e36-143">**[Create a Mimecast Admin Console test user](#create-a-mimecast-admin-console-test-user)** - toohave a counterpart of Britta Simon in Mimecast Admin Console that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="42e36-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="42e36-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="42e36-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="42e36-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="42e36-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="42e36-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="42e36-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a felügyeleti konzol Mimecast alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="42e36-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mimecast Admin Console application.</span></span>

<span data-ttu-id="42e36-148">**Mimecast felügyeleti konzolt, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="42e36-148">**tooconfigure Azure AD single sign-on with Mimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="42e36-149">Az Azure portál, a hello hello **Mimecast felügyeleti konzol** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="42e36-149">In hello Azure portal, on hello **Mimecast Admin Console** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="42e36-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="42e36-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_samlbase.png)

3. <span data-ttu-id="42e36-153">A hello **Mimecast felügyeleti konzol tartományát és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="42e36-153">On hello **Mimecast Admin Console Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Mimecast felügyeleti konzol tartományát és URL-címek](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_url.png)

    <span data-ttu-id="42e36-155">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:</span><span class="sxs-lookup"><span data-stu-id="42e36-155">In hello **Sign-on URL** textbox, type hello URL:</span></span>
    | |
    | -- |
    | `https://webmail-uk.mimecast.com`|
    | `https://webmail-us.mimecast.com`|

    > [!NOTE] 
    > <span data-ttu-id="42e36-156">hello bejelentkezési URL-címet az adott régió.</span><span class="sxs-lookup"><span data-stu-id="42e36-156">hello sign on URL is region specific.</span></span>

4. <span data-ttu-id="42e36-157">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="42e36-157">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_certificate.png) 

5. <span data-ttu-id="42e36-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="42e36-161">A hello **Mimecast felügyeleti konzol konfiguráció** területén kattintson **konfigurálása Mimecast felügyeleti konzol** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="42e36-161">On hello **Mimecast Admin Console Configuration** section, click **Configure Mimecast Admin Console** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="42e36-162">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="42e36-162">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Mimecast felügyeleti konzol konfiguráció](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_configure.png) 

7. <span data-ttu-id="42e36-164">Egy másik webes böngészőablakban jelentkezzen be a Mimecast felügyeleti konzolt rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="42e36-164">In a different web browser window, log into your Mimecast Admin Console as an administrator.</span></span>

8. <span data-ttu-id="42e36-165">Nyissa meg túl**szolgáltatások \> alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42e36-165">Go too**Services \> Application**.</span></span>

    <span data-ttu-id="42e36-166">![Szolgáltatások](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "szolgáltatások")</span><span class="sxs-lookup"><span data-stu-id="42e36-166">![Services](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794998.png "Services")</span></span>

9. <span data-ttu-id="42e36-167">Kattintson a **hitelesítési profilok**.</span><span class="sxs-lookup"><span data-stu-id="42e36-167">Click **Authentication Profiles**.</span></span>

    <span data-ttu-id="42e36-168">![Hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="42e36-168">![Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic794999.png "Authentication Profiles")</span></span>
    
10. <span data-ttu-id="42e36-169">Kattintson a **új hitelesítési profil**.</span><span class="sxs-lookup"><span data-stu-id="42e36-169">Click **New Authentication Profile**.</span></span>

    <span data-ttu-id="42e36-170">![Új hitelesítési profilok](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "új hitelesítési profilok")</span><span class="sxs-lookup"><span data-stu-id="42e36-170">![New Authentication Profiles](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795000.png "New Authentication Profiles")</span></span>

11. <span data-ttu-id="42e36-171">A hello **hitelesítési profil** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="42e36-171">In hello **Authentication Profile** section, perform hello following steps:</span></span>

    <span data-ttu-id="42e36-172">![Hitelesítési profil](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "hitelesítési profil")</span><span class="sxs-lookup"><span data-stu-id="42e36-172">![Authentication Profile](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795015.png "Authentication Profile")</span></span>
    
    <span data-ttu-id="42e36-173">a.</span><span class="sxs-lookup"><span data-stu-id="42e36-173">a.</span></span> <span data-ttu-id="42e36-174">A hello **leírás** szövegmező, adja meg a konfiguráció nevét.</span><span class="sxs-lookup"><span data-stu-id="42e36-174">In hello **Description** textbox, type a name for your configuration.</span></span>
    
    <span data-ttu-id="42e36-175">b.</span><span class="sxs-lookup"><span data-stu-id="42e36-175">b.</span></span> <span data-ttu-id="42e36-176">Válassza ki **Mimecast felügyeleti konzol az SAML-alapú hitelesítés kényszerítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="42e36-176">Select **Enforce SAML Authentication for Mimecast Admin Console**.</span></span>
    
    <span data-ttu-id="42e36-177">c.</span><span class="sxs-lookup"><span data-stu-id="42e36-177">c.</span></span> <span data-ttu-id="42e36-178">Mint **szolgáltató**, jelölje be **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="42e36-178">As **Provider**, select **Azure Active Directory**.</span></span>
    
    <span data-ttu-id="42e36-179">d.</span><span class="sxs-lookup"><span data-stu-id="42e36-179">d.</span></span> <span data-ttu-id="42e36-180">Beillesztés **SAML Entitásazonosító**, amely akkor másolta, az Azure-portálon hello hello **kiállítójának URL-címe** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="42e36-180">Paste **SAML Entity ID**, which you have copied from hello Azure portal into hello **Issuer URL** textbox.</span></span>
    
    <span data-ttu-id="42e36-181">e.</span><span class="sxs-lookup"><span data-stu-id="42e36-181">e.</span></span> <span data-ttu-id="42e36-182">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="42e36-182">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Login URL** textbox.</span></span>

    <span data-ttu-id="42e36-183">f.</span><span class="sxs-lookup"><span data-stu-id="42e36-183">f.</span></span> <span data-ttu-id="42e36-184">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **kijelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="42e36-184">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **Logout URL** textbox.</span></span>
    
    >[!NOTE]
    ><span data-ttu-id="42e36-185">hello bejelentkezési URL-címe és hello kijelentkezési URL-cím értéke esetében a hello Mimecast felügyeleti konzol hello azonos.</span><span class="sxs-lookup"><span data-stu-id="42e36-185">hello Login URL value and hello Logout URL value are for hello Mimecast Admin Console hello same.</span></span>
    
    <span data-ttu-id="42e36-186">g.</span><span class="sxs-lookup"><span data-stu-id="42e36-186">g.</span></span> <span data-ttu-id="42e36-187">Nyissa meg a Jegyzettömbben az Azure portálról letöltött a base-64 tanúsítvány hello első sor eltávolítása ("*--*") és az utolsó sora hello ("*--*"), hátralévő részét tartalom másolása hello a vágólapra majd illessze be azt toohello **Identitástanúsítvány szolgáltató (Metadata)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="42e36-187">Open your base-64 certificate downloaded from Azure portal in notepad, remove hello first line (“*--*“) and hello last line (“*--*“), copy hello remaining content of it into your clipboard, and then paste it toohello **Identity Provider Certificate (Metadata)** textbox.</span></span>
    
    <span data-ttu-id="42e36-188">h.</span><span class="sxs-lookup"><span data-stu-id="42e36-188">h.</span></span> <span data-ttu-id="42e36-189">Válassza ki **engedélyezése egyszeri bejelentkezéshez**.</span><span class="sxs-lookup"><span data-stu-id="42e36-189">Select **Allow Single Sign On**.</span></span>
    
    <span data-ttu-id="42e36-190">i.</span><span class="sxs-lookup"><span data-stu-id="42e36-190">i.</span></span> <span data-ttu-id="42e36-191">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-191">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="42e36-192">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="42e36-192">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="42e36-193">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="42e36-193">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="42e36-194">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="42e36-194">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="42e36-195">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="42e36-195">Create an Azure AD test user</span></span>

<span data-ttu-id="42e36-196">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="42e36-196">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="42e36-198">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="42e36-198">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="42e36-199">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-199">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="42e36-201">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="42e36-201">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="42e36-203">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="42e36-203">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="42e36-205">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="42e36-205">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-mimecast-admin-console-tutorial/create_aaduser_04.png)

    <span data-ttu-id="42e36-207">a.</span><span class="sxs-lookup"><span data-stu-id="42e36-207">a.</span></span> <span data-ttu-id="42e36-208">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="42e36-208">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="42e36-209">b.</span><span class="sxs-lookup"><span data-stu-id="42e36-209">b.</span></span> <span data-ttu-id="42e36-210">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="42e36-210">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="42e36-211">c.</span><span class="sxs-lookup"><span data-stu-id="42e36-211">c.</span></span> <span data-ttu-id="42e36-212">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="42e36-212">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="42e36-213">d.</span><span class="sxs-lookup"><span data-stu-id="42e36-213">d.</span></span> <span data-ttu-id="42e36-214">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-214">Click **Create**.</span></span>
 
### <a name="create-a-mimecast-admin-console-test-user"></a><span data-ttu-id="42e36-215">Felügyeleti konzol Mimecast tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="42e36-215">Create a Mimecast Admin Console test user</span></span>

<span data-ttu-id="42e36-216">A sorrend tooenable az Azure AD felhasználók toolog Mimecast felügyeleti konzolhoz azok kell üzembe Mimecast felügyeleti konzolhoz.</span><span class="sxs-lookup"><span data-stu-id="42e36-216">In order tooenable Azure AD users toolog into Mimecast Admin Console, they must be provisioned into Mimecast Admin Console.</span></span> <span data-ttu-id="42e36-217">Felügyeleti konzol Mimecast hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="42e36-217">In hello case of Mimecast Admin Console, provisioning is a manual task.</span></span>

* <span data-ttu-id="42e36-218">Felhasználók létrehozása előtt kell tooregister egy tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="42e36-218">You need tooregister a domain before you can create users.</span></span>

<span data-ttu-id="42e36-219">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="42e36-219">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="42e36-220">Jelentkezzen be tooyour **Mimecast felügyeleti konzol** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="42e36-220">Sign on tooyour **Mimecast Admin Console** as administrator.</span></span>
2. <span data-ttu-id="42e36-221">Nyissa meg túl**könyvtárak \> belső**.</span><span class="sxs-lookup"><span data-stu-id="42e36-221">Go too**Directories \> Internal**.</span></span>
   
   <span data-ttu-id="42e36-222">![Könyvtárak](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "könyvtárak")</span><span class="sxs-lookup"><span data-stu-id="42e36-222">![Directories](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795003.png "Directories")</span></span>
3. <span data-ttu-id="42e36-223">Kattintson a **regisztrálni az új tartomány**.</span><span class="sxs-lookup"><span data-stu-id="42e36-223">Click **Register New Domain**.</span></span>
   
   <span data-ttu-id="42e36-224">![Új tartomány regisztrálása](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "új tartomány regisztrálása")</span><span class="sxs-lookup"><span data-stu-id="42e36-224">![Register New Domain](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795004.png "Register New Domain")</span></span>
4. <span data-ttu-id="42e36-225">Az új tartomány létrehozása után kattintson **új cím**.</span><span class="sxs-lookup"><span data-stu-id="42e36-225">After your new domain has been created, click **New Address**.</span></span>
   
   <span data-ttu-id="42e36-226">![Új cím](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "új cím")</span><span class="sxs-lookup"><span data-stu-id="42e36-226">![New Address](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795005.png "New Address")</span></span>
5. <span data-ttu-id="42e36-227">Hello új cím párbeszédpanelen hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="42e36-227">In hello new address dialog, perform hello following steps:</span></span>
   
   <span data-ttu-id="42e36-228">![Mentés](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "mentése")</span><span class="sxs-lookup"><span data-stu-id="42e36-228">![Save](./media/active-directory-saas-mimecast-admin-console-tutorial/ic795006.png "Save")</span></span>
   
   <span data-ttu-id="42e36-229">a.</span><span class="sxs-lookup"><span data-stu-id="42e36-229">a.</span></span> <span data-ttu-id="42e36-230">Típus hello **E-mail cím**, **globális név**, **jelszó**, és **jelszó megerősítése** attribútumok egy érvényes Azure ad fiókot, hogy szeretné, hogy a hello tooprovision kapcsolatos szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="42e36-230">Type hello **Email Address**, **Global Name**, **Password**, and **Confirm Password** attributes of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span>

   <span data-ttu-id="42e36-231">b.</span><span class="sxs-lookup"><span data-stu-id="42e36-231">b.</span></span> <span data-ttu-id="42e36-232">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-232">Click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="42e36-233">Mimecast felügyeleti konzol felhasználói fiók létrehozása eszközök vagy Mimecast felügyeleti konzol tooprovision az Azure AD felhasználói fiókok által nyújtott API-kat is használhatja.</span><span class="sxs-lookup"><span data-stu-id="42e36-233">You can use any other Mimecast Admin Console user account creation tools or APIs provided by Mimecast Admin Console tooprovision Azure AD user accounts.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="42e36-234">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="42e36-234">Assign hello Azure AD test user</span></span>

<span data-ttu-id="42e36-235">Ebben a szakaszban Britta Simon toouse Azure egyszeri bejelentkezés azáltal, hogy biztosítja a hozzáférés tooMimecast felügyeleti konzol engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="42e36-235">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMimecast Admin Console.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="42e36-237">**tooassign Britta Simon tooMimecast felügyeleti konzol, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="42e36-237">**tooassign Britta Simon tooMimecast Admin Console, perform hello following steps:**</span></span>

1. <span data-ttu-id="42e36-238">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="42e36-238">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="42e36-240">Hello alkalmazások listában válassza ki a **Mimecast felügyeleti konzol**.</span><span class="sxs-lookup"><span data-stu-id="42e36-240">In hello applications list, select **Mimecast Admin Console**.</span></span>

    ![hello Mimecast felügyeleti konzol hivatkozásra hello alkalmazások listája](./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_mimecastadminconsole_app.png)  

3. <span data-ttu-id="42e36-242">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="42e36-242">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="42e36-244">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="42e36-244">Click **Add** button.</span></span> <span data-ttu-id="42e36-245">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42e36-245">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="42e36-247">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="42e36-247">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="42e36-248">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42e36-248">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="42e36-249">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="42e36-249">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="42e36-250">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="42e36-250">Test single sign-on</span></span>

<span data-ttu-id="42e36-251">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="42e36-251">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="42e36-252">Ha a hozzáférési Panel hello hello Mimecast felügyeleti konzol csempe gombra kattint, automatikusan bejelentkezett tooyour Mimecast felügyeleti konzol alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="42e36-252">When you click hello Mimecast Admin Console tile in hello Access Panel, you should get automatically signed-on tooyour Mimecast Admin Console application.</span></span>
<span data-ttu-id="42e36-253">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="42e36-253">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="42e36-254">További források</span><span class="sxs-lookup"><span data-stu-id="42e36-254">Additional resources</span></span>

* [<span data-ttu-id="42e36-255">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="42e36-255">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="42e36-256">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="42e36-256">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mimecast-admin-console-tutorial/tutorial_general_203.png

