---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező Egyensúlyozhatom Egyesült Államok (nem-UltiPro) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: b4a8f026-cb5f-41eb-9680-68eddc33565e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 874b5da277b9c68504c4af2ac87ed90d2bbd93b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-perception-united-states-non-ultipro"></a><span data-ttu-id="a4547-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező Egyensúlyozhatom Egyesült Államok (nem-UltiPro)</span><span class="sxs-lookup"><span data-stu-id="a4547-103">Tutorial: Azure Active Directory integration with Perception United States (Non-UltiPro)</span></span>

<span data-ttu-id="a4547-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Egyensúlyozhatom Egyesült Államok (nem-UltiPro) az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a4547-104">In this tutorial, you learn how toointegrate Perception United States (Non-UltiPro) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a4547-105">Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a4547-105">Integrating Perception United States (Non-UltiPro) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a4547-106">Az Azure AD hozzáférési tooPerception az Amerikai Egyesült Államok (nem-UltiPro) rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="a4547-106">You can control in Azure AD who has access tooPerception United States (Non-UltiPro).</span></span>
- <span data-ttu-id="a4547-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPerception az Amerikai Egyesült Államok (nem-UltiPro) (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="a4547-107">You can enable your users tooautomatically get signed-on tooPerception United States (Non-UltiPro) (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="a4547-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="a4547-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="a4547-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a4547-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a4547-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a4547-110">Prerequisites</span></span>

<span data-ttu-id="a4547-111">tooconfigure az Azure AD-integrációs a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro), a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="a4547-111">tooconfigure Azure AD integration with Perception United States (Non-UltiPro), you need hello following items:</span></span>

- <span data-ttu-id="a4547-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a4547-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a4547-113">Egy Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a4547-113">A Perception United States (Non-UltiPro) single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a4547-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a4547-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a4547-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a4547-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a4547-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a4547-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a4547-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a4547-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a4547-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a4547-118">Scenario description</span></span>
<span data-ttu-id="a4547-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a4547-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a4547-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a4547-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a4547-121">Hello gyűjteményből Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a4547-121">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
2. <span data-ttu-id="a4547-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a4547-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-perception-united-states-non-ultipro-from-hello-gallery"></a><span data-ttu-id="a4547-123">Hello gyűjteményből Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a4547-123">Adding Perception United States (Non-UltiPro) from hello gallery</span></span>
<span data-ttu-id="a4547-124">tooconfigure hello integrációs Egyensúlyozhatom Amerikai Egyesült államokbeli (nem-UltiPro) az Azure AD-be, szükség tooadd Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="a4547-124">tooconfigure hello integration of Perception United States (Non-UltiPro) into Azure AD, you need tooadd Perception United States (Non-UltiPro) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a4547-125">**tooadd Egyensúlyozhatom Egyesült Államok (nem-UltiPro) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a4547-125">**tooadd Perception United States (Non-UltiPro) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4547-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a4547-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="a4547-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a4547-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a4547-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a4547-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="a4547-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a4547-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="a4547-133">Hello keresési mezőbe, írja be a **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)**, jelölje be **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** eredmény panelen kattintson a **Hozzáadás** gomb tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a4547-133">In hello search box, type **Perception United States (Non-UltiPro)**, select **Perception United States (Non-UltiPro)** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Hello eredménylistában egyensúlyozhatom Egyesült Államok (nem-UltiPro)](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="a4547-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a4547-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="a4547-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Egyensúlyozhatom Egyesült Államok (nem-UltiPro) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="a4547-136">In this section, you configure and test Azure AD single sign-on with Perception United States (Non-UltiPro) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a4547-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a4547-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Perception United States (Non-UltiPro) is tooa user in Azure AD.</span></span> <span data-ttu-id="a4547-138">Ez azt jelenti hello kapcsolódó felhasználó a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a4547-138">In other words, a link relationship between an Azure AD user and hello related user in Perception United States (Non-UltiPro) needs toobe established.</span></span>

<span data-ttu-id="a4547-139">A Egyensúlyozhatom Egyesült Államok (nem-UltiPro), rendeljen hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a4547-139">In Perception United States (Non-UltiPro), assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a4547-140">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro), a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a4547-140">tooconfigure and test Azure AD single sign-on with Perception United States (Non-UltiPro), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a4547-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a4547-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a4547-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a4547-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a4547-143">**[Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tesztfelhasználó létrehozása](#create-a-perception-united-states-non-ultipro-test-user)**  -toohave egy megfelelője a Britta Simon a Egyensúlyozhatom Egyesült Államok (nem-UltiPro), amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a4547-143">**[Create a Perception United States (Non-UltiPro) test user](#create-a-perception-united-states-non-ultipro-test-user)** - toohave a counterpart of Britta Simon in Perception United States (Non-UltiPro) that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a4547-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a4547-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a4547-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a4547-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="a4547-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a4547-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="a4547-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a4547-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Perception United States (Non-UltiPro) application.</span></span>

<span data-ttu-id="a4547-148">**az Azure AD tooconfigure egyszeri bejelentkezést a Egyensúlyozhatom Egyesült Államok (nem-UltiPro), hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a4547-148">**tooconfigure Azure AD single sign-on with Perception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="a4547-149">Az Azure portál, a hello hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a4547-149">In hello Azure portal, on hello **Perception United States (Non-UltiPro)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="a4547-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a4547-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_samlbase.png)

3. <span data-ttu-id="a4547-153">A hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a4547-153">On hello **Perception United States (Non-UltiPro) Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_url.png)

    <span data-ttu-id="a4547-155">a.</span><span class="sxs-lookup"><span data-stu-id="a4547-155">a.</span></span> <span data-ttu-id="a4547-156">A hello **azonosító** szövegmezőhöz típus hello URL-címe:`https://perception.kanjoya.com/sp`</span><span class="sxs-lookup"><span data-stu-id="a4547-156">In hello **Identifier** textbox, type hello URL: `https://perception.kanjoya.com/sp`</span></span>

    <span data-ttu-id="a4547-157">b.</span><span class="sxs-lookup"><span data-stu-id="a4547-157">b.</span></span> <span data-ttu-id="a4547-158">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://perception.kanjoya.com/sso?idp=<entity_id>`</span><span class="sxs-lookup"><span data-stu-id="a4547-158">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://perception.kanjoya.com/sso?idp=<entity_id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a4547-159">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="a4547-159">hello value is not real.</span></span> <span data-ttu-id="a4547-160">Hello érték hello tényleges válasz URL-CÍMEN, hello oktatóanyag későbbi részében ismertetett frissíti.</span><span class="sxs-lookup"><span data-stu-id="a4547-160">You will update hello value with hello actual Reply URL, which is explained later in hello tutorial.</span></span>
 
4. <span data-ttu-id="a4547-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a4547-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_certificate.png) 

5. <span data-ttu-id="a4547-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4547-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="a4547-165">A hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) konfigurációs** kattintson **Egyensúlyozhatom Egyesült Államok konfigurálása (nem-UltiPro)** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a4547-165">On hello **Perception United States (Non-UltiPro) Configuration** section, click **Configure Perception United States (Non-UltiPro)** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a4547-166">Másolás hello **SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a4547-166">Copy hello **SAML Entity ID** from hello **Quick Reference section.**</span></span>

    <span data-ttu-id="a4547-167">a.</span><span class="sxs-lookup"><span data-stu-id="a4547-167">a.</span></span> <span data-ttu-id="a4547-168">Hello **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)** alkalmazáshoz szükséges hello **SAML Entitásazonosító** érték, amely másolta, toobe uri-kódolású.</span><span class="sxs-lookup"><span data-stu-id="a4547-168">hello **Perception United States (Non-UltiPro)** application requires hello **SAML Entity ID** value, which you have copied, toobe uri encoded.</span></span> <span data-ttu-id="a4547-169">tooget hello uri-kódolt értéket, használjon hello következő hivatkozásra kattintva:**http://www.url-encode-decode.com/**.</span><span class="sxs-lookup"><span data-stu-id="a4547-169">tooget hello uri encoded value, use hello following link:**http://www.url-encode-decode.com/**.</span></span>

    <span data-ttu-id="a4547-170">b.</span><span class="sxs-lookup"><span data-stu-id="a4547-170">b.</span></span> <span data-ttu-id="a4547-171">Első hello uri után kódolt érték összevonásához hello **válasz URL-CÍMEN** ahogyan az alábbi -</span><span class="sxs-lookup"><span data-stu-id="a4547-171">After getting hello uri encoded value combine it with hello **Reply URL** as mentioned below-</span></span>

    `https://perception.kanjoya.com/sso?idp=<URI encooded entity_id>`
    
    <span data-ttu-id="a4547-172">c.</span><span class="sxs-lookup"><span data-stu-id="a4547-172">c.</span></span> <span data-ttu-id="a4547-173">Beillesztés hello hello érték feletti **válasz URL-CÍMEN** textbox **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tartományhoz és URL-címek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a4547-173">Paste hello above value in hello **Reply URL** textbox in **Perception United States (Non-UltiPro) Domain and URLs** section.</span></span>

    ![Az Amerikai Egyesült Államok (nem UltiPro) konfigurációs érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_configure.png) 

7. <span data-ttu-id="a4547-175">Egy másik böngészőablakban bejelentkezéskor tooyour Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a4547-175">In another browser window, sign on tooyour Perception United States (Non-UltiPro) company site as an administrator.</span></span>

8. <span data-ttu-id="a4547-176">Hello eszköztárán kattintson **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="a4547-176">In hello main toolbar, click **Account Settings**.</span></span>

    ![Az Amerikai Egyesült Államok (nem-UltiPro) felhasználói érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_user.png)

9. <span data-ttu-id="a4547-178">A hello **Fiókbeállítások** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a4547-178">On hello **Account Settings** page, perform hello following steps:</span></span>

    ![Az Amerikai Egyesült Államok (nem-UltiPro) felhasználói érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_account.png)

    <span data-ttu-id="a4547-180">a.</span><span class="sxs-lookup"><span data-stu-id="a4547-180">a.</span></span> <span data-ttu-id="a4547-181">A hello **vállalatnév** szövegmezőhöz hello hello nevét **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="a4547-181">In hello **Company Name** textbox, type hello name of hello **Company**.</span></span>
    
    <span data-ttu-id="a4547-182">b.</span><span class="sxs-lookup"><span data-stu-id="a4547-182">b.</span></span> <span data-ttu-id="a4547-183">A hello **fióknév** szövegmezőhöz hello hello nevét **fiók**.</span><span class="sxs-lookup"><span data-stu-id="a4547-183">In hello **Account Name** textbox, type hello name of hello **Account**.</span></span>

    <span data-ttu-id="a4547-184">c.</span><span class="sxs-lookup"><span data-stu-id="a4547-184">c.</span></span> <span data-ttu-id="a4547-185">A **alapértelmezett válasz-tooEmail** szövegmezőben, érvényes típust hello **E-mail**.</span><span class="sxs-lookup"><span data-stu-id="a4547-185">In **Default Reply-tooEmail** text box, type hello valid **Email**.</span></span>

    <span data-ttu-id="a4547-186">d.</span><span class="sxs-lookup"><span data-stu-id="a4547-186">d.</span></span> <span data-ttu-id="a4547-187">Válassza ki **SSO identitásszolgáltató** , **SAML 2.0**.</span><span class="sxs-lookup"><span data-stu-id="a4547-187">Select **SSO Identity Provider** as **SAML 2.0**.</span></span>

10. <span data-ttu-id="a4547-188">A hello **SSO konfigurációs** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a4547-188">On hello **SSO Configuration** page, perform hello following steps:</span></span>

    ![Az Amerikai Egyesült Államok (nem UltiPro) SSOConfig érzete](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_ssoconfig.png)

    <span data-ttu-id="a4547-190">a.</span><span class="sxs-lookup"><span data-stu-id="a4547-190">a.</span></span> <span data-ttu-id="a4547-191">Válassza ki **SAML NameID típus** , **E-mail**.</span><span class="sxs-lookup"><span data-stu-id="a4547-191">Select **SAML NameID Type** as **EMAIL**.</span></span>

    <span data-ttu-id="a4547-192">b.</span><span class="sxs-lookup"><span data-stu-id="a4547-192">b.</span></span> <span data-ttu-id="a4547-193">A hello **SSO Konfigurációnévvel** szövegmezőhöz hello nevét a **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="a4547-193">In hello **SSO Configuration Name** textbox, type hello name of your **Configuration**.</span></span>
    
    <span data-ttu-id="a4547-194">c.</span><span class="sxs-lookup"><span data-stu-id="a4547-194">c.</span></span> <span data-ttu-id="a4547-195">A **identitás-szolgáltató neve** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a4547-195">In **Identity Provider Name** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="a4547-196">d.</span><span class="sxs-lookup"><span data-stu-id="a4547-196">d.</span></span> <span data-ttu-id="a4547-197">A **SAML tartomány szövegmező**, írja be például a hello tartományt  **@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="a4547-197">In **SAML Domain textbox**, enter hello domain like **@contoso.com**.</span></span>

    <span data-ttu-id="a4547-198">e.</span><span class="sxs-lookup"><span data-stu-id="a4547-198">e.</span></span> <span data-ttu-id="a4547-199">Kattintson a **töltse fel újra** tooupload hello **metaadatainak XML-kódja** fájlt.</span><span class="sxs-lookup"><span data-stu-id="a4547-199">Click on **Upload Again** tooupload hello **Metadata XML** file.</span></span>

    <span data-ttu-id="a4547-200">f.</span><span class="sxs-lookup"><span data-stu-id="a4547-200">f.</span></span> <span data-ttu-id="a4547-201">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="a4547-201">Click **Update**.</span></span>


> [!TIP]
> <span data-ttu-id="a4547-202">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a4547-202">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a4547-203">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a4547-203">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a4547-204">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a4547-204">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="a4547-205">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="a4547-205">Create an Azure AD test user</span></span>

<span data-ttu-id="a4547-206">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a4547-206">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="a4547-208">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a4547-208">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a4547-209">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4547-209">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="a4547-211">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a4547-211">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="a4547-213">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="a4547-213">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="a4547-215">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a4547-215">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-perceptionunitedstates-tutorial/create_aaduser_04.png)

    <span data-ttu-id="a4547-217">a.</span><span class="sxs-lookup"><span data-stu-id="a4547-217">a.</span></span> <span data-ttu-id="a4547-218">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a4547-218">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a4547-219">b.</span><span class="sxs-lookup"><span data-stu-id="a4547-219">b.</span></span> <span data-ttu-id="a4547-220">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="a4547-220">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="a4547-221">c.</span><span class="sxs-lookup"><span data-stu-id="a4547-221">c.</span></span> <span data-ttu-id="a4547-222">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="a4547-222">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="a4547-223">d.</span><span class="sxs-lookup"><span data-stu-id="a4547-223">d.</span></span> <span data-ttu-id="a4547-224">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a4547-224">Click **Create**.</span></span>
  
### <a name="create-a-perception-united-states-non-ultipro-test-user"></a><span data-ttu-id="a4547-225">Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a4547-225">Create a Perception United States (Non-UltiPro) test user</span></span>

<span data-ttu-id="a4547-226">Ebben a szakaszban egy Britta Simon a Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a4547-226">In this section, you create a user called Britta Simon in Perception United States (Non-UltiPro).</span></span> <span data-ttu-id="a4547-227">Együttműködve [Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) támogatási csoport](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello felhasználók hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) platform.</span><span class="sxs-lookup"><span data-stu-id="a4547-227">Work with [Perception United States (Non-UltiPro) support team](http://www.ultimatesoftware.com/Contact/ContactUs) tooadd hello users in hello Perception United States (Non-UltiPro) platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="a4547-228">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="a4547-228">Assign hello Azure AD test user</span></span>

<span data-ttu-id="a4547-229">Ebben a szakaszban Azure egyszeri bejelentkezés Britta Simon toouse hozzáférés tooPerception Egyesült Államok (nem-UltiPro) megadásával engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="a4547-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPerception United States (Non-UltiPro).</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="a4547-231">**tooassign Britta Simon tooPerception az Amerikai Egyesült Államok (nem-UltiPro), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a4547-231">**tooassign Britta Simon tooPerception United States (Non-UltiPro), perform hello following steps:**</span></span>

1. <span data-ttu-id="a4547-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a4547-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a4547-234">Hello alkalmazások listában válassza ki a **Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro)**.</span><span class="sxs-lookup"><span data-stu-id="a4547-234">In hello applications list, select **Perception United States (Non-UltiPro)**.</span></span>

    ![hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) hivatkozásra hello alkalmazások listája](./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_perceptionunitedstates_app.png)  

3. <span data-ttu-id="a4547-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a4547-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="a4547-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a4547-238">Click **Add** button.</span></span> <span data-ttu-id="a4547-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a4547-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="a4547-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a4547-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a4547-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a4547-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a4547-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a4547-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="a4547-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a4547-244">Test single sign-on</span></span>

<span data-ttu-id="a4547-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a4547-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a4547-246">Hello Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Egyensúlyozhatom az Amerikai Egyesült Államok (nem-UltiPro) alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="a4547-246">When you click hello Perception United States (Non-UltiPro) tile in hello Access Panel, you should get automatically signed-on tooyour Perception United States (Non-UltiPro) application.</span></span>
<span data-ttu-id="a4547-247">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a4547-247">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a4547-248">További források</span><span class="sxs-lookup"><span data-stu-id="a4547-248">Additional resources</span></span>

* [<span data-ttu-id="a4547-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a4547-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a4547-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a4547-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-perceptionunitedstates-tutorial/tutorial_general_203.png

