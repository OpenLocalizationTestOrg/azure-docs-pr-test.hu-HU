---
title: "Oktatóanyag: Azure Active Directory-integráció a Atlassian felhőalapú |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Atlassian felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 729b8eb6-efc4-47fb-9f34-8998ca2c9545
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: f679e8b3306bf0efb9373d8baa0cfe095b760aaf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-atlassian-cloud"></a><span data-ttu-id="2f106-103">Oktatóanyag: Azure Active Directoryval integrált Atlassian felhő</span><span class="sxs-lookup"><span data-stu-id="2f106-103">Tutorial: Azure Active Directory integration with Atlassian Cloud</span></span>

<span data-ttu-id="2f106-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Atlassian felhőalapú Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="2f106-104">In this tutorial, you learn how toointegrate Atlassian Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="2f106-105">Atlassian felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="2f106-105">Integrating Atlassian Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="2f106-106">Megadhatja a hozzáférés tooAtlassian felhő rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2f106-106">You can control in Azure AD who has access tooAtlassian Cloud</span></span>
- <span data-ttu-id="2f106-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAtlassian felhő (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="2f106-107">You can enable your users tooautomatically get signed-on tooAtlassian Cloud (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="2f106-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="2f106-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="2f106-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="2f106-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2f106-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2f106-110">Prerequisites</span></span>

<span data-ttu-id="2f106-111">tooconfigure Atlassian felhőalapú Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="2f106-111">tooconfigure Azure AD integration with Atlassian Cloud, you need hello following items:</span></span>

- <span data-ttu-id="2f106-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="2f106-112">An Azure AD subscription</span></span>
- <span data-ttu-id="2f106-113">Egy Atlassian felhő egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="2f106-113">An Atlassian Cloud single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="2f106-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="2f106-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="2f106-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="2f106-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="2f106-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="2f106-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="2f106-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="2f106-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="2f106-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="2f106-118">Scenario description</span></span>
<span data-ttu-id="2f106-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="2f106-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="2f106-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="2f106-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="2f106-121">Hello gyűjteményből Atlassian felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f106-121">Adding Atlassian Cloud from hello gallery</span></span>
2. <span data-ttu-id="2f106-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2f106-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-atlassian-cloud-from-hello-gallery"></a><span data-ttu-id="2f106-123">Hello gyűjteményből Atlassian felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2f106-123">Adding Atlassian Cloud from hello gallery</span></span>
<span data-ttu-id="2f106-124">tooconfigure hello integrációs Atlassian felhőalapú, az Azure AD-be, meg kell tooadd Atlassian felhő hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="2f106-124">tooconfigure hello integration of Atlassian Cloud into Azure AD, you need tooadd Atlassian Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="2f106-125">**tooadd Atlassian felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2f106-125">**tooadd Atlassian Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f106-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2f106-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="2f106-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="2f106-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="2f106-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2f106-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="2f106-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="2f106-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="2f106-133">Hello keresési mezőbe, írja be a **Atlassian felhő**.</span><span class="sxs-lookup"><span data-stu-id="2f106-133">In hello search box, type **Atlassian Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_search.png)

5. <span data-ttu-id="2f106-135">A hello eredmények panelen válassza ki a **Atlassian felhő**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="2f106-135">In hello results panel, select **Atlassian Cloud**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="2f106-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="2f106-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="2f106-138">Ebben a szakaszban konfigurálhatja, és a Atlassian felhőalapú Azure AD az egyszeri bejelentkezés teszt "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="2f106-138">In this section, you configure and test Azure AD single sign-on with Atlassian Cloud based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="2f106-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Atlassian felhőben tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="2f106-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Atlassian Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="2f106-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Atlassian felhőben közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="2f106-140">In other words, a link relationship between an Azure AD user and hello related user in Atlassian Cloud needs toobe established.</span></span>

<span data-ttu-id="2f106-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Atlassian felhőben.</span><span class="sxs-lookup"><span data-stu-id="2f106-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Atlassian Cloud.</span></span>

<span data-ttu-id="2f106-142">tooconfigure és a Atlassian felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="2f106-142">tooconfigure and test Azure AD single sign-on with Atlassian Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="2f106-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="2f106-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="2f106-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="2f106-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="2f106-145">**[Egy Atlassian felhő tesztfelhasználó létrehozása](#creating-an-atlassian-cloud-test-user)**  -toohave egy megfelelője a Britta Simon Atlassian felhőben található, a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="2f106-145">**[Creating an Atlassian Cloud test user](#creating-an-atlassian-cloud-test-user)** - toohave a counterpart of Britta Simon in Atlassian Cloud that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="2f106-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2f106-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="2f106-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="2f106-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="2f106-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2f106-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="2f106-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Atlassian felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="2f106-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Atlassian Cloud application.</span></span>

<span data-ttu-id="2f106-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Atlassian felhőalapú, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2f106-150">**tooconfigure Azure AD single sign-on with Atlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f106-151">Az Azure portál, a hello hello **Atlassian felhő** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="2f106-151">In hello Azure portal, on hello **Atlassian Cloud** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="2f106-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="2f106-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_samlbase.png)

3. <span data-ttu-id="2f106-155">A hello **Atlassian felhőalapú tartományt és URL-címek** csoportjában hajtsa végre a következő lépéseket, ha tooconfigure hello alkalmazás hello **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="2f106-155">On hello **Atlassian Cloud Domain and URLs** section, perform hello following steps if you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url.png)

    <span data-ttu-id="2f106-157">a.</span><span class="sxs-lookup"><span data-stu-id="2f106-157">a.</span></span> <span data-ttu-id="2f106-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.atlassian.net/admin/saml/edit`</span><span class="sxs-lookup"><span data-stu-id="2f106-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net/admin/saml/edit`</span></span>

    <span data-ttu-id="2f106-159">b.</span><span class="sxs-lookup"><span data-stu-id="2f106-159">b.</span></span> <span data-ttu-id="2f106-160">A hello **válasz URL-CÍMEN** szövegmező, adja meg az URL-címet:`https://id.atlassian.com/login/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="2f106-160">In hello **Reply URL** textbox, type a URL as: `https://id.atlassian.com/login/saml/acs`</span></span>

4. <span data-ttu-id="2f106-161">Ellenőrizze **megjelenítése speciális URL-beállításainak** , és végezze el a következő lépés, ha tooconfigure hello alkalmazás hello **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="2f106-161">Check **Show advanced URL settings** and perform hello following step if you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_url1.png)

    <span data-ttu-id="2f106-163">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<instancename>.atlassian.net`</span><span class="sxs-lookup"><span data-stu-id="2f106-163">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<instancename>.atlassian.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="2f106-164">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="2f106-164">These values are not real.</span></span> <span data-ttu-id="2f106-165">Frissítse a azonosító és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="2f106-165">Update these values with hello actual Identifier and Sign-On URL.</span></span> <span data-ttu-id="2f106-166">Hello pontos értékek letölthető Atlassian felhő SAML konfigurálására szolgáló képernyőn.</span><span class="sxs-lookup"><span data-stu-id="2f106-166">You can get hello exact values from Atlassian Cloud SAML Configuration screen.</span></span>
 
5. <span data-ttu-id="2f106-167">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="2f106-167">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_certificate.png) 

6. <span data-ttu-id="2f106-169">A hello **Atlassian Felhőkonfiguráció** kattintson **Atlassian felhő konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="2f106-169">On hello **Atlassian Cloud Configuration** section, click **Configure Atlassian Cloud** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="2f106-170">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="2f106-170">Copy hello **SAML Entity ID and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_configure.png) 

7. <span data-ttu-id="2f106-172">tooget SSO konfigurálva az alkalmazáshoz, a bejelentkezési toohello Atlassian Portal hello rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="2f106-172">tooget SSO configured for your application, login toohello Atlassian Portal using hello administrator rights.</span></span>

8. <span data-ttu-id="2f106-173">Kattintson a bal oldali navigációs hello hello hitelesítési szakaszában **tartományok**.</span><span class="sxs-lookup"><span data-stu-id="2f106-173">In hello Authentication section of hello left navigation click **Domains**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_06.png)

    <span data-ttu-id="2f106-175">a.</span><span class="sxs-lookup"><span data-stu-id="2f106-175">a.</span></span> <span data-ttu-id="2f106-176">Hello szövegmezőben adja meg a tartomány nevét, és kattintson **tartomány hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="2f106-176">In hello textbox, type your domain name, and then click **Add domain**.</span></span>
        
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_07.png)

    <span data-ttu-id="2f106-178">b.</span><span class="sxs-lookup"><span data-stu-id="2f106-178">b.</span></span> <span data-ttu-id="2f106-179">tooverify hello tartomány, kattintson a **ellenőrizze**.</span><span class="sxs-lookup"><span data-stu-id="2f106-179">tooverify hello domain, click **Verify**.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_08.png)

    <span data-ttu-id="2f106-181">c.</span><span class="sxs-lookup"><span data-stu-id="2f106-181">c.</span></span> <span data-ttu-id="2f106-182">Töltse le a hello ellenőrzési html-fájlba, töltse fel a tartományi webhely gyökérmappájában toohello, és kattintson **tartomány hitelesítése**.</span><span class="sxs-lookup"><span data-stu-id="2f106-182">Download hello domain verification html file, upload it toohello root folder of your domain's website, and then click **Verify domain**.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_09.png)

    <span data-ttu-id="2f106-184">d.</span><span class="sxs-lookup"><span data-stu-id="2f106-184">d.</span></span> <span data-ttu-id="2f106-185">Miután hello tartomány ellenőrzése után hello hello értékének **állapot** mező **ellenőrizve**.</span><span class="sxs-lookup"><span data-stu-id="2f106-185">Once hello domain is verified, hello value of hello **Status** field is **Verified**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_10.png)

9. <span data-ttu-id="2f106-187">Hello bal oldali navigációs sávon kattintson **SAML**.</span><span class="sxs-lookup"><span data-stu-id="2f106-187">In hello left navigation bar, click **SAML**.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_11.png)

10. <span data-ttu-id="2f106-189">SAML-konfiguráció létrehozása, és adja hozzá a hello identitás szolgáltató konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2f106-189">Create a SAML Configuration and add hello Identity provider configuration.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_12.png)

    <span data-ttu-id="2f106-191">a.</span><span class="sxs-lookup"><span data-stu-id="2f106-191">a.</span></span> <span data-ttu-id="2f106-192">A hello **identitásszolgáltató Entitásazonosító** szövegmezőben, Beillesztés hello értékének **SAML Entitásazonosító** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="2f106-192">In hello **Identity provider Entity ID** text box, paste hello value of  **SAML Entity ID** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2f106-193">b.</span><span class="sxs-lookup"><span data-stu-id="2f106-193">b.</span></span> <span data-ttu-id="2f106-194">A hello **identitásszolgáltató egyszeri bejelentkezési URL-cím** szövegmezőben, Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="2f106-194">In hello **Identity provider SSO URL** text box, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>

    <span data-ttu-id="2f106-195">c.</span><span class="sxs-lookup"><span data-stu-id="2f106-195">c.</span></span> <span data-ttu-id="2f106-196">Nyissa meg a hello letöltött tanúsítvány az Azure portálon, és másolja hello értékek nélkül hello kezdő és záró sorok, és illessze be hello **nyilvános X509 tanúsítvány** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2f106-196">Open hello downloaded certificate from Azure portal and copy hello values without hello Begin and End lines and paste it in hello **Public X509 certificate** box.</span></span>
    
    <span data-ttu-id="2f106-197">d.</span><span class="sxs-lookup"><span data-stu-id="2f106-197">d.</span></span> <span data-ttu-id="2f106-198">Kattintson a **konfiguráció mentése** tooSave hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="2f106-198">Click **Save Configuration**  tooSave hello settings.</span></span>
     
11. <span data-ttu-id="2f106-199">Az Azure AD hello beállításait, hogy rendelkezik-e javítani azonosító URL-t telepítő hello toomake frissítése.</span><span class="sxs-lookup"><span data-stu-id="2f106-199">Update hello Azure AD settings toomake sure that you have setup hello correct Identifier URL.</span></span>
  
    <span data-ttu-id="2f106-200">a.</span><span class="sxs-lookup"><span data-stu-id="2f106-200">a.</span></span> <span data-ttu-id="2f106-201">Másolás hello **SP identitás azonosító** hello SAML a képernyőn, és illessze be az Azure AD szolgáltatásba hello **azonosító** érték.</span><span class="sxs-lookup"><span data-stu-id="2f106-201">Copy hello **SP Identity ID** from hello SAML screen and paste it in Azure AD as hello **Identifier** value.</span></span>

    <span data-ttu-id="2f106-202">b.</span><span class="sxs-lookup"><span data-stu-id="2f106-202">b.</span></span> <span data-ttu-id="2f106-203">URL-cím bejelentkezési hello bérlői URL-CÍMÉT a Atlassian felhőben.</span><span class="sxs-lookup"><span data-stu-id="2f106-203">Sign On URL is hello tenant URL of your Atlassian Cloud.</span></span>     

     ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_13.png)
    
12. <span data-ttu-id="2f106-205">Hello Azure-portálon, kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="2f106-205">In hello Azure portal, Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_400.png)

> [!TIP]
> <span data-ttu-id="2f106-207">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="2f106-207">You can now read a concise version of these instructions inside hello [Azure  portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="2f106-208">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="2f106-208">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="2f106-209">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="2f106-209">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="2f106-210">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f106-210">Creating an Azure AD test user</span></span>
<span data-ttu-id="2f106-211">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="2f106-211">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="2f106-213">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="2f106-213">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f106-214">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="2f106-214">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="2f106-216">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="2f106-216">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="2f106-218">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2f106-218">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="2f106-220">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="2f106-220">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="2f106-222">a.</span><span class="sxs-lookup"><span data-stu-id="2f106-222">a.</span></span> <span data-ttu-id="2f106-223">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="2f106-223">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="2f106-224">b.</span><span class="sxs-lookup"><span data-stu-id="2f106-224">b.</span></span> <span data-ttu-id="2f106-225">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="2f106-225">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="2f106-226">c.</span><span class="sxs-lookup"><span data-stu-id="2f106-226">c.</span></span> <span data-ttu-id="2f106-227">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="2f106-227">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="2f106-228">d.</span><span class="sxs-lookup"><span data-stu-id="2f106-228">d.</span></span> <span data-ttu-id="2f106-229">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2f106-229">Click **Create**.</span></span>
 
### <a name="creating-an-atlassian-cloud-test-user"></a><span data-ttu-id="2f106-230">Egy Atlassian felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="2f106-230">Creating an Atlassian Cloud test user</span></span>

<span data-ttu-id="2f106-231">az Azure AD tooenable felhasználók toolog a felhő tooAtlassian, akkor ki kell építenie Atlassian felhőbe.</span><span class="sxs-lookup"><span data-stu-id="2f106-231">tooenable Azure AD users toolog in tooAtlassian Cloud, they must be provisioned into Atlassian Cloud.</span></span>  
<span data-ttu-id="2f106-232">Atlassian felhő esetén kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="2f106-232">In case of Atlassian Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="2f106-233">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="2f106-233">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f106-234">A hely felügyeleti szakasz hello, kattintson a hello **felhasználók** gomb</span><span class="sxs-lookup"><span data-stu-id="2f106-234">In hello Site administration section, click hello **Users** button</span></span>

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_14.png) 

2. <span data-ttu-id="2f106-236">Kattintson a hello **felhasználó létrehozása** gomb toocreate hello Atlassian felhő felhasználójának</span><span class="sxs-lookup"><span data-stu-id="2f106-236">Click hello **Create User** button toocreate a user in hello Atlassian Cloud</span></span>

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_15.png) 

3. <span data-ttu-id="2f106-238">Adja meg hello felhasználó **E-mail cím**, **felhasználónév**, és **teljes nevét** , és rendelje hozzá a hello alkalmazás-hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="2f106-238">Enter hello user's **Email address**, **Username**, and **Full Name** and assign hello application access.</span></span> 

    ![Atlassian felhő felhasználó létrehozása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_16.png)
 
4. <span data-ttu-id="2f106-240">Kattintson a **a felhasználó létrehozása** gomb, hello e-mail meghívó toohello felhasználó elküldi és elfogadása után hello meghívó hello felhasználói hello rendszerben aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="2f106-240">Click **Create user** button, it will send hello email invitation toohello user and after accepting hello invitation hello user will be active in hello system.</span></span> 

>[!NOTE] 
><span data-ttu-id="2f106-241">Is létrehozhat hello tömeges felhasználók hello kattintva **tömeges létrehozása** gombra a felhasználók szakaszban hello.</span><span class="sxs-lookup"><span data-stu-id="2f106-241">You can also create hello bulk users by clicking hello **Bulk Create** button in hello Users section.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="2f106-242">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="2f106-242">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="2f106-243">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooAtlassian felhő megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="2f106-243">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAtlassian Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="2f106-245">**tooassign Britta Simon tooAtlassian felhő, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="2f106-245">**tooassign Britta Simon tooAtlassian Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="2f106-246">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="2f106-246">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="2f106-248">Hello alkalmazások listában válassza ki a **Atlassian felhő**.</span><span class="sxs-lookup"><span data-stu-id="2f106-248">In hello applications list, select **Atlassian Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_atlassiancloud_app.png) 

3. <span data-ttu-id="2f106-250">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="2f106-250">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="2f106-252">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="2f106-252">Click **Add** button.</span></span> <span data-ttu-id="2f106-253">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2f106-253">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="2f106-255">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="2f106-255">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="2f106-256">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2f106-256">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="2f106-257">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2f106-257">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="2f106-258">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="2f106-258">Testing single sign-on</span></span>

<span data-ttu-id="2f106-259">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="2f106-259">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="2f106-260">Hello Atlassian felhő hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour Atlassian felhőalapú alkalmazásnál.</span><span class="sxs-lookup"><span data-stu-id="2f106-260">When you click hello Atlassian Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Atlassian Cloud application.</span></span> <span data-ttu-id="2f106-261">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="2f106-261">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="2f106-262">További források</span><span class="sxs-lookup"><span data-stu-id="2f106-262">Additional resources</span></span>

* [<span data-ttu-id="2f106-263">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="2f106-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="2f106-264">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="2f106-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-atlassian-cloud-tutorial/tutorial_general_203.png

