---
title: "Oktatóanyag: Azure Active Directoryval integrált BambooHR |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és BambooHR között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f826b5d2-9c64-47df-bbbf-0adf9eb0fa71
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: jeedes
ms.openlocfilehash: f9083f846beb3a4bf4cebbf18b42aba2dfef2472
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bamboohr"></a><span data-ttu-id="20379-103">Oktatóanyag: Azure Active Directoryval integrált BambooHR</span><span class="sxs-lookup"><span data-stu-id="20379-103">Tutorial: Azure Active Directory integration with BambooHR</span></span>

<span data-ttu-id="20379-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate BambooHR az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="20379-104">In this tutorial, you learn how toointegrate BambooHR with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="20379-105">BambooHR integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="20379-105">Integrating BambooHR with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="20379-106">Megadhatja a hozzáférés tooBambooHR rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="20379-106">You can control in Azure AD who has access tooBambooHR</span></span>
- <span data-ttu-id="20379-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBambooHR (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="20379-107">You can enable your users tooautomatically get signed-on tooBambooHR (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="20379-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="20379-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="20379-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="20379-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="20379-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="20379-110">Prerequisites</span></span>

<span data-ttu-id="20379-111">az Azure AD integrálása BambooHR tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="20379-111">tooconfigure Azure AD integration with BambooHR, you need hello following items:</span></span>

- <span data-ttu-id="20379-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="20379-112">An Azure AD subscription</span></span>
- <span data-ttu-id="20379-113">Egy BambooHR egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="20379-113">A BambooHR single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="20379-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="20379-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="20379-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="20379-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="20379-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="20379-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="20379-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="20379-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="20379-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="20379-118">Scenario description</span></span>
<span data-ttu-id="20379-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="20379-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="20379-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="20379-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="20379-121">Hello gyűjteményből BambooHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20379-121">Adding BambooHR from hello gallery</span></span>
2. <span data-ttu-id="20379-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20379-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bamboohr-from-hello-gallery"></a><span data-ttu-id="20379-123">Hello gyűjteményből BambooHR hozzáadása</span><span class="sxs-lookup"><span data-stu-id="20379-123">Adding BambooHR from hello gallery</span></span>
<span data-ttu-id="20379-124">tooconfigure hello integrációja BambooHR az Azure AD-be, meg kell tooadd BambooHR hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="20379-124">tooconfigure hello integration of BambooHR into Azure AD, you need tooadd BambooHR from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="20379-125">**tooadd BambooHR hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="20379-125">**tooadd BambooHR from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="20379-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20379-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="20379-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="20379-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="20379-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20379-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="20379-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="20379-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="20379-133">Hello keresési mezőbe, írja be a **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="20379-133">In hello search box, type **BambooHR**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_search.png)

5. <span data-ttu-id="20379-135">A hello eredmények panelen válassza ki a **BambooHR**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-135">In hello results panel, select **BambooHR**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="20379-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="20379-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="20379-138">Ebben a szakaszban konfigurálása, és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon." nevű tesztfelhasználó alapján BambooHR</span><span class="sxs-lookup"><span data-stu-id="20379-138">In this section, you configure and test Azure AD single sign-on with BambooHR based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="20379-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó BambooHR tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="20379-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in BambooHR is tooa user in Azure AD.</span></span> <span data-ttu-id="20379-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello BambooHR közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="20379-140">In other words, a link relationship between an Azure AD user and hello related user in BambooHR needs toobe established.</span></span>

<span data-ttu-id="20379-141">BambooHR, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="20379-141">In BambooHR, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="20379-142">tooconfigure és az Azure AD az egyszeri bejelentkezés BambooHR-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="20379-142">tooconfigure and test Azure AD single sign-on with BambooHR, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="20379-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="20379-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="20379-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="20379-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="20379-145">**[BambooHR tesztfelhasználó létrehozása](#creating-a-bamboohr-test-user)**  -toohave egy megfelelője a Britta Simon a BambooHR, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="20379-145">**[Creating a BambooHR test user](#creating-a-bamboohr-test-user)** - toohave a counterpart of Britta Simon in BambooHR that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="20379-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="20379-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="20379-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="20379-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="20379-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="20379-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="20379-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az BambooHR alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="20379-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your BambooHR application.</span></span>

<span data-ttu-id="20379-150">**az Azure AD tooconfigure egyszeri bejelentkezést a BambooHR, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="20379-150">**tooconfigure Azure AD single sign-on with BambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="20379-151">Az Azure portál, a hello hello **BambooHR** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="20379-151">In hello Azure portal, on hello **BambooHR** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="20379-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="20379-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_samlbase.png)

3. <span data-ttu-id="20379-155">A hello **BambooHR tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="20379-155">On hello **BambooHR Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_url.png)

    <span data-ttu-id="20379-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.bamboohr.com`</span><span class="sxs-lookup"><span data-stu-id="20379-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.bamboohr.com`</span></span>
    
    > [!NOTE] 
    > <span data-ttu-id="20379-158">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="20379-158">This value is not real.</span></span> <span data-ttu-id="20379-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="20379-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="20379-160">Ügyfél [BambooHR ügyfél-támogatási csoport](https://www.bamboohr.com/contact.php) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="20379-160">Contact [BambooHR Client support team](https://www.bamboohr.com/contact.php) tooget this value.</span></span> 
 
4. <span data-ttu-id="20379-161">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="20379-161">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_certificate.png) 

5. <span data-ttu-id="20379-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="20379-165">A hello **BambooHR konfigurációs** kattintson **konfigurálása BambooHR** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="20379-165">On hello **BambooHR Configuration** section, click **Configure BambooHR** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="20379-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="20379-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_configure.png) 

6. <span data-ttu-id="20379-168">Egy másik webes böngészőablakban jelentkezzen be a BambooHR vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="20379-168">In a different web browser window, log into your BambooHR company site as an administrator.</span></span>

7. <span data-ttu-id="20379-169">Hello kezdőlap hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="20379-169">On hello homepage, perform hello following steps:</span></span>
   
    <span data-ttu-id="20379-170">![Egyszeri bejelentkezés](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="20379-170">![Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796691.png "Single Sign-On")</span></span>   

    <span data-ttu-id="20379-171">a.</span><span class="sxs-lookup"><span data-stu-id="20379-171">a.</span></span> <span data-ttu-id="20379-172">Kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="20379-172">Click **Apps**.</span></span>
   
    <span data-ttu-id="20379-173">b.</span><span class="sxs-lookup"><span data-stu-id="20379-173">b.</span></span> <span data-ttu-id="20379-174">A hello alkalmazások hello bal oldalon kattintson **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="20379-174">In hello apps menu on hello left, click **Single Sign-On**.</span></span>
   
    <span data-ttu-id="20379-175">c.</span><span class="sxs-lookup"><span data-stu-id="20379-175">c.</span></span> <span data-ttu-id="20379-176">Kattintson a **SAML-alapú egyszeri bejelentkezést**.</span><span class="sxs-lookup"><span data-stu-id="20379-176">Click **SAML Single Sign-On**.</span></span>

8. <span data-ttu-id="20379-177">A hello **SAML-alapú egyszeri bejelentkezést** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="20379-177">In hello **SAML Single Sign-On** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="20379-178">![SAML-alapú egyszeri bejelentkezést](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML-alapú egyszeri bejelentkezést.")</span><span class="sxs-lookup"><span data-stu-id="20379-178">![SAML Single Sign-On](./media/active-directory-saas-bamboo-hr-tutorial/ic796692.png "SAML Single Sign-On")</span></span>
   
    <span data-ttu-id="20379-179">a.</span><span class="sxs-lookup"><span data-stu-id="20379-179">a.</span></span> <span data-ttu-id="20379-180">Beillesztés hello **SAML-alapú egyszeri bejelentkezési URL-címe** hello érték **Egyszeri bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="20379-180">Paste hello **SAML Single Sign-On Service URL** value into hello **SSO Login Url** textbox.</span></span>
      
    <span data-ttu-id="20379-181">b.</span><span class="sxs-lookup"><span data-stu-id="20379-181">b.</span></span> <span data-ttu-id="20379-182">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött, és illessze be azt toohello **X.509 tanúsítvány** szövegmező</span><span class="sxs-lookup"><span data-stu-id="20379-182">Open base-64 encoded certificate downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **X.509 Certificate** textbox</span></span>
   
    <span data-ttu-id="20379-183">c.</span><span class="sxs-lookup"><span data-stu-id="20379-183">c.</span></span> <span data-ttu-id="20379-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="20379-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="20379-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="20379-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="20379-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="20379-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="20379-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="20379-188">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20379-188">Creating an Azure AD test user</span></span>
<span data-ttu-id="20379-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="20379-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="20379-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="20379-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="20379-192">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="20379-192">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="20379-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="20379-194">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="20379-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20379-196">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="20379-198">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="20379-198">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bamboo-hr-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="20379-200">a.</span><span class="sxs-lookup"><span data-stu-id="20379-200">a.</span></span> <span data-ttu-id="20379-201">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="20379-201">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="20379-202">b.</span><span class="sxs-lookup"><span data-stu-id="20379-202">b.</span></span> <span data-ttu-id="20379-203">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="20379-203">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="20379-204">c.</span><span class="sxs-lookup"><span data-stu-id="20379-204">c.</span></span> <span data-ttu-id="20379-205">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="20379-205">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="20379-206">d.</span><span class="sxs-lookup"><span data-stu-id="20379-206">d.</span></span> <span data-ttu-id="20379-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-207">Click **Create**.</span></span>
 
### <a name="creating-a-bamboohr-test-user"></a><span data-ttu-id="20379-208">BambooHR tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="20379-208">Creating a BambooHR test user</span></span>

<span data-ttu-id="20379-209">az Azure AD tooenable felhasználók toolog a tooBambooHR, akkor ki kell építenie BambooHR be.</span><span class="sxs-lookup"><span data-stu-id="20379-209">tooenable Azure AD users toolog in tooBambooHR, they must be provisioned into BambooHR.</span></span>  

<span data-ttu-id="20379-210">BambooHR hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="20379-210">In hello case of BambooHR, provisioning is a manual task.</span></span>

<span data-ttu-id="20379-211">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="20379-211">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="20379-212">Jelentkezzen be tooyour **BambooHR** hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="20379-212">Log in tooyour **BambooHR** site as administrator.</span></span>

2. <span data-ttu-id="20379-213">Hello hello felső eszköztárán kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="20379-213">In hello toolbar on hello top, click **Settings**.</span></span>
   
    <span data-ttu-id="20379-214">![Beállítás](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "beállítás")</span><span class="sxs-lookup"><span data-stu-id="20379-214">![Setting](./media/active-directory-saas-bamboo-hr-tutorial/ic796694.png "Setting")</span></span>

3. <span data-ttu-id="20379-215">Kattintson a **áttekintése**.</span><span class="sxs-lookup"><span data-stu-id="20379-215">Click **Overview**.</span></span>

4. <span data-ttu-id="20379-216">Hello bal oldali navigációs panelen, lépjen túl**biztonsági \> felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="20379-216">In hello left navigation pane, go too**Security \> Users**.</span></span>

5. <span data-ttu-id="20379-217">Írja be a hello felhasználónevet, jelszót és egy érvényes AAD-fiókba, tooprovision hello a kívánt e-mail címe kapcsolódó szövegmezőből.</span><span class="sxs-lookup"><span data-stu-id="20379-217">Type hello user name, password, and email address of a valid AAD account you want tooprovision into hello related textboxes.</span></span>

6. <span data-ttu-id="20379-218">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-218">Click **Save**.</span></span>
        
>[!NOTE]
><span data-ttu-id="20379-219">Bármely más BambooHR felhasználói fiók létrehozása eszközök vagy BambooHR tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="20379-219">You can use any other BambooHR user account creation tools or APIs provided by BambooHR tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="20379-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="20379-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="20379-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooBambooHR megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="20379-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooBambooHR.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="20379-223">**tooassign Britta Simon tooBambooHR, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="20379-223">**tooassign Britta Simon tooBambooHR, perform hello following steps:**</span></span>

1. <span data-ttu-id="20379-224">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="20379-224">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="20379-226">Hello alkalmazások listában válassza ki a **BambooHR**.</span><span class="sxs-lookup"><span data-stu-id="20379-226">In hello applications list, select **BambooHR**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bamboo-hr-tutorial/tutorial_bamboohr_app.png) 

3. <span data-ttu-id="20379-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="20379-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="20379-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="20379-230">Click **Add** button.</span></span> <span data-ttu-id="20379-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20379-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="20379-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="20379-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="20379-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20379-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="20379-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="20379-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="20379-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="20379-236">Testing single sign-on</span></span>

<span data-ttu-id="20379-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="20379-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="20379-238">Ha a hozzáférési Panel hello hello BambooHR csempe gombra kattint, automatikusan bejelentkezett tooyour BambooHR alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="20379-238">When you click hello BambooHR tile in hello Access Panel, you should get automatically signed-on tooyour BambooHR application.</span></span>
<span data-ttu-id="20379-239">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="20379-239">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="20379-240">További források</span><span class="sxs-lookup"><span data-stu-id="20379-240">Additional resources</span></span>

* [<span data-ttu-id="20379-241">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="20379-241">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="20379-242">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="20379-242">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bamboo-hr-tutorial/tutorial_general_203.png

