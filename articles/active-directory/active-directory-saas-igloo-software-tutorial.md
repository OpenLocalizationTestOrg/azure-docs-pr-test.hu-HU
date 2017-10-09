---
title: "Oktatóanyag: Azure Active Directory-integráció Igloo szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Igloo szoftverek között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 406405d4faa6e56f1005a61e69a29ef2ef2eb34b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a><span data-ttu-id="d23c7-103">Oktatóanyag: Azure Active Directory-integráció Igloo szoftverrel</span><span class="sxs-lookup"><span data-stu-id="d23c7-103">Tutorial: Azure Active Directory integration with Igloo Software</span></span>

<span data-ttu-id="d23c7-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Igloo szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d23c7-104">In this tutorial, you learn how toointegrate Igloo Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d23c7-105">Igloo szoftver integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-105">Integrating Igloo Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d23c7-106">Megadhatja a hozzáférés tooIgloo szoftver rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d23c7-106">You can control in Azure AD who has access tooIgloo Software</span></span>
- <span data-ttu-id="d23c7-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooIgloo szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d23c7-107">You can enable your users tooautomatically get signed-on tooIgloo Software (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d23c7-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d23c7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d23c7-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d23c7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d23c7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d23c7-110">Prerequisites</span></span>

<span data-ttu-id="d23c7-111">tooconfigure Igloo szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="d23c7-111">tooconfigure Azure AD integration with Igloo Software, you need hello following items:</span></span>

- <span data-ttu-id="d23c7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d23c7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d23c7-113">Egy Igloo szoftver egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d23c7-113">An Igloo Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d23c7-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d23c7-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d23c7-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d23c7-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d23c7-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d23c7-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d23c7-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d23c7-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d23c7-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d23c7-118">Scenario description</span></span>
<span data-ttu-id="d23c7-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d23c7-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d23c7-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d23c7-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d23c7-121">Hello gyűjteményből Igloo szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23c7-121">Adding Igloo Software from hello gallery</span></span>
2. <span data-ttu-id="d23c7-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d23c7-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-igloo-software-from-hello-gallery"></a><span data-ttu-id="d23c7-123">Hello gyűjteményből Igloo szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d23c7-123">Adding Igloo Software from hello gallery</span></span>
<span data-ttu-id="d23c7-124">tooconfigure hello integrációs Igloo szoftver az Azure AD-be, meg kell tooadd Igloo szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d23c7-124">tooconfigure hello integration of Igloo Software into Azure AD, you need tooadd Igloo Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d23c7-125">**tooadd Igloo szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d23c7-125">**tooadd Igloo Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d23c7-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d23c7-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d23c7-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d23c7-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d23c7-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d23c7-133">Hello keresési mezőbe, írja be a **Igloo szoftver**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-133">In hello search box, type **Igloo Software**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_search.png)

5. <span data-ttu-id="d23c7-135">A hello eredmények panelen válassza ki a **Igloo szoftver**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-135">In hello results panel, select **Igloo Software**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d23c7-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d23c7-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d23c7-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Igloo szoftverrel.</span><span class="sxs-lookup"><span data-stu-id="d23c7-138">In this section, you configure and test Azure AD single sign-on with Igloo Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d23c7-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Igloo szoftverfrissítési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d23c7-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Igloo Software is tooa user in Azure AD.</span></span> <span data-ttu-id="d23c7-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Igloo szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d23c7-140">In other words, a link relationship between an Azure AD user and hello related user in Igloo Software needs toobe established.</span></span>

<span data-ttu-id="d23c7-141">Igloo szoftver, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d23c7-141">In Igloo Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d23c7-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Igloo szoftverrel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d23c7-142">tooconfigure and test Azure AD single sign-on with Igloo Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d23c7-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d23c7-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d23c7-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d23c7-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d23c7-145">**[Egy Igloo szoftver tesztfelhasználó létrehozása](#creating-an-igloo-software-test-user)**  -toohave egy megfelelője a Britta Simon Igloo szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d23c7-145">**[Creating an Igloo Software test user](#creating-an-igloo-software-test-user)** - toohave a counterpart of Britta Simon in Igloo Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d23c7-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d23c7-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d23c7-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d23c7-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d23c7-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d23c7-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d23c7-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Igloo alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="d23c7-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Igloo Software application.</span></span>

<span data-ttu-id="d23c7-150">**az Azure AD tooconfigure egyszeri bejelentkezés Igloo szoftvert, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d23c7-150">**tooconfigure Azure AD single sign-on with Igloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="d23c7-151">Az Azure portál, a hello hello **Igloo szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-151">In hello Azure portal, on hello **Igloo Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d23c7-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d23c7-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_samlbase.png)

3. <span data-ttu-id="d23c7-155">A hello **Igloo szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-155">On hello **Igloo Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_url.png)
    
    <span data-ttu-id="d23c7-157">a.</span><span class="sxs-lookup"><span data-stu-id="d23c7-157">a.</span></span> <span data-ttu-id="d23c7-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.igloocommmunities.com`</span><span class="sxs-lookup"><span data-stu-id="d23c7-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com`</span></span>

    <span data-ttu-id="d23c7-159">b.</span><span class="sxs-lookup"><span data-stu-id="d23c7-159">b.</span></span> <span data-ttu-id="d23c7-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="d23c7-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    <span data-ttu-id="d23c7-161">c.</span><span class="sxs-lookup"><span data-stu-id="d23c7-161">c.</span></span> <span data-ttu-id="d23c7-162">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company name>.igloocommmunities.com/saml.digest`</span><span class="sxs-lookup"><span data-stu-id="d23c7-162">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<company name>.igloocommmunities.com/saml.digest`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d23c7-163">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d23c7-163">These values are not real.</span></span> <span data-ttu-id="d23c7-164">Frissítse a azonosító, a válasz URL-CÍMEN és a bejelentkezési URL-cím a tényleges hello értékeket.</span><span class="sxs-lookup"><span data-stu-id="d23c7-164">Update these values with hello actual Identifier, Reply URL, and Sign-On URL.</span></span> <span data-ttu-id="d23c7-165">Ügyfél [Igloo szoftver ügyfél-támogatási csoport](https://www.igloosoftware.com/services/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d23c7-165">Contact [Igloo Software Client support team](https://www.igloosoftware.com/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="d23c7-166">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d23c7-166">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_certificate.png) 

5. <span data-ttu-id="d23c7-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="d23c7-170">A hello **Igloo szoftverkonfigurációt** kattintson **Igloo szoftver konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d23c7-170">On hello **Igloo Software Configuration** section, click **Configure Igloo Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d23c7-171">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d23c7-171">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_configure.png) 

7. <span data-ttu-id="d23c7-173">Egy másik webes böngészőablakban jelentkezzen tooyour Igloo szoftver vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d23c7-173">In a different web browser window, log in tooyour Igloo Software company site as an administrator.</span></span>

8. <span data-ttu-id="d23c7-174">Nyissa meg toohello **Vezérlőpult**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-174">Go toohello **Control Panel**.</span></span>
   
     <span data-ttu-id="d23c7-175">![Vezérlőpult](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "vezérlőpultot")</span><span class="sxs-lookup"><span data-stu-id="d23c7-175">![Control Panel](./media/active-directory-saas-igloo-software-tutorial/ic799949.png "Control Panel")</span></span>

9. <span data-ttu-id="d23c7-176">A hello **tagsági** lapra, majd **beállításaival bejelentkezési**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-176">In hello **Membership** tab, click **Sign In Settings**.</span></span>
   
    <span data-ttu-id="d23c7-177">![Jelentkezzen be a beállítások](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "beállítások bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="d23c7-177">![Sign in Settings](./media/active-directory-saas-igloo-software-tutorial/ic783968.png "Sign in Settings")</span></span>

10. <span data-ttu-id="d23c7-178">Kattintson a SAML-alapú konfigurációs szakasz hello, **SAML-alapú hitelesítés beállítása**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-178">In hello SAML Configuration section, click **Configure SAML Authentication**.</span></span>
   
    <span data-ttu-id="d23c7-179">![SAML-alapú konfigurációs](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML-konfigurációja")</span><span class="sxs-lookup"><span data-stu-id="d23c7-179">![SAML Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783969.png "SAML Configuration")</span></span>
   
11. <span data-ttu-id="d23c7-180">A hello **általános konfigurációs** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-180">In hello **General Configuration** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="d23c7-181">![Általános konfiguráció](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "általános konfiguráció")</span><span class="sxs-lookup"><span data-stu-id="d23c7-181">![General Configuration](./media/active-directory-saas-igloo-software-tutorial/ic783970.png "General Configuration")</span></span>

    <span data-ttu-id="d23c7-182">a.</span><span class="sxs-lookup"><span data-stu-id="d23c7-182">a.</span></span> <span data-ttu-id="d23c7-183">A hello **kapcsolatnév** szövegmező, adjon meg egy egyéni nevet, a konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="d23c7-183">In hello **Connection Name** textbox, type a custom name for your configuration.</span></span>
   
    <span data-ttu-id="d23c7-184">b.</span><span class="sxs-lookup"><span data-stu-id="d23c7-184">b.</span></span> <span data-ttu-id="d23c7-185">A hello **IdP bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d23c7-185">In hello **IdP Login URL** textbox, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="d23c7-186">c.</span><span class="sxs-lookup"><span data-stu-id="d23c7-186">c.</span></span> <span data-ttu-id="d23c7-187">A hello **IdP kijelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d23c7-187">In hello **IdP Logout URL** textbox, paste hello value of **Sign-Out URL** which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="d23c7-188">d.</span><span class="sxs-lookup"><span data-stu-id="d23c7-188">d.</span></span> <span data-ttu-id="d23c7-189">Válassza ki **kijelentkezési válasz és kérelem HTTP típusú** , **POST**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-189">Select **Logout Response and Request HTTP Type** as **POST**.</span></span>
   
    <span data-ttu-id="d23c7-190">e.</span><span class="sxs-lookup"><span data-stu-id="d23c7-190">e.</span></span> <span data-ttu-id="d23c7-191">Nyissa meg a **base-64** kódolású tanúsítvány a Jegyzettömbben az Azure portálról, a vágólapra tartalmának másolása hello letöltött és toohello Beillesztés **nyilvános tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d23c7-191">Open your **base-64** encoded certificate in notepad downloaded from Azure portal, copy hello content of it into your clipboard, and then paste it toohello **Public Certificate** textbox.</span></span>
    
12. <span data-ttu-id="d23c7-192">A hello **válasz és a hitelesítési beállításokat**, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-192">In hello **Response and Authentication Configuration**, perform hello following steps:</span></span>
    
    <span data-ttu-id="d23c7-193">![Válasz és a hitelesítési beállításokat](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "válasz és hitelesítés konfigurációját.")</span><span class="sxs-lookup"><span data-stu-id="d23c7-193">![Response and Authentication Configuration](./media/active-directory-saas-igloo-software-tutorial/IC783971.png "Response and Authentication Configuration")</span></span>
  
      <span data-ttu-id="d23c7-194">a.</span><span class="sxs-lookup"><span data-stu-id="d23c7-194">a.</span></span> <span data-ttu-id="d23c7-195">Mint **identitásszolgáltató**, jelölje be **Microsoft ADFS**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-195">As **Identity Provider**, select **Microsoft ADFS**.</span></span>
      
      <span data-ttu-id="d23c7-196">b.</span><span class="sxs-lookup"><span data-stu-id="d23c7-196">b.</span></span> <span data-ttu-id="d23c7-197">Mint **azonosítótípusa**, jelölje be **E-mail cím**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-197">As **Identifier Type**, select **Email Address**.</span></span> 

      <span data-ttu-id="d23c7-198">c.</span><span class="sxs-lookup"><span data-stu-id="d23c7-198">c.</span></span> <span data-ttu-id="d23c7-199">A hello **E-mail attribútum** szövegmezőhöz típus **emailaddress**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-199">In hello **Email Attribute** textbox, type **emailaddress**.</span></span>

      <span data-ttu-id="d23c7-200">d.</span><span class="sxs-lookup"><span data-stu-id="d23c7-200">d.</span></span> <span data-ttu-id="d23c7-201">A hello **Keresztnév attribútum** szövegmezőhöz típus **givenname**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-201">In hello **First Name Attribute** textbox, type **givenname**.</span></span>

      <span data-ttu-id="d23c7-202">e.</span><span class="sxs-lookup"><span data-stu-id="d23c7-202">e.</span></span> <span data-ttu-id="d23c7-203">A hello **utolsó Name attribútum** szövegmezőhöz típus **vezetékneve**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-203">In hello **Last Name Attribute** textbox, type **surname**.</span></span>

13. <span data-ttu-id="d23c7-204">Hajtsa végre a következő lépéseket toocomplete hello konfigurációs hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-204">Perform hello following steps toocomplete hello configuration:</span></span>
    
    <span data-ttu-id="d23c7-205">![Jelentkezzen be a felhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "jelentkezzen be a felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="d23c7-205">![User creation on Sign in](./media/active-directory-saas-igloo-software-tutorial/IC783972.png "User creation on Sign in")</span></span> 

     <span data-ttu-id="d23c7-206">a.</span><span class="sxs-lookup"><span data-stu-id="d23c7-206">a.</span></span> <span data-ttu-id="d23c7-207">Mint **jelentkezzen be a felhasználó létrehozása**, jelölje be **a hely új felhasználó létrehozása, amikor bejelentkeznek a**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-207">As **User creation on Sign in**, select **Create a new user in your site when they sign in**.</span></span>

     <span data-ttu-id="d23c7-208">b.</span><span class="sxs-lookup"><span data-stu-id="d23c7-208">b.</span></span> <span data-ttu-id="d23c7-209">Mint **beállítások bejelentkezés**, jelölje be **használható SAML gomb a "Bejelentkezés" képernyőn**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-209">As **Sign in Settings**, select **Use SAML button on “Sign in” screen**.</span></span>

     <span data-ttu-id="d23c7-210">c.</span><span class="sxs-lookup"><span data-stu-id="d23c7-210">c.</span></span> <span data-ttu-id="d23c7-211">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-211">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d23c7-212">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d23c7-212">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d23c7-213">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d23c7-213">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d23c7-214">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d23c7-214">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d23c7-215">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d23c7-215">Creating an Azure AD test user</span></span>
<span data-ttu-id="d23c7-216">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d23c7-216">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d23c7-218">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d23c7-218">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d23c7-219">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-219">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d23c7-221">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-221">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d23c7-223">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23c7-223">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d23c7-225">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d23c7-225">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-igloo-software-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d23c7-227">a.</span><span class="sxs-lookup"><span data-stu-id="d23c7-227">a.</span></span> <span data-ttu-id="d23c7-228">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-228">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d23c7-229">b.</span><span class="sxs-lookup"><span data-stu-id="d23c7-229">b.</span></span> <span data-ttu-id="d23c7-230">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d23c7-230">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d23c7-231">c.</span><span class="sxs-lookup"><span data-stu-id="d23c7-231">c.</span></span> <span data-ttu-id="d23c7-232">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-232">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d23c7-233">d.</span><span class="sxs-lookup"><span data-stu-id="d23c7-233">d.</span></span> <span data-ttu-id="d23c7-234">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-234">Click **Create**.</span></span>
 
### <a name="creating-an-igloo-software-test-user"></a><span data-ttu-id="d23c7-235">Egy Igloo szoftver tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d23c7-235">Creating an Igloo Software test user</span></span>

<span data-ttu-id="d23c7-236">Nincs művelet elem a akkor tooconfigure felhasználók átadásához tooIgloo szoftver.</span><span class="sxs-lookup"><span data-stu-id="d23c7-236">There is no action item for you tooconfigure user provisioning tooIgloo Software.</span></span>  

<span data-ttu-id="d23c7-237">Amikor egy hozzárendelt felhasználó kísérli meg a szoftver hello hozzáférési panel Igloo szoftver használatával ellenőrzi, hogy létezik-e hello felhasználói tooIgloo toolog.</span><span class="sxs-lookup"><span data-stu-id="d23c7-237">When an assigned user tries toolog in tooIgloo Software using hello access panel, Igloo Software checks whether hello user exists.</span></span>  <span data-ttu-id="d23c7-238">Ha nincs felhasználói fiók elérhető még, automatikusan létrejön Igloo szoftvereket.</span><span class="sxs-lookup"><span data-stu-id="d23c7-238">If there is no user account available yet, it is automatically created by Igloo Software.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d23c7-239">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d23c7-239">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d23c7-240">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooIgloo szoftver megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d23c7-240">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooIgloo Software.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d23c7-242">**tooassign Britta Simon tooIgloo szoftver, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d23c7-242">**tooassign Britta Simon tooIgloo Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="d23c7-243">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-243">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d23c7-245">Hello alkalmazások listában válassza ki a **Igloo szoftver**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-245">In hello applications list, select **Igloo Software**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-igloo-software-tutorial/tutorial_igloosoftware_app.png) 

3. <span data-ttu-id="d23c7-247">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d23c7-247">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d23c7-249">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d23c7-249">Click **Add** button.</span></span> <span data-ttu-id="d23c7-250">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23c7-250">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d23c7-252">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d23c7-252">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d23c7-253">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23c7-253">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d23c7-254">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d23c7-254">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d23c7-255">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d23c7-255">Testing single sign-on</span></span>

<span data-ttu-id="d23c7-256">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d23c7-256">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d23c7-257">Ha a hozzáférési Panel hello hello Igloo szoftver csempe gombra kattint, automatikusan bejelentkezett tooyour Igloo alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d23c7-257">When you click hello Igloo Software tile in hello Access Panel, you should get automatically signed-on tooyour Igloo Software application.</span></span>
<span data-ttu-id="d23c7-258">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d23c7-258">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="d23c7-259">További források</span><span class="sxs-lookup"><span data-stu-id="d23c7-259">Additional resources</span></span>

* [<span data-ttu-id="d23c7-260">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d23c7-260">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d23c7-261">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d23c7-261">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-igloo-software-tutorial/tutorial_general_203.png

