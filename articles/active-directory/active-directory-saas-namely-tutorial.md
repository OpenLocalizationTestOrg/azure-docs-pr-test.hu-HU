---
title: "Oktatóanyag: Azure Active Directoryval integrált nevezetesen |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory közötti, nevezetesen."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 9541d5c4-4c82-4b5b-b01a-6a3f75a2b7a1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: jeedes
ms.openlocfilehash: b0477ca6fa52a21abea7de458f8a99a01e8c25c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-namely"></a><span data-ttu-id="b0cba-103">Oktatóanyag: Azure Active Directoryval integrált nevezetesen</span><span class="sxs-lookup"><span data-stu-id="b0cba-103">Tutorial: Azure Active Directory integration with Namely</span></span>

<span data-ttu-id="b0cba-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate nevezetesen az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b0cba-104">In this tutorial, you learn how toointegrate Namely with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b0cba-105">Kulcstartó integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="b0cba-105">Integrating Namely with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="b0cba-106">Megadhatja a hozzáférés tooNamely rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b0cba-106">You can control in Azure AD who has access tooNamely</span></span>
- <span data-ttu-id="b0cba-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooNamely (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b0cba-107">You can enable your users tooautomatically get signed-on tooNamely (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b0cba-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b0cba-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="b0cba-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b0cba-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b0cba-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b0cba-110">Prerequisites</span></span>

<span data-ttu-id="b0cba-111">tooconfigure az Azure AD integrálása, akkor a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="b0cba-111">tooconfigure Azure AD integration with Namely, you need hello following items:</span></span>

- <span data-ttu-id="b0cba-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b0cba-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b0cba-113">Kulcstartó egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="b0cba-113">A Namely single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b0cba-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="b0cba-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b0cba-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="b0cba-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b0cba-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b0cba-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="b0cba-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0cba-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b0cba-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b0cba-118">Scenario description</span></span>
<span data-ttu-id="b0cba-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b0cba-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b0cba-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b0cba-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b0cba-121">Hello gyűjteményből nevezetesen hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b0cba-121">Adding Namely from hello gallery</span></span>
2. <span data-ttu-id="b0cba-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b0cba-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-namely-from-hello-gallery"></a><span data-ttu-id="b0cba-123">Hello gyűjteményből nevezetesen hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b0cba-123">Adding Namely from hello gallery</span></span>
<span data-ttu-id="b0cba-124">tooconfigure hello integrációja nevezetesen az Azure AD-be, meg kell tooadd, nevezetesen a hello gyűjteménye tooyour kezelt SaaS-alkalmazások listáját.</span><span class="sxs-lookup"><span data-stu-id="b0cba-124">tooconfigure hello integration of Namely into Azure AD, you need tooadd Namely from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="b0cba-125">**Nevezetesen galériából hello tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b0cba-125">**tooadd Namely from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0cba-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b0cba-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="b0cba-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b0cba-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="b0cba-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b0cba-133">Hello keresési mezőbe, írja be a **nevezetesen**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-133">In hello search box, type **Namely**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_search.png)

5. <span data-ttu-id="b0cba-135">A hello eredmények panelen válassza ki a **nevezetesen**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-135">In hello results panel, select **Namely**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/tutorial_namely_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b0cba-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b0cba-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b0cba-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést, nevezetesen "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="b0cba-138">In this section, you configure and test Azure AD single sign-on with Namely based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b0cba-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó a nevezetesen az tooa felhasználó, az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="b0cba-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Namely is tooa user in Azure AD.</span></span> <span data-ttu-id="b0cba-140">Egy Azure AD-felhasználó és a kapcsolódó felhasználó hello hivatkozás kapcsolatának Ez azt jelenti, nevezetesen létrehozott toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b0cba-140">In other words, a link relationship between an Azure AD user and hello related user in Namely needs toobe established.</span></span>

<span data-ttu-id="b0cba-141">A kulcstartó, rendelje az hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="b0cba-141">In Namely, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="b0cba-142">tooconfigure és tesztelési az Azure AD egyszeri bejelentkezést, Önnek kell toocomplete hello építőelemeket a következő:</span><span class="sxs-lookup"><span data-stu-id="b0cba-142">tooconfigure and test Azure AD single sign-on with Namely, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="b0cba-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b0cba-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="b0cba-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="b0cba-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b0cba-145">**[Létrehozása egy nevezetesen tesztfelhasználó](#creating-a-namely-test-user)**  -toohave Britta Simon egy partner, a nevezetesen, amely csatolt toohello felhasználói az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="b0cba-145">**[Creating a Namely test user](#creating-a-namely-test-user)** - toohave a counterpart of Britta Simon in Namely that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="b0cba-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b0cba-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b0cba-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="b0cba-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b0cba-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0cba-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b0cba-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez a nevezetesen alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b0cba-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Namely application.</span></span>

<span data-ttu-id="b0cba-150">**az Azure AD tooconfigure egyszeri bejelentkezést a nevezetesen, hajtsa végre a hello a következő lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b0cba-150">**tooconfigure Azure AD single sign-on with Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0cba-151">Az Azure portál, a hello hello **nevezetesen** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-151">In hello Azure portal, on hello **Namely** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b0cba-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="b0cba-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_samlbase.png)

3. <span data-ttu-id="b0cba-155">A hello **nevezetesen tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b0cba-155">On hello **Namely Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_url.png)

    <span data-ttu-id="b0cba-157">a.</span><span class="sxs-lookup"><span data-stu-id="b0cba-157">a.</span></span> <span data-ttu-id="b0cba-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.namely.com`</span><span class="sxs-lookup"><span data-stu-id="b0cba-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com`</span></span>

    <span data-ttu-id="b0cba-159">b.</span><span class="sxs-lookup"><span data-stu-id="b0cba-159">b.</span></span> <span data-ttu-id="b0cba-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.namely.com/saml/metadata`</span><span class="sxs-lookup"><span data-stu-id="b0cba-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<subdomain>.namely.com/saml/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b0cba-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="b0cba-161">These values are not real.</span></span> <span data-ttu-id="b0cba-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="b0cba-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="b0cba-163">Ügyfél [nevezetesen ügyfél-támogatási csoport](https://www.namely.com/contact/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b0cba-163">Contact [Namely Client support team](https://www.namely.com/contact/) tooget these values.</span></span> 
 
4. <span data-ttu-id="b0cba-164">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b0cba-164">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_certificate.png) 

5. <span data-ttu-id="b0cba-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="b0cba-168">A hello **nevezetesen konfigurációs** kattintson **konfigurálása nevezetesen** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b0cba-168">On hello **Namely Configuration** section, click **Configure Namely** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="b0cba-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="b0cba-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_configure.png) 

7. <span data-ttu-id="b0cba-171">Egy másik böngészőablakban bejelentkezéskor tooyour nevezetesen vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b0cba-171">In another browser window, sign on tooyour Namely company site as an administrator.</span></span>

8. <span data-ttu-id="b0cba-172">Hello hello felső eszköztárán kattintson **vállalati**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-172">In hello toolbar on hello top, click **Company**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_06.png) 

9. <span data-ttu-id="b0cba-174">Kattintson a hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="b0cba-174">Click hello **Settings** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_07.png) 

10. <span data-ttu-id="b0cba-176">Kattintson a **SAML**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-176">Click **SAML**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_08.png) 

11. <span data-ttu-id="b0cba-178">A hello **SAML beállítások** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b0cba-178">On hello **SAML Settings** page, perform hello following steps:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_09.png)
 
    <span data-ttu-id="b0cba-180">a.</span><span class="sxs-lookup"><span data-stu-id="b0cba-180">a.</span></span> <span data-ttu-id="b0cba-181">Kattintson a **SAML engedélyezése**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-181">Click **Enable SAML**.</span></span> 

    <span data-ttu-id="b0cba-182">b.</span><span class="sxs-lookup"><span data-stu-id="b0cba-182">b.</span></span> <span data-ttu-id="b0cba-183">A hello **identitási szolgáltató egyszeri bejelentkezési URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="b0cba-183">In hello **Identity provider SSO url** textbox,  paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
    
    <span data-ttu-id="b0cba-184">c.</span><span class="sxs-lookup"><span data-stu-id="b0cba-184">c.</span></span> <span data-ttu-id="b0cba-185">Nyissa meg a letöltött tanúsítvány a Jegyzettömbben, másolása hello tartalom, és illessze be hello **szolgáltató identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="b0cba-185">Open your downloaded certificate in Notepad, copy hello content, and then paste it into hello **Identity provider certificate** textbox.</span></span>
     
    <span data-ttu-id="b0cba-186">d.</span><span class="sxs-lookup"><span data-stu-id="b0cba-186">d.</span></span> <span data-ttu-id="b0cba-187">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-187">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="b0cba-188">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="b0cba-188">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="b0cba-189">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="b0cba-189">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="b0cba-190">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="b0cba-190">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b0cba-191">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0cba-191">Creating an Azure AD test user</span></span>
<span data-ttu-id="b0cba-192">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="b0cba-192">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b0cba-194">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="b0cba-194">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0cba-195">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-195">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b0cba-197">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-197">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b0cba-199">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0cba-199">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b0cba-201">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b0cba-201">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-namely-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b0cba-203">a.</span><span class="sxs-lookup"><span data-stu-id="b0cba-203">a.</span></span> <span data-ttu-id="b0cba-204">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-204">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b0cba-205">b.</span><span class="sxs-lookup"><span data-stu-id="b0cba-205">b.</span></span> <span data-ttu-id="b0cba-206">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0cba-206">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0cba-207">c.</span><span class="sxs-lookup"><span data-stu-id="b0cba-207">c.</span></span> <span data-ttu-id="b0cba-208">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-208">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="b0cba-209">d.</span><span class="sxs-lookup"><span data-stu-id="b0cba-209">d.</span></span> <span data-ttu-id="b0cba-210">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-210">Click **Create**.</span></span>
 
### <a name="creating-a-namely-test-user"></a><span data-ttu-id="b0cba-211">Létrehozása egy nevezetesen teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="b0cba-211">Creating a Namely test user</span></span>

<span data-ttu-id="b0cba-212">hello ebben a szakaszban célja toocreate nevezetesen Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b0cba-212">hello objective of this section is toocreate a user called Britta Simon in Namely.</span></span>

<span data-ttu-id="b0cba-213">**toocreate Britta Simon nevű, felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="b0cba-213">**toocreate a user called Britta Simon in Namely, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0cba-214">Bejelentkezés tooyour hely nevezetesen vállalati rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="b0cba-214">Sign-on tooyour Namely company site as an administrator.</span></span>

2. <span data-ttu-id="b0cba-215">Hello hello felső eszköztárán kattintson **személyek**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-215">In hello toolbar on hello top, click **People**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_10.png) 

3. <span data-ttu-id="b0cba-217">Kattintson a hello **Directory** fülre.</span><span class="sxs-lookup"><span data-stu-id="b0cba-217">Click hello **Directory** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_11.png) 

4. <span data-ttu-id="b0cba-219">Kattintson a **adja hozzá az új személy**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-219">Click **Add New Person**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_12.png)

5. <span data-ttu-id="b0cba-221">A hello **hozzáadása új személy** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="b0cba-221">On hello **Add New Person** dialog, perform hello following steps:</span></span>

    <span data-ttu-id="b0cba-222">a.</span><span class="sxs-lookup"><span data-stu-id="b0cba-222">a.</span></span> <span data-ttu-id="b0cba-223">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-223">In hello **First name** textbox, type **Britta**.</span></span>

    <span data-ttu-id="b0cba-224">b.</span><span class="sxs-lookup"><span data-stu-id="b0cba-224">b.</span></span> <span data-ttu-id="b0cba-225">A hello **Vezetéknév** szövegmezőhöz típus **Simon**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-225">In hello **Last name** textbox, type **Simon**.</span></span>

    <span data-ttu-id="b0cba-226">c.</span><span class="sxs-lookup"><span data-stu-id="b0cba-226">c.</span></span> <span data-ttu-id="b0cba-227">A hello **E-mail** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b0cba-227">In hello **Email** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b0cba-228">d.</span><span class="sxs-lookup"><span data-stu-id="b0cba-228">d.</span></span> <span data-ttu-id="b0cba-229">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-229">Click **Save**.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="b0cba-230">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b0cba-230">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="b0cba-231">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooNamely megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="b0cba-231">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooNamely.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b0cba-233">**tooassign Britta Simon tooNamely, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="b0cba-233">**tooassign Britta Simon tooNamely, perform hello following steps:**</span></span>

1. <span data-ttu-id="b0cba-234">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-234">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b0cba-236">Hello alkalmazások listában válassza ki a **nevezetesen**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-236">In hello applications list, select **Namely**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-namely-tutorial/tutorial_namely_app.png) 

3. <span data-ttu-id="b0cba-238">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b0cba-238">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b0cba-240">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b0cba-240">Click **Add** button.</span></span> <span data-ttu-id="b0cba-241">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0cba-241">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b0cba-243">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b0cba-243">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="b0cba-244">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0cba-244">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b0cba-245">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b0cba-245">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b0cba-246">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b0cba-246">Testing single sign-on</span></span>

<span data-ttu-id="b0cba-247">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="b0cba-247">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="b0cba-248">Hello kattintva hello hozzáférési Panel nevezetesen csempére, akkor kapja meg automatikusan bejelentkezett tooyour nevezetesen alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b0cba-248">When you click hello Namely tile in hello Access Panel, you should get automatically signed-on tooyour Namely application</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0cba-249">További források</span><span class="sxs-lookup"><span data-stu-id="b0cba-249">Additional resources</span></span>

* [<span data-ttu-id="b0cba-250">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="b0cba-250">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b0cba-251">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b0cba-251">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-namely-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-namely-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-namely-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-namely-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-namely-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-namely-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-namely-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-namely-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-namely-tutorial/tutorial_general_203.png

