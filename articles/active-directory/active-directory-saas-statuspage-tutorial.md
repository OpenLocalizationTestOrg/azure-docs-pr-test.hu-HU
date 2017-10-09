---
title: "Oktatóanyag: Azure Active Directoryval integrált StatusPage |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és StatusPage között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f6ee8bb3-df43-4c0d-bf84-89f18deac4b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.openlocfilehash: 7c6717017984241e9e459273ead4b5e062311120
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-statuspage"></a><span data-ttu-id="6b5e5-103">Oktatóanyag: Azure Active Directoryval integrált StatusPage</span><span class="sxs-lookup"><span data-stu-id="6b5e5-103">Tutorial: Azure Active Directory integration with StatusPage</span></span>

<span data-ttu-id="6b5e5-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate StatusPage az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="6b5e5-104">In this tutorial, you learn how toointegrate StatusPage with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="6b5e5-105">StatusPage integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-105">Integrating StatusPage with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="6b5e5-106">Megadhatja a hozzáférés tooStatusPage rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="6b5e5-106">You can control in Azure AD who has access tooStatusPage</span></span>
- <span data-ttu-id="6b5e5-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooStatusPage (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="6b5e5-107">You can enable your users tooautomatically get signed-on tooStatusPage (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="6b5e5-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="6b5e5-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="6b5e5-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="6b5e5-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b5e5-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6b5e5-110">Prerequisites</span></span>

<span data-ttu-id="6b5e5-111">az Azure AD integrálása StatusPage tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-111">tooconfigure Azure AD integration with StatusPage, you need hello following items:</span></span>

- <span data-ttu-id="6b5e5-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="6b5e5-112">An Azure AD subscription</span></span>
- <span data-ttu-id="6b5e5-113">Egy StatusPage egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="6b5e5-113">A StatusPage single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="6b5e5-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="6b5e5-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="6b5e5-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="6b5e5-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="6b5e5-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="6b5e5-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-118">Scenario description</span></span>
<span data-ttu-id="6b5e5-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="6b5e5-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="6b5e5-121">Hello gyűjteményből StatusPage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-121">Adding StatusPage from hello gallery</span></span>
2. <span data-ttu-id="6b5e5-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6b5e5-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-statuspage-from-hello-gallery"></a><span data-ttu-id="6b5e5-123">Hello gyűjteményből StatusPage hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-123">Adding StatusPage from hello gallery</span></span>
<span data-ttu-id="6b5e5-124">tooconfigure hello integrációja StatusPage az Azure AD-be, meg kell tooadd StatusPage hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-124">tooconfigure hello integration of StatusPage into Azure AD, you need tooadd StatusPage from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="6b5e5-125">**tooadd StatusPage hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-125">**tooadd StatusPage from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b5e5-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="6b5e5-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="6b5e5-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="6b5e5-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="6b5e5-133">Hello keresési mezőbe, írja be a **StatusPage**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-133">In hello search box, type **StatusPage**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_search.png)

5. <span data-ttu-id="6b5e5-135">A hello eredmények panelen válassza ki a **StatusPage**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-135">In hello results panel, select **StatusPage**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="6b5e5-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6b5e5-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="6b5e5-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján StatusPage.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-138">In this section, you configure and test Azure AD single sign-on with StatusPage based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="6b5e5-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó StatusPage tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in StatusPage is tooa user in Azure AD.</span></span> <span data-ttu-id="6b5e5-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello StatusPage közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-140">In other words, a link relationship between an Azure AD user and hello related user in StatusPage needs toobe established.</span></span>

<span data-ttu-id="6b5e5-141">StatusPage, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-141">In StatusPage, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="6b5e5-142">tooconfigure és az Azure AD az egyszeri bejelentkezés StatusPage-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-142">tooconfigure and test Azure AD single sign-on with StatusPage, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="6b5e5-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="6b5e5-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="6b5e5-145">**[StatusPage tesztfelhasználó létrehozása](#creating-a-statuspage-test-user)**  -toohave egy megfelelője a Britta Simon a StatusPage, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-145">**[Creating a StatusPage test user](#creating-a-statuspage-test-user)** - toohave a counterpart of Britta Simon in StatusPage that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="6b5e5-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="6b5e5-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="6b5e5-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="6b5e5-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az StatusPage alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your StatusPage application.</span></span>

<span data-ttu-id="6b5e5-150">**az Azure AD tooconfigure egyszeri bejelentkezést a StatusPage, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-150">**tooconfigure Azure AD single sign-on with StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b5e5-151">Az Azure portál, a hello hello **StatusPage** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-151">In hello Azure portal, on hello **StatusPage** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="6b5e5-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_samlbase.png)

3. <span data-ttu-id="6b5e5-155">A hello **StatusPage tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-155">On hello **StatusPage Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_url.png)

    <span data-ttu-id="6b5e5-157">a.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-157">a.</span></span> <span data-ttu-id="6b5e5-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/` |
    | `https://<subdomain>.statuspage.io/` |

    <span data-ttu-id="6b5e5-159">b.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-159">b.</span></span> <span data-ttu-id="6b5e5-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-160">In hello **Reply URL** textbox, type a URL using hello following pattern:</span></span> 
    | |
    |--|
    | `https://<subdomain>.statuspagestaging.com/sso/saml/consume` |
    | `https://<subdomain>.statuspage.io/sso/saml/consume` |

    > [!NOTE]
    > <span data-ttu-id="6b5e5-161">Hello StatusPage támogatási csoportjához, [ SupportTeam@statuspage.io ](mailto:SupportTeam@statuspage.io)toorequest metaadat szükséges tooconfigure egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-161">Contact hello StatusPage support team at [SupportTeam@statuspage.io](mailto:SupportTeam@statuspage.io)toorequest metadata necessary tooconfigure single sign-on.</span></span> 
    >
    ><span data-ttu-id="6b5e5-162">a.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-162">a.</span></span> <span data-ttu-id="6b5e5-163">Hello metaadatokból hello kibocsátó érték másolja és illessze be hello **azonosító** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-163">From hello metadata, copy hello Issuer value, and then paste it into hello **Identifier** textbox.</span></span>
    >
    ><span data-ttu-id="6b5e5-164">b.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-164">b.</span></span> <span data-ttu-id="6b5e5-165">Hello metaadatokból hello válasz URL-CÍMEN másolja és illessze be hello **válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-165">From hello metadata, copy hello Reply URL, and then paste it into hello **Reply URL** textbox.</span></span>

4. <span data-ttu-id="6b5e5-166">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-166">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_certificate.png) 

5. <span data-ttu-id="6b5e5-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-168">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="6b5e5-170">A hello **StatusPage konfigurációs** kattintson **konfigurálása StatusPage** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-170">On hello **StatusPage Configuration** section, click **Configure StatusPage** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="6b5e5-171">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-171">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_configure.png) 

7. <span data-ttu-id="6b5e5-173">Egy másik böngészőablakban bejelentkezéskor tooyour StatusPage vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-173">In another browser window, sign on tooyour StatusPage company site as an administrator.</span></span>

8. <span data-ttu-id="6b5e5-174">Hello eszköztárán kattintson **fiók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-174">In hello main toolbar, click **Manage Account**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png) 

10. <span data-ttu-id="6b5e5-176">Kattintson a hello **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-176">Click hello **Single Sign-on** tab.</span></span> 
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_07.png) 

11. <span data-ttu-id="6b5e5-178">Hello egyszeri bejelentkezés beállítása lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-178">On hello SSO Setup page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_08.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_09.png) 
 
    <span data-ttu-id="6b5e5-181">a.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-181">a.</span></span> <span data-ttu-id="6b5e5-182">A hello **SSO célként megadott URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-182">In hello **SSO Target URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="6b5e5-183">b.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-183">b.</span></span> <span data-ttu-id="6b5e5-184">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-184">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Certificate** textbox.</span></span> 

    <span data-ttu-id="6b5e5-185">c.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-185">c.</span></span> <span data-ttu-id="6b5e5-186">Kattintson a **konfiguráció mentése**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-186">Click **SAVE CONFIGURATION**.</span></span>

> [!TIP]
> <span data-ttu-id="6b5e5-187">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="6b5e5-187">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="6b5e5-188">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-188">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="6b5e5-189">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="6b5e5-189">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="6b5e5-190">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-190">Creating an Azure AD test user</span></span>
<span data-ttu-id="6b5e5-191">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-191">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="6b5e5-193">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-193">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b5e5-194">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-194">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="6b5e5-196">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-196">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="6b5e5-198">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-198">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="6b5e5-200">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6b5e5-200">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="6b5e5-202">a.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-202">a.</span></span> <span data-ttu-id="6b5e5-203">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-203">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="6b5e5-204">b.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-204">b.</span></span> <span data-ttu-id="6b5e5-205">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-205">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="6b5e5-206">c.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-206">c.</span></span> <span data-ttu-id="6b5e5-207">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-207">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="6b5e5-208">d.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-208">d.</span></span> <span data-ttu-id="6b5e5-209">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-209">Click **Create**.</span></span>
 
### <a name="creating-a-statuspage-test-user"></a><span data-ttu-id="6b5e5-210">StatusPage tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="6b5e5-210">Creating a StatusPage test user</span></span>

<span data-ttu-id="6b5e5-211">hello ebben a szakaszban célja toocreate StatusPage Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-211">hello objective of this section is toocreate a user called Britta Simon in StatusPage.</span></span>

<span data-ttu-id="6b5e5-212">StatusPage támogatja közvetlenül az időponthoz kötött kiosztást.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-212">StatusPage supports just-in-time provisioning.</span></span> <span data-ttu-id="6b5e5-213">Már engedélyezett legyen [az Azure AD konfigurálása egyszeri bejelentkezéshez](#configuring-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="6b5e5-213">You have already enabled it in [Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on).</span></span>

<span data-ttu-id="6b5e5-214">**toocreate Britta Simon meghívta StatusPage, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-214">**toocreate a user called Britta Simon in StatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b5e5-215">Bejelentkezés tooyour StatusPage vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-215">Sign-on tooyour StatusPage company site as an administrator.</span></span>

2. <span data-ttu-id="6b5e5-216">Hello hello felső menüben kattintson a **fiók kezelése**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-216">In hello menu on hello top, click **Manage Account**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_06.png)

3. <span data-ttu-id="6b5e5-218">Kattintson a hello **csapattagok** fülre.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-218">Click hello **Team Members** tab.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_10.png) 

4. <span data-ttu-id="6b5e5-220">Kattintson a **Hozzáadás CSAPATTAG**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-220">Click **ADD TEAM MEMBER**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_11.png) 

5. <span data-ttu-id="6b5e5-222">Típus hello **E-mail cím**, **Utónév**, és **Vezetéknév** érvényes felhasználó a kívánt tooprovision hello szövegmezők kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-222">Type hello **Email Address**, **First Name**, and **Sur Name** of a valid user you want tooprovision into hello related textboxes.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_12.png) 

6. <span data-ttu-id="6b5e5-224">Mint **szerepkör**, válassza a **ügyfél rendszergazda**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-224">As **Role**, choose **Client Administrator**.</span></span>

7. <span data-ttu-id="6b5e5-225">Kattintson a **fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-225">Click **CREATE ACCOUNT**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="6b5e5-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="6b5e5-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="6b5e5-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooStatusPage megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooStatusPage.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="6b5e5-229">**tooassign Britta Simon tooStatusPage, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="6b5e5-229">**tooassign Britta Simon tooStatusPage, perform hello following steps:**</span></span>

1. <span data-ttu-id="6b5e5-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="6b5e5-232">Hello alkalmazások listában válassza ki a **StatusPage**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-232">In hello applications list, select **StatusPage**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-statuspage-tutorial/tutorial_statuspage_app.png) 

3. <span data-ttu-id="6b5e5-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="6b5e5-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-236">Click **Add** button.</span></span> <span data-ttu-id="6b5e5-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="6b5e5-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="6b5e5-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="6b5e5-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="6b5e5-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="6b5e5-242">Testing single sign-on</span></span>

<span data-ttu-id="6b5e5-243">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-243">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="6b5e5-244">Ha a hozzáférési Panel hello hello StatusPage csempe gombra kattint, automatikusan bejelentkezett tooyour StatusPage alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="6b5e5-244">When you click hello StatusPage tile in hello Access Panel, you should get automatically signed-on tooyour StatusPage application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6b5e5-245">További források</span><span class="sxs-lookup"><span data-stu-id="6b5e5-245">Additional resources</span></span>

* [<span data-ttu-id="6b5e5-246">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6b5e5-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="6b5e5-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="6b5e5-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-statuspage-tutorial/tutorial_general_203.png

