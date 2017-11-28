---
title: "Oktatóanyag: Azure Active Directoryval integrált RunMyProcess |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és RunMyProcess között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: jeedes
ms.openlocfilehash: f02acda015aeb8d131d8e3ef88bf50c4e8e94750
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-runmyprocess"></a><span data-ttu-id="83903-103">Oktatóanyag: Azure Active Directoryval integrált RunMyProcess</span><span class="sxs-lookup"><span data-stu-id="83903-103">Tutorial: Azure Active Directory integration with RunMyProcess</span></span>

<span data-ttu-id="83903-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate RunMyProcess az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="83903-104">In this tutorial, you learn how toointegrate RunMyProcess with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="83903-105">RunMyProcess integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="83903-105">Integrating RunMyProcess with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="83903-106">Megadhatja a hozzáférés tooRunMyProcess rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="83903-106">You can control in Azure AD who has access tooRunMyProcess</span></span>
- <span data-ttu-id="83903-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRunMyProcess (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="83903-107">You can enable your users tooautomatically get signed-on tooRunMyProcess (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="83903-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="83903-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="83903-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="83903-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="83903-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="83903-110">Prerequisites</span></span>

<span data-ttu-id="83903-111">az Azure AD integrálása RunMyProcess tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="83903-111">tooconfigure Azure AD integration with RunMyProcess, you need hello following items:</span></span>

- <span data-ttu-id="83903-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="83903-112">An Azure AD subscription</span></span>
- <span data-ttu-id="83903-113">Egy RunMyProcess egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="83903-113">A RunMyProcess single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="83903-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="83903-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="83903-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="83903-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="83903-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="83903-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="83903-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat:[próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="83903-117">If you don't have an Azure AD trial environment, you can get a one-month trial here:[Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="83903-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="83903-118">Scenario description</span></span>
<span data-ttu-id="83903-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="83903-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="83903-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="83903-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="83903-121">Hello gyűjteményből RunMyProcess hozzáadása</span><span class="sxs-lookup"><span data-stu-id="83903-121">Adding RunMyProcess from hello gallery</span></span>
2. <span data-ttu-id="83903-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="83903-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-runmyprocess-from-hello-gallery"></a><span data-ttu-id="83903-123">Hello gyűjteményből RunMyProcess hozzáadása</span><span class="sxs-lookup"><span data-stu-id="83903-123">Adding RunMyProcess from hello gallery</span></span>
<span data-ttu-id="83903-124">tooconfigure hello integrációja RunMyProcess az Azure AD-be, meg kell tooadd RunMyProcess hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="83903-124">tooconfigure hello integration of RunMyProcess into Azure AD, you need tooadd RunMyProcess from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="83903-125">**tooadd RunMyProcess hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="83903-125">**tooadd RunMyProcess from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="83903-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="83903-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="83903-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="83903-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="83903-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="83903-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="83903-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="83903-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="83903-133">Hello keresési mezőbe, írja be a **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="83903-133">In hello search box, type **RunMyProcess**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_search.png)

5. <span data-ttu-id="83903-135">A hello eredmények panelen válassza ki a **RunMyProcess**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="83903-135">In hello results panel, select **RunMyProcess**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="83903-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="83903-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="83903-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján RunMyProcess.</span><span class="sxs-lookup"><span data-stu-id="83903-138">In this section, you configure and test Azure AD single sign-on with RunMyProcess based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="83903-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó RunMyProcess tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="83903-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in RunMyProcess is tooa user in Azure AD.</span></span> <span data-ttu-id="83903-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello RunMyProcess közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="83903-140">In other words, a link relationship between an Azure AD user and hello related user in RunMyProcess needs toobe established.</span></span>

<span data-ttu-id="83903-141">RunMyProcess, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="83903-141">In RunMyProcess, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="83903-142">tooconfigure és az Azure AD az egyszeri bejelentkezés RunMyProcess-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="83903-142">tooconfigure and test Azure AD single sign-on with RunMyProcess, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="83903-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="83903-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="83903-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="83903-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="83903-145">**[RunMyProcess tesztfelhasználó létrehozása](#creating-a-runmyprocess-test-user)**  -toohave egy megfelelője a Britta Simon a RunMyProcess, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="83903-145">**[Creating a RunMyProcess test user](#creating-a-runmyprocess-test-user)** - toohave a counterpart of Britta Simon in RunMyProcess that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="83903-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="83903-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="83903-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="83903-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="83903-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="83903-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="83903-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az RunMyProcess alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="83903-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your RunMyProcess application.</span></span>

<span data-ttu-id="83903-150">**az Azure AD tooconfigure egyszeri bejelentkezést a RunMyProcess, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="83903-150">**tooconfigure Azure AD single sign-on with RunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="83903-151">Az Azure portál, a hello hello **RunMyProcess** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="83903-151">In hello Azure portal, on hello **RunMyProcess** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="83903-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="83903-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_samlbase.png)

3. <span data-ttu-id="83903-155">A hello **RunMyProcess tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="83903-155">On hello **RunMyProcess Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_url.png)

    <span data-ttu-id="83903-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://live.runmyprocess.com/live/<tenant id>`</span><span class="sxs-lookup"><span data-stu-id="83903-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://live.runmyprocess.com/live/<tenant id>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="83903-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="83903-158">hello value is not real.</span></span> <span data-ttu-id="83903-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="83903-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="83903-160">Ügyfél [RunMyProcess ügyfél-támogatási csoport](mailto:support@runmyprocess.com) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="83903-160">Contact [RunMyProcess Client support team](mailto:support@runmyprocess.com) tooget hello value.</span></span> 

4. <span data-ttu-id="83903-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="83903-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_certificate.png) 

5. <span data-ttu-id="83903-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="83903-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="83903-165">A hello **RunMyProcess konfigurációs** kattintson **konfigurálása RunMyProcess** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="83903-165">On hello **RunMyProcess Configuration** section, click **Configure RunMyProcess** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="83903-166">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="83903-166">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_configure.png) 

7. <span data-ttu-id="83903-168">Egy másik webes böngészőablakban, bejelentkezés tooyour RunMyProcess Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="83903-168">In a different web browser window, sign-on tooyour RunMyProcess tenant as an administrator.</span></span>

8. <span data-ttu-id="83903-169">A bal oldali navigációs panelen kattintson a **fiók** válassza **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="83903-169">In left navigation panel, click **Account** and select **Configuration**.</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_001.png)

9. <span data-ttu-id="83903-171">Nyissa meg túl**hitelesítési módszer** szakaszt, és hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="83903-171">Go too**Authentication method** section and perform below steps:</span></span>
   
    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    <span data-ttu-id="83903-173">a.</span><span class="sxs-lookup"><span data-stu-id="83903-173">a.</span></span> <span data-ttu-id="83903-174">Mint **metódus**, jelölje be **SSO Samlv2**.</span><span class="sxs-lookup"><span data-stu-id="83903-174">As **Method**, select **SSO with Samlv2**.</span></span> 

    <span data-ttu-id="83903-175">b.</span><span class="sxs-lookup"><span data-stu-id="83903-175">b.</span></span> <span data-ttu-id="83903-176">A hello **SSO átirányítási** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="83903-176">In hello **SSO redirect** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="83903-177">c.</span><span class="sxs-lookup"><span data-stu-id="83903-177">c.</span></span> <span data-ttu-id="83903-178">A hello **kijelentkezési átirányítási** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="83903-178">In hello **Logout redirect** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="83903-179">d.</span><span class="sxs-lookup"><span data-stu-id="83903-179">d.</span></span> <span data-ttu-id="83903-180">A hello **név Azonosítóformátumának** szövegmezőhöz hello értéke **azonosító formátuma** , **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: emailAddress**.</span><span class="sxs-lookup"><span data-stu-id="83903-180">In hello **Name Id Format** textbox, type hello value of **Name Identifier Format** as **urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**.</span></span>

    <span data-ttu-id="83903-181">e.</span><span class="sxs-lookup"><span data-stu-id="83903-181">e.</span></span> <span data-ttu-id="83903-182">Hello a letöltött tanúsítvány fájl tartalma hello másolja és illessze be hello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="83903-182">Copy hello content of hello downloaded certificate file and then paste it into hello **Certificate** textbox.</span></span> 
 
    <span data-ttu-id="83903-183">f.</span><span class="sxs-lookup"><span data-stu-id="83903-183">f.</span></span> <span data-ttu-id="83903-184">Kattintson a **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="83903-184">Click **Save** icon.</span></span>

> [!TIP]
> <span data-ttu-id="83903-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="83903-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="83903-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="83903-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="83903-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="83903-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="83903-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="83903-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="83903-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="83903-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="83903-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="83903-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="83903-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="83903-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="83903-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="83903-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="83903-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="83903-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="83903-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="83903-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-runmyprocess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="83903-200">a.</span><span class="sxs-lookup"><span data-stu-id="83903-200">a.</span></span> <span data-ttu-id="83903-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="83903-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="83903-202">b.</span><span class="sxs-lookup"><span data-stu-id="83903-202">b.</span></span> <span data-ttu-id="83903-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="83903-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="83903-204">c.</span><span class="sxs-lookup"><span data-stu-id="83903-204">c.</span></span> <span data-ttu-id="83903-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="83903-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="83903-206">d.</span><span class="sxs-lookup"><span data-stu-id="83903-206">d.</span></span> <span data-ttu-id="83903-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="83903-207">Click **Create**.</span></span>
 
### <a name="creating-a-runmyprocess-test-user"></a><span data-ttu-id="83903-208">RunMyProcess tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="83903-208">Creating a RunMyProcess test user</span></span>

<span data-ttu-id="83903-209">A sorrend tooenable az Azure AD felhasználók toolog a tooRunMyProcess azok ki kell építenie RunMyProcess be.</span><span class="sxs-lookup"><span data-stu-id="83903-209">In order tooenable Azure AD users toolog in tooRunMyProcess, they must be provisioned into RunMyProcess.</span></span> <span data-ttu-id="83903-210">RunMyProcess hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="83903-210">In hello case of RunMyProcess, provisioning is a manual task.</span></span>

<span data-ttu-id="83903-211">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="83903-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="83903-212">Jelentkezzen be tooyour RunMyProcess vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="83903-212">Log in tooyour RunMyProcess company site as an administrator.</span></span>

2. <span data-ttu-id="83903-213">Kattintson a **fiók** válassza **felhasználók** a bal oldali navigációs panelen, majd kattintson a **új felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="83903-213">Click **Account** and select **Users** in left navigation panel, then click **New User**.</span></span>
   
    <span data-ttu-id="83903-214">![Új felhasználó](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "új felhasználó")</span><span class="sxs-lookup"><span data-stu-id="83903-214">![New User](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_003.png "New User")</span></span>

3. <span data-ttu-id="83903-215">A hello **felhasználói beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="83903-215">In hello **User Settings** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="83903-216">![Profil](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "profil")</span><span class="sxs-lookup"><span data-stu-id="83903-216">![Profile](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profile")</span></span> 
  
    <span data-ttu-id="83903-217">a.</span><span class="sxs-lookup"><span data-stu-id="83903-217">a.</span></span> <span data-ttu-id="83903-218">Típus hello **neve** és **E-mail** egy érvényes Azure AD-fiókot a kívánt tooprovision hello kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="83903-218">Type hello **Name** and **E-mail** of a valid Azure AD account you want tooprovision into hello related textboxes.</span></span> 

    <span data-ttu-id="83903-219">b.</span><span class="sxs-lookup"><span data-stu-id="83903-219">b.</span></span> <span data-ttu-id="83903-220">Válasszon egy **IDE nyelvi**, **nyelvi**, és **profil**.</span><span class="sxs-lookup"><span data-stu-id="83903-220">Select an **IDE language**, **Language**, and **Profile**.</span></span> 

    <span data-ttu-id="83903-221">c.</span><span class="sxs-lookup"><span data-stu-id="83903-221">c.</span></span> <span data-ttu-id="83903-222">Válassza ki **fiók létrehozása e-mail toome küldése**.</span><span class="sxs-lookup"><span data-stu-id="83903-222">Select **Send account creation e-mail toome**.</span></span> 

    <span data-ttu-id="83903-223">d.</span><span class="sxs-lookup"><span data-stu-id="83903-223">d.</span></span> <span data-ttu-id="83903-224">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="83903-224">Click **Save**.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="83903-225">Bármely más RunMyProcess felhasználói fiók létrehozása eszközök vagy RunMyProcess tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="83903-225">You can use any other RunMyProcess user account creation tools or APIs provided by RunMyProcess tooprovision Azure Active Directory user accounts.</span></span> 
    > 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="83903-226">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="83903-226">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="83903-227">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRunMyProcess megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="83903-227">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRunMyProcess.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="83903-229">**tooassign Britta Simon tooRunMyProcess, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="83903-229">**tooassign Britta Simon tooRunMyProcess, perform hello following steps:**</span></span>

1. <span data-ttu-id="83903-230">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="83903-230">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="83903-232">Hello alkalmazások listában válassza ki a **RunMyProcess**.</span><span class="sxs-lookup"><span data-stu-id="83903-232">In hello applications list, select **RunMyProcess**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-runmyprocess-tutorial/tutorial_runmyprocess_app.png) 

3. <span data-ttu-id="83903-234">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="83903-234">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="83903-236">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="83903-236">Click **Add** button.</span></span> <span data-ttu-id="83903-237">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="83903-237">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="83903-239">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="83903-239">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="83903-240">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="83903-240">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="83903-241">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="83903-241">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="83903-242">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="83903-242">Testing single sign-on</span></span>

<span data-ttu-id="83903-243">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="83903-243">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="83903-244">Hello RunMyProcess hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour RunMyProcess alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="83903-244">When you click hello RunMyProcess tile in hello Access Panel, you should get automatically signed-on tooyour RunMyProcess application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="83903-245">További források</span><span class="sxs-lookup"><span data-stu-id="83903-245">Additional resources</span></span>

* [<span data-ttu-id="83903-246">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="83903-246">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="83903-247">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="83903-247">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-runmyprocess-tutorial/tutorial_general_203.png

