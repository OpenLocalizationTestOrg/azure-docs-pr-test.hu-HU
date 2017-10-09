---
title: "Oktatóanyag: Azure Active Directoryval integrált Promapp |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Promapp között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 418d0601-6e7a-4997-a683-73fa30a2cfb5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/03/2017
ms.author: jeedes
ms.openlocfilehash: 02de7679b0c86d7aa8cacb41762f900dbf2ff231
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-promapp"></a><span data-ttu-id="7c38f-103">Oktatóanyag: Azure Active Directoryval integrált Promapp</span><span class="sxs-lookup"><span data-stu-id="7c38f-103">Tutorial: Azure Active Directory integration with Promapp</span></span>

<span data-ttu-id="7c38f-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Promapp az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7c38f-104">In this tutorial, you learn how toointegrate Promapp with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7c38f-105">Promapp integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7c38f-105">Integrating Promapp with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7c38f-106">Megadhatja a hozzáférés tooPromapp rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7c38f-106">You can control in Azure AD who has access tooPromapp</span></span>
- <span data-ttu-id="7c38f-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPromapp (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7c38f-107">You can enable your users tooautomatically get signed-on tooPromapp (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7c38f-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7c38f-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7c38f-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7c38f-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c38f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7c38f-110">Prerequisites</span></span>

<span data-ttu-id="7c38f-111">az Azure AD integrálása Promapp tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="7c38f-111">tooconfigure Azure AD integration with Promapp, you need hello following items:</span></span>

- <span data-ttu-id="7c38f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7c38f-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7c38f-113">Egy Promapp egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="7c38f-113">A Promapp single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7c38f-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7c38f-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7c38f-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7c38f-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7c38f-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7c38f-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="7c38f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c38f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7c38f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7c38f-118">Scenario description</span></span>
<span data-ttu-id="7c38f-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7c38f-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7c38f-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7c38f-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7c38f-121">Hello gyűjteményből Promapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7c38f-121">Adding Promapp from hello gallery</span></span>
2. <span data-ttu-id="7c38f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c38f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-promapp-from-hello-gallery"></a><span data-ttu-id="7c38f-123">Hello gyűjteményből Promapp hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7c38f-123">Adding Promapp from hello gallery</span></span>
<span data-ttu-id="7c38f-124">tooconfigure hello integrációja Promapp az Azure AD-be, meg kell tooadd Promapp hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7c38f-124">tooconfigure hello integration of Promapp into Azure AD, you need tooadd Promapp from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7c38f-125">**tooadd Promapp hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7c38f-125">**tooadd Promapp from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c38f-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7c38f-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7c38f-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7c38f-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="7c38f-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7c38f-133">Hello keresési mezőbe, írja be a **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-133">In hello search box, type **Promapp**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_search.png)

5. <span data-ttu-id="7c38f-135">A hello eredmények panelen válassza ki a **Promapp**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-135">In hello results panel, select **Promapp**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7c38f-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7c38f-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7c38f-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Promapp.</span><span class="sxs-lookup"><span data-stu-id="7c38f-138">In this section, you configure and test Azure AD single sign-on with Promapp based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7c38f-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Promapp tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7c38f-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Promapp is tooa user in Azure AD.</span></span> <span data-ttu-id="7c38f-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Promapp közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7c38f-140">In other words, a link relationship between an Azure AD user and hello related user in Promapp needs toobe established.</span></span>

<span data-ttu-id="7c38f-141">Promapp, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="7c38f-141">In Promapp, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="7c38f-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Promapp-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7c38f-142">tooconfigure and test Azure AD single sign-on with Promapp, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7c38f-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7c38f-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7c38f-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7c38f-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7c38f-145">**[Promapp tesztfelhasználó létrehozása](#creating-a-promapp-test-user)**  -toohave egy megfelelője a Britta Simon a Promapp, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="7c38f-145">**[Creating a Promapp test user](#creating-a-promapp-test-user)** - toohave a counterpart of Britta Simon in Promapp that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="7c38f-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7c38f-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7c38f-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7c38f-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7c38f-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7c38f-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7c38f-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Promapp alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="7c38f-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Promapp application.</span></span>

<span data-ttu-id="7c38f-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Promapp, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7c38f-150">**tooconfigure Azure AD single sign-on with Promapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c38f-151">Az Azure portál, a hello hello **Promapp** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-151">In hello Azure portal, on hello **Promapp** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7c38f-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7c38f-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_samlbase.png)

3. <span data-ttu-id="7c38f-155">A hello **Promapp tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7c38f-155">On hello **Promapp Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_url.png)

    <span data-ttu-id="7c38f-157">a.</span><span class="sxs-lookup"><span data-stu-id="7c38f-157">a.</span></span> <span data-ttu-id="7c38f-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span><span class="sxs-lookup"><span data-stu-id="7c38f-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME/saml/authenticate`</span></span>

    <span data-ttu-id="7c38f-159">b.</span><span class="sxs-lookup"><span data-stu-id="7c38f-159">b.</span></span> <span data-ttu-id="7c38f-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://DOMAINNAME.promapp.com/TENANTNAME`</span><span class="sxs-lookup"><span data-stu-id="7c38f-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://DOMAINNAME.promapp.com/TENANTNAME`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="7c38f-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="7c38f-161">These values are not real.</span></span> <span data-ttu-id="7c38f-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="7c38f-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="7c38f-163">Ügyfél [Promapp ügyfél-támogatási csoport](https://www.promapp.com/about-us/contact-us/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="7c38f-163">Contact [Promapp Client support team](https://www.promapp.com/about-us/contact-us/) tooget these values.</span></span>

4. <span data-ttu-id="7c38f-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7c38f-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_certificate.png) 

5. <span data-ttu-id="7c38f-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="7c38f-168">A hello **Promapp konfigurációs** kattintson **konfigurálása Promapp** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7c38f-168">On hello **Promapp Configuration** section, click **Configure Promapp** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7c38f-169">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7c38f-169">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_configure.png) 

7. <span data-ttu-id="7c38f-171">Bejelentkezés tooyour Promapp vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="7c38f-171">Sign-on tooyour Promapp company site as administrator.</span></span> 

8. <span data-ttu-id="7c38f-172">Hello hello felső menüben kattintson a **Admin**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-172">In hello menu on hello top, click **Admin**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][12]

9. <span data-ttu-id="7c38f-174">Kattintson a **Configure** (Konfigurálás) elemre.</span><span class="sxs-lookup"><span data-stu-id="7c38f-174">Click **Configure**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][13]

10. <span data-ttu-id="7c38f-176">A hello **biztonsági** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7c38f-176">On hello **Security** dialog, perform hello following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][14]
    
    <span data-ttu-id="7c38f-178">a.</span><span class="sxs-lookup"><span data-stu-id="7c38f-178">a.</span></span> <span data-ttu-id="7c38f-179">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amely akkor másolta, az Azure-portálon hello hello **SSO-bejelentkezési URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="7c38f-179">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal into hello **SSO-Login URL** textbox.</span></span>
    
    <span data-ttu-id="7c38f-180">b.</span><span class="sxs-lookup"><span data-stu-id="7c38f-180">b.</span></span> <span data-ttu-id="7c38f-181">Mint **SSO - egyszeri bejelentkezés mód**, jelölje be **nem kötelező**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-181">As **SSO - Single Sign-on Mode**, select **Optional**, and then click **Save**.</span></span>

    <span data-ttu-id="7c38f-182">c.</span><span class="sxs-lookup"><span data-stu-id="7c38f-182">c.</span></span> <span data-ttu-id="7c38f-183">Nyissa meg hello letöltött tanúsítvány a Jegyzettömbben másolási hello tanúsítványok tartalmát nélkül hello első sort (---BEGIN CERTIFICATE---) és hello utolsó sort (---vége tanúsítvány---), illessze be hello **SSO-x.509 tanúsítvány** Szövegmező, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-183">Open hello downloaded certificate in notepad, copy hello certificate content without hello first line (-----BEGIN CERTIFICATE-----) and hello last line (-----END CERTIFICATE-----), paste it into hello **SSO-x.509 Certificate** textbox, and then click **Save**.</span></span>
        
> [!TIP]
> <span data-ttu-id="7c38f-184">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7c38f-184">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7c38f-185">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7c38f-185">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7c38f-186">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7c38f-186">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7c38f-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c38f-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="7c38f-188">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7c38f-188">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7c38f-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7c38f-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c38f-191">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-191">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7c38f-193">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-193">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7c38f-195">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c38f-195">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7c38f-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7c38f-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-promapp-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7c38f-199">a.</span><span class="sxs-lookup"><span data-stu-id="7c38f-199">a.</span></span> <span data-ttu-id="7c38f-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="7c38f-201">b.</span><span class="sxs-lookup"><span data-stu-id="7c38f-201">b.</span></span> <span data-ttu-id="7c38f-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="7c38f-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="7c38f-203">c.</span><span class="sxs-lookup"><span data-stu-id="7c38f-203">c.</span></span> <span data-ttu-id="7c38f-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7c38f-205">d.</span><span class="sxs-lookup"><span data-stu-id="7c38f-205">d.</span></span> <span data-ttu-id="7c38f-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-206">Click **Create**.</span></span>
 
### <a name="creating-a-promapp-test-user"></a><span data-ttu-id="7c38f-207">Promapp tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7c38f-207">Creating a Promapp test user</span></span>

<span data-ttu-id="7c38f-208">hello Promapp alkalmazás támogatja a csak időponthoz kötött kiépítés.</span><span class="sxs-lookup"><span data-stu-id="7c38f-208">hello Promapp application supports Just-in-Time provisioning.</span></span> <span data-ttu-id="7c38f-209">Ez azt jelenti, egy felhasználói fiók automatikusan létrejön szükség kísérlet tooaccess hello használata az alkalmazások hello hozzáférési Panel alatt.</span><span class="sxs-lookup"><span data-stu-id="7c38f-209">This means, a user account is automatically created if necessary during an attempt tooaccess hello application using hello Access Panel.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7c38f-210">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7c38f-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7c38f-211">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPromapp megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7c38f-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPromapp.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7c38f-213">**tooassign Britta Simon tooPromapp, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="7c38f-213">**tooassign Britta Simon tooPromapp, perform hello following steps:**</span></span>

1. <span data-ttu-id="7c38f-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7c38f-216">Hello alkalmazások listában válassza ki a **Promapp**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-216">In hello applications list, select **Promapp**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-promapp-tutorial/tutorial_promapp_app.png) 

3. <span data-ttu-id="7c38f-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7c38f-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7c38f-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7c38f-220">Click **Add** button.</span></span> <span data-ttu-id="7c38f-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c38f-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7c38f-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7c38f-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7c38f-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c38f-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7c38f-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7c38f-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7c38f-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7c38f-226">Testing single sign-on</span></span>

<span data-ttu-id="7c38f-227">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="7c38f-227">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="7c38f-228">Ha a hozzáférési Panel hello hello Promapp csempe gombra kattint, automatikusan bejelentkezett tooyour Promapp alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7c38f-228">When you click hello Promapp tile in hello Access Panel, you should get automatically signed-on tooyour Promapp application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7c38f-229">További források</span><span class="sxs-lookup"><span data-stu-id="7c38f-229">Additional resources</span></span>

* [<span data-ttu-id="7c38f-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7c38f-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7c38f-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7c38f-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_04.png
[12]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_05.png
[13]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_06.png
[14]: ./media/active-directory-saas-promapp-tutorial/tutorial_promapp_07.png

[100]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-promapp-tutorial/tutorial_general_203.png

