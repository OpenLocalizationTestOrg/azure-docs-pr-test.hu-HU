---
title: "Oktatóanyag: Azure Active Directoryval integrált UNIFI |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és UNIFI között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: e1f49ee4-d2d4-4a82-9baf-0587ca1f20f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: af7cc1167b5c0cff2a1f4cdaa8a2b93f5a718f81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-unifi"></a><span data-ttu-id="a6bcb-103">Oktatóanyag: Azure Active Directoryval integrált UNIFI</span><span class="sxs-lookup"><span data-stu-id="a6bcb-103">Tutorial: Azure Active Directory integration with UNIFI</span></span>

<span data-ttu-id="a6bcb-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate UNIFI az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a6bcb-104">In this tutorial, you learn how toointegrate UNIFI with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a6bcb-105">UNIFI integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-105">Integrating UNIFI with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a6bcb-106">Megadhatja a hozzáférés tooUNIFI rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a6bcb-106">You can control in Azure AD who has access tooUNIFI</span></span>
- <span data-ttu-id="a6bcb-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooUNIFI (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a6bcb-107">You can enable your users tooautomatically get signed-on tooUNIFI (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a6bcb-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a6bcb-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a6bcb-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a6bcb-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a6bcb-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a6bcb-110">Prerequisites</span></span>

<span data-ttu-id="a6bcb-111">az Azure AD integrálása UNIFI tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-111">tooconfigure Azure AD integration with UNIFI, you need hello following items:</span></span>

- <span data-ttu-id="a6bcb-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a6bcb-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a6bcb-113">Egy UNIFI egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a6bcb-113">A UNIFI single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a6bcb-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a6bcb-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a6bcb-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a6bcb-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a6bcb-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a6bcb-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-118">Scenario description</span></span>
<span data-ttu-id="a6bcb-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a6bcb-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a6bcb-121">Hello gyűjteményből UNIFI hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-121">Adding UNIFI from hello gallery</span></span>
2. <span data-ttu-id="a6bcb-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a6bcb-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-unifi-from-hello-gallery"></a><span data-ttu-id="a6bcb-123">Hello gyűjteményből UNIFI hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-123">Adding UNIFI from hello gallery</span></span>
<span data-ttu-id="a6bcb-124">tooconfigure hello integrációja UNIFI az Azure AD-be, meg kell tooadd UNIFI hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-124">tooconfigure hello integration of UNIFI into Azure AD, you need tooadd UNIFI from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a6bcb-125">**tooadd UNIFI hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a6bcb-125">**tooadd UNIFI from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6bcb-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a6bcb-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a6bcb-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a6bcb-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a6bcb-133">Hello keresési mezőbe, írja be a **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-133">In hello search box, type **UNIFI**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_search.png)

5. <span data-ttu-id="a6bcb-135">A hello eredmények panelen válassza ki a **UNIFI**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-135">In hello results panel, select **UNIFI**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a6bcb-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a6bcb-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a6bcb-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján UNIFI.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-138">In this section, you configure and test Azure AD single sign-on with UNIFI based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a6bcb-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó UNIFI tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in UNIFI is tooa user in Azure AD.</span></span> <span data-ttu-id="a6bcb-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello UNIFI közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-140">In other words, a link relationship between an Azure AD user and hello related user in UNIFI needs toobe established.</span></span>

<span data-ttu-id="a6bcb-141">UNIFI, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-141">In UNIFI, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a6bcb-142">tooconfigure és az Azure AD az egyszeri bejelentkezés UNIFI-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-142">tooconfigure and test Azure AD single sign-on with UNIFI, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a6bcb-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a6bcb-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a6bcb-145">**[Tesztfelhasználó létrehozása egy UNIFI](#creating-a-unifi-test-user)**  -toohave egy megfelelője a Britta Simon a UNIFI, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-145">**[Creating a UNIFI test user](#creating-a-unifi-test-user)** - toohave a counterpart of Britta Simon in UNIFI that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a6bcb-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a6bcb-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a6bcb-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a6bcb-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az UNIFI alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your UNIFI application.</span></span>

<span data-ttu-id="a6bcb-150">**az Azure AD tooconfigure egyszeri bejelentkezést a UNIFI, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a6bcb-150">**tooconfigure Azure AD single sign-on with UNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6bcb-151">Az Azure portál, a hello hello **UNIFI** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-151">In hello Azure portal, on hello **UNIFI** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a6bcb-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_samlbase.png)

3. <span data-ttu-id="a6bcb-155">A hello **UNIFI tartomány és az URL-címek** szakaszban, ha tooconfigure hello alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-155">On hello **UNIFI Domain and URLs** section, If you wish tooconfigure hello application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url1.png)

    <span data-ttu-id="a6bcb-157">A hello **azonosító** szövegmezőhöz Típusérték hello:`INVIEWlabs`</span><span class="sxs-lookup"><span data-stu-id="a6bcb-157">In hello **Identifier** textbox, type hello value: `INVIEWlabs`</span></span> 

4. <span data-ttu-id="a6bcb-158">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha tooconfigure hello alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-158">Check **Show advanced URL settings**, If you wish tooconfigure hello application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_url2.png)

    <span data-ttu-id="a6bcb-160">A hello **bejelentkezési URL-cím** szövegmezőhöz típus hello URL-címe:`https://app.discoverunifi.com/login`</span><span class="sxs-lookup"><span data-stu-id="a6bcb-160">In hello **Sign-on URL** textbox, type hello URL: `https://app.discoverunifi.com/login`</span></span>

5. <span data-ttu-id="a6bcb-161">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-161">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_certificate.png) 

6. <span data-ttu-id="a6bcb-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_general_400.png)
    
7. <span data-ttu-id="a6bcb-165">A hello **UNIFI konfigurációs** kattintson **konfigurálása UNIFI** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-165">On hello **UNIFI Configuration** section, click **Configure UNIFI** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="a6bcb-166">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="a6bcb-166">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_configure.png)

8. <span data-ttu-id="a6bcb-168">Egy másik webes böngészőablakban tooyour bejelentkezés **UNIFI** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-168">In a different web browser window, sign on tooyour **UNIFI** company site as administrator.</span></span>

9. <span data-ttu-id="a6bcb-169">Kattintson a hello **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-169">Click on hello **Users**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app1.png) 

10. <span data-ttu-id="a6bcb-171">Kattintson a hello **hozzáadása új identitásszolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-171">Click on hello **Add New Identity Provider**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app2.png)

11. <span data-ttu-id="a6bcb-173">A hello **identitásszolgáltató hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-173">In hello **Add Identity Provider** section, perform hello following steps:</span></span>   

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/app3.png) 

    <span data-ttu-id="a6bcb-175">a.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-175">a.</span></span> <span data-ttu-id="a6bcb-176">A hello **szolgáltatónevet** szövegmezőhöz hello identitásszolgáltató hello nevét...</span><span class="sxs-lookup"><span data-stu-id="a6bcb-176">In hello **Provider Name** textbox, type hello name of hello Identity Provider..</span></span>

    <span data-ttu-id="a6bcb-177">b.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-177">b.</span></span> <span data-ttu-id="a6bcb-178">A hello hello **szolgáltató URL-cím** szövegmező illessze be a hello **SAML-alapú egyszeri bejelentkezési URL-címe** értéket, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-178">In hello hello **Provider URL** textbox paste hello **SAML Single Sign-On Service URL** value, which you have copied from Azure portal.</span></span>

    <span data-ttu-id="a6bcb-179">c.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-179">c.</span></span> <span data-ttu-id="a6bcb-180">Nyissa meg hello hello Azure-portálon a Jegyzettömbben a letöltött tanúsítvány eltávolítása hello **---BEGIN CERTIFICATE---** és **---vége tanúsítvány---** címkét, és illessze be a fennmaradó hello Hello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-180">Open hello Certificate that you have downloaded from hello Azure portal in notepad, remove hello **---BEGIN CERTIFICATE---** and **---END CERTIFICATE---** tag and then paste hello remaining content in hello **Certificate** textbox.</span></span>

    <span data-ttu-id="a6bcb-181">d.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-181">d.</span></span> <span data-ttu-id="a6bcb-182">Jelölje be hello **alapértelmezett szolgáltató** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-182">Select hello **is Default Provider** checkbox.</span></span>

> [!TIP]
> <span data-ttu-id="a6bcb-183">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a6bcb-183">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a6bcb-184">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-184">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a6bcb-185">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a6bcb-185">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a6bcb-186">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-186">Creating an Azure AD test user</span></span>
<span data-ttu-id="a6bcb-187">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-187">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a6bcb-189">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a6bcb-189">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6bcb-190">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-190">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a6bcb-192">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-192">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a6bcb-194">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-194">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a6bcb-196">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a6bcb-196">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-unifi-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a6bcb-198">a.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-198">a.</span></span> <span data-ttu-id="a6bcb-199">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-199">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a6bcb-200">b.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-200">b.</span></span> <span data-ttu-id="a6bcb-201">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-201">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a6bcb-202">c.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-202">c.</span></span> <span data-ttu-id="a6bcb-203">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-203">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a6bcb-204">d.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-204">d.</span></span> <span data-ttu-id="a6bcb-205">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-205">Click **Create**.</span></span>
 
### <a name="creating-a-unifi-test-user"></a><span data-ttu-id="a6bcb-206">UNIFI tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a6bcb-206">Creating a UNIFI test user</span></span>

<span data-ttu-id="a6bcb-207">Ebben a szakaszban egy Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-207">In this section, you create a user called Britta Simon.</span></span> <span data-ttu-id="a6bcb-208">**UNIFI** támogatja az automatikus felhasználólétesítés, így manuális lépésekre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-208">**UNIFI** supports automatic user provisioning so no manual steps are required.</span></span> <span data-ttu-id="a6bcb-209">Felhasználók hello Azure AD a sikeres hitelesítés után automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-209">Users are created automatically after successful authentication from hello Azure AD.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a6bcb-210">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a6bcb-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a6bcb-211">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooUNIFI megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooUNIFI.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a6bcb-213">**tooassign Britta Simon tooUNIFI, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a6bcb-213">**tooassign Britta Simon tooUNIFI, perform hello following steps:**</span></span>

1. <span data-ttu-id="a6bcb-214">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-214">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a6bcb-216">Hello alkalmazások listában válassza ki a **UNIFI**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-216">In hello applications list, select **UNIFI**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-unifi-tutorial/tutorial_unifi_app.png) 

3. <span data-ttu-id="a6bcb-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a6bcb-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-220">Click **Add** button.</span></span> <span data-ttu-id="a6bcb-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a6bcb-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a6bcb-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a6bcb-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a6bcb-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a6bcb-226">Testing single sign-on</span></span>

<span data-ttu-id="a6bcb-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a6bcb-228">Hello UNIFI hello hozzáférési Panel csempére kattintva kapja meg automatikusan bejelentkezett tooyour UNIFI alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a6bcb-228">When you click hello UNIFI tile in hello Access Panel, you should get automatically signed-on tooyour UNIFI application.</span></span>
<span data-ttu-id="a6bcb-229">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a6bcb-229">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="a6bcb-230">További források</span><span class="sxs-lookup"><span data-stu-id="a6bcb-230">Additional resources</span></span>

* [<span data-ttu-id="a6bcb-231">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a6bcb-231">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a6bcb-232">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a6bcb-232">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-unifi-tutorial/tutorial_general_203.png

