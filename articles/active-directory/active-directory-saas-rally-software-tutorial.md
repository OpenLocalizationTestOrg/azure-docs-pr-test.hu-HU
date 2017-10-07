---
title: "Oktatóanyag: Azure Active Directory-integráció Rally szoftverrel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a szoftver Rally között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: ba25fade-e152-42dd-8377-a30bbc48c3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: jeedes
ms.openlocfilehash: c75c8b98ce7fab19964c13de5ad7e19ef3ebd0e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-rally-software"></a><span data-ttu-id="30d5e-103">Oktatóanyag: Azure Active Directory-integráció Rally szoftverrel</span><span class="sxs-lookup"><span data-stu-id="30d5e-103">Tutorial: Azure Active Directory integration with Rally Software</span></span>

<span data-ttu-id="30d5e-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Rally szoftver az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="30d5e-104">In this tutorial, you learn how toointegrate Rally Software with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="30d5e-105">Szoftver Rally integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="30d5e-105">Integrating Rally Software with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="30d5e-106">Az Azure AD hozzáférési tooRally szoftver rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="30d5e-106">You can control in Azure AD who has access tooRally Software.</span></span>
- <span data-ttu-id="30d5e-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooRally szoftver (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="30d5e-107">You can enable your users tooautomatically get signed-on tooRally Software (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="30d5e-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="30d5e-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="30d5e-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="30d5e-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="30d5e-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="30d5e-110">Prerequisites</span></span>

<span data-ttu-id="30d5e-111">tooconfigure Rally szoftver az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="30d5e-111">tooconfigure Azure AD integration with Rally Software, you need hello following items:</span></span>

- <span data-ttu-id="30d5e-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="30d5e-112">An Azure AD subscription</span></span>
- <span data-ttu-id="30d5e-113">A szoftver Rally egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="30d5e-113">A Rally Software single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="30d5e-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="30d5e-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="30d5e-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="30d5e-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="30d5e-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="30d5e-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="30d5e-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="30d5e-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="30d5e-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="30d5e-118">Scenario description</span></span>
<span data-ttu-id="30d5e-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="30d5e-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="30d5e-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="30d5e-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="30d5e-121">Hello gyűjteményből Rally szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30d5e-121">Adding Rally Software from hello gallery</span></span>
2. <span data-ttu-id="30d5e-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="30d5e-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-rally-software-from-hello-gallery"></a><span data-ttu-id="30d5e-123">Hello gyűjteményből Rally szoftver hozzáadása</span><span class="sxs-lookup"><span data-stu-id="30d5e-123">Adding Rally Software from hello gallery</span></span>
<span data-ttu-id="30d5e-124">tooconfigure hello integrációs Rally szoftver az Azure AD-be, meg kell tooadd Rally szoftver hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="30d5e-124">tooconfigure hello integration of Rally Software into Azure AD, you need tooadd Rally Software from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="30d5e-125">**tooadd Rally szoftver hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="30d5e-125">**tooadd Rally Software from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="30d5e-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="30d5e-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="30d5e-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="30d5e-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="30d5e-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="30d5e-133">Hello keresési mezőbe, írja be a **Rally szoftver**, jelölje be **Rally szoftver** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-133">In hello search box, type **Rally Software**, select **Rally Software** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Szoftver Rally hello eredmények listájában](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="30d5e-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30d5e-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="30d5e-136">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Rally szoftverrel.</span><span class="sxs-lookup"><span data-stu-id="30d5e-136">In this section, you configure and test Azure AD single sign-on with Rally Software based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="30d5e-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Rally szoftverfrissítési tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="30d5e-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Rally Software is tooa user in Azure AD.</span></span> <span data-ttu-id="30d5e-138">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználói hello Rally szoftver közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="30d5e-138">In other words, a link relationship between an Azure AD user and hello related user in Rally Software needs toobe established.</span></span>

<span data-ttu-id="30d5e-139">Szoftver Rally rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="30d5e-139">In Rally Software, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="30d5e-140">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése Rally szoftverrel, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="30d5e-140">tooconfigure and test Azure AD single sign-on with Rally Software, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="30d5e-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="30d5e-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="30d5e-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="30d5e-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="30d5e-143">**[Szoftver Rally tesztfelhasználó létrehozása](#create-a-rally-software-test-user)**  -toohave egy megfelelője a Britta Simon Rally szoftver, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="30d5e-143">**[Create a Rally Software test user](#create-a-rally-software-test-user)** - toohave a counterpart of Britta Simon in Rally Software that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="30d5e-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="30d5e-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="30d5e-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="30d5e-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="30d5e-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="30d5e-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="30d5e-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és a Rally alkalmazás egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="30d5e-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Rally Software application.</span></span>

<span data-ttu-id="30d5e-148">**az Azure AD tooconfigure egyszeri bejelentkezés Rally szoftvert, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="30d5e-148">**tooconfigure Azure AD single sign-on with Rally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="30d5e-149">Az Azure portál, a hello hello **Rally szoftver** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-149">In hello Azure portal, on hello **Rally Software** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="30d5e-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="30d5e-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_samlbase.png)

3. <span data-ttu-id="30d5e-153">A hello **Rally szoftver tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30d5e-153">On hello **Rally Software Domain and URLs** section, perform hello following steps:</span></span>

    ![Szoftver tartomány és az URL-címek egyetlen bejelentkezési adatokat Rally](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_url.png)

    <span data-ttu-id="30d5e-155">a.</span><span class="sxs-lookup"><span data-stu-id="30d5e-155">a.</span></span> <span data-ttu-id="30d5e-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="30d5e-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    <span data-ttu-id="30d5e-157">b.</span><span class="sxs-lookup"><span data-stu-id="30d5e-157">b.</span></span> <span data-ttu-id="30d5e-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<tenant-name>.rally.com`</span><span class="sxs-lookup"><span data-stu-id="30d5e-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<tenant-name>.rally.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="30d5e-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="30d5e-159">These values are not real.</span></span> <span data-ttu-id="30d5e-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="30d5e-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="30d5e-161">Ügyfél [Rally szoftver ügyfél-támogatási csoport](https://help.rallydev.com/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="30d5e-161">Contact [Rally Software Client support team](https://help.rallydev.com/) tooget these values.</span></span> 
 


4. <span data-ttu-id="30d5e-162">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="30d5e-162">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_certificate.png) 

5. <span data-ttu-id="30d5e-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-rally-software-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="30d5e-166">A hello **szoftverkonfigurációt Rally** kattintson **Rally szoftver konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="30d5e-166">On hello **Rally Software Configuration** section, click **Configure Rally Software** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="30d5e-167">Másolás hello **Sign-Out URL-címet, és a SAML Entitásazonosító** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="30d5e-167">Copy hello **Sign-Out URL, and SAML Entity ID** from hello **Quick Reference section.**</span></span>

    ![Rally szoftverkonfigurációjának összeállítása](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_configure.png) 

7. <span data-ttu-id="30d5e-169">Jelentkezzen be tooyour **Rally szoftver** bérlő.</span><span class="sxs-lookup"><span data-stu-id="30d5e-169">Log in tooyour **Rally Software** tenant.</span></span>

8. <span data-ttu-id="30d5e-170">Hello hello felső eszköztárán kattintson **telepítő**, majd válassza ki **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-170">In hello toolbar on hello top, click **Setup**, and then select **Subscription**.</span></span>
   
    <span data-ttu-id="30d5e-171">![Előfizetés](./media/active-directory-saas-rally-software-tutorial/ic769531.png "előfizetés")</span><span class="sxs-lookup"><span data-stu-id="30d5e-171">![Subscription](./media/active-directory-saas-rally-software-tutorial/ic769531.png "Subscription")</span></span>

9. <span data-ttu-id="30d5e-172">Kattintson a hello **művelet** gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-172">Click hello **Action** button.</span></span> <span data-ttu-id="30d5e-173">Válassza ki **előfizetés szerkesztése** hello felső jobb oldalán hello eszköztár.</span><span class="sxs-lookup"><span data-stu-id="30d5e-173">Select **Edit Subscription** at hello top right side of hello toolbar.</span></span>

10. <span data-ttu-id="30d5e-174">A hello **előfizetés** párbeszédpanel lapon hajtsa végre az alábbi lépésekkel hello, és kattintson a **mentés és Bezárás**:</span><span class="sxs-lookup"><span data-stu-id="30d5e-174">On hello **Subscription** dialog page, perform hello following steps, and then click **Save & Close**:</span></span>
   
    <span data-ttu-id="30d5e-175">![Hitelesítési](./media/active-directory-saas-rally-software-tutorial/ic769542.png "hitelesítés")</span><span class="sxs-lookup"><span data-stu-id="30d5e-175">![Authentication](./media/active-directory-saas-rally-software-tutorial/ic769542.png "Authentication")</span></span>
   
    <span data-ttu-id="30d5e-176">a.</span><span class="sxs-lookup"><span data-stu-id="30d5e-176">a.</span></span> <span data-ttu-id="30d5e-177">Válassza ki **Rally vagy az SSO hitelesítési** hitelesítési legördülő menüből.</span><span class="sxs-lookup"><span data-stu-id="30d5e-177">Select **Rally or SSO authentication** from Authentication dropdown.</span></span>

    <span data-ttu-id="30d5e-178">b.</span><span class="sxs-lookup"><span data-stu-id="30d5e-178">b.</span></span> <span data-ttu-id="30d5e-179">A hello **identitási szolgáltató URL-cím** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="30d5e-179">In hello **Identity provider URL** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="30d5e-180">c.</span><span class="sxs-lookup"><span data-stu-id="30d5e-180">c.</span></span> <span data-ttu-id="30d5e-181">A hello **egyszeri bejelentkezési, kijelentkezési** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="30d5e-181">In hello **SSO Logout** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>

> [!TIP]
> <span data-ttu-id="30d5e-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="30d5e-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="30d5e-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="30d5e-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="30d5e-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="30d5e-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="30d5e-185">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="30d5e-185">Create an Azure AD test user</span></span>

<span data-ttu-id="30d5e-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="30d5e-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="30d5e-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="30d5e-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="30d5e-189">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-189">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-rally-software-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="30d5e-191">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-191">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-rally-software-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="30d5e-193">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="30d5e-193">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-rally-software-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="30d5e-195">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30d5e-195">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-rally-software-tutorial/create_aaduser_04.png)

    <span data-ttu-id="30d5e-197">a.</span><span class="sxs-lookup"><span data-stu-id="30d5e-197">a.</span></span> <span data-ttu-id="30d5e-198">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-198">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="30d5e-199">b.</span><span class="sxs-lookup"><span data-stu-id="30d5e-199">b.</span></span> <span data-ttu-id="30d5e-200">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="30d5e-200">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="30d5e-201">c.</span><span class="sxs-lookup"><span data-stu-id="30d5e-201">c.</span></span> <span data-ttu-id="30d5e-202">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="30d5e-202">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="30d5e-203">d.</span><span class="sxs-lookup"><span data-stu-id="30d5e-203">d.</span></span> <span data-ttu-id="30d5e-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-204">Click **Create**.</span></span>
 
### <a name="create-a-rally-software-test-user"></a><span data-ttu-id="30d5e-205">Szoftver Rally tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="30d5e-205">Create a Rally Software test user</span></span>

<span data-ttu-id="30d5e-206">Az Azure AD felhasználók toobe képes toosign a, a kiépített toohello Rally alkalmazás használatával az Azure Active Directory felhasználói neveket kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="30d5e-206">For Azure AD users toobe able toosign in, they must be provisioned toohello Rally Software application using their Azure Active Directory user names.</span></span>

<span data-ttu-id="30d5e-207">**tooconfigure felhasználók átadásához, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="30d5e-207">**tooconfigure user provisioning, perform hello following steps:**</span></span>

1. <span data-ttu-id="30d5e-208">Jelentkezzen be tooyour Rally szoftver bérlő.</span><span class="sxs-lookup"><span data-stu-id="30d5e-208">Log in tooyour Rally Software tenant.</span></span>

2. <span data-ttu-id="30d5e-209">Nyissa meg túl**telepítő \> felhasználók**, és kattintson a **+ Hozzáadás új**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-209">Go too**Setup \> USERS**, and then click **+ Add New**.</span></span>
   
    <span data-ttu-id="30d5e-210">![Felhasználók](./media/active-directory-saas-rally-software-tutorial/ic781039.png "felhasználók")</span><span class="sxs-lookup"><span data-stu-id="30d5e-210">![Users](./media/active-directory-saas-rally-software-tutorial/ic781039.png "Users")</span></span>

3. <span data-ttu-id="30d5e-211">Hello típusnév hello új felhasználó szövegmezőben, és kattintson **Hozzáadás adatokkal**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-211">Type hello name in hello New User textbox, and then click **Add with Details**.</span></span>

4. <span data-ttu-id="30d5e-212">A hello **felhasználó létrehozása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="30d5e-212">In hello **Create User** section, perform hello following steps:</span></span>
   
    <span data-ttu-id="30d5e-213">![Hozzon létre felhasználói](./media/active-directory-saas-rally-software-tutorial/ic781040.png "felhasználó létrehozása")</span><span class="sxs-lookup"><span data-stu-id="30d5e-213">![Create User](./media/active-directory-saas-rally-software-tutorial/ic781040.png "Create User")</span></span>

    <span data-ttu-id="30d5e-214">a.</span><span class="sxs-lookup"><span data-stu-id="30d5e-214">a.</span></span> <span data-ttu-id="30d5e-215">A hello **felhasználónév** szövegmező, például a felhasználó hello típusnév **Brittsimon**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-215">In hello **User Name** textbox, type hello name of user like **Brittsimon**.</span></span>
   
    <span data-ttu-id="30d5e-216">b.</span><span class="sxs-lookup"><span data-stu-id="30d5e-216">b.</span></span> <span data-ttu-id="30d5e-217">A **E-mail cím** szövegmező, írja be például a felhasználó hello e-mailek  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="30d5e-217">In **E-mail Address** textbox, enter hello email of user like **brittasimon@contoso.com**.</span></span>

    <span data-ttu-id="30d5e-218">c.</span><span class="sxs-lookup"><span data-stu-id="30d5e-218">c.</span></span> <span data-ttu-id="30d5e-219">A **Utónév** szöveg mezőbe írja be például a felhasználó utónevét hello **Britta**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-219">In **First Name** text box, enter hello first name of user like **Britta**.</span></span>

    <span data-ttu-id="30d5e-220">d.</span><span class="sxs-lookup"><span data-stu-id="30d5e-220">d.</span></span> <span data-ttu-id="30d5e-221">A **Vezetéknév** szöveg mezőbe írja be például a felhasználó vezetékneve hello **Simon**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-221">In **Last Name** text box, enter hello last name of user like **Simon**.</span></span>

    <span data-ttu-id="30d5e-222">e.</span><span class="sxs-lookup"><span data-stu-id="30d5e-222">e.</span></span> <span data-ttu-id="30d5e-223">Kattintson a **mentés és Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-223">Click **Save & Close**.</span></span>

   >[!NOTE]
   ><span data-ttu-id="30d5e-224">Bármely más Rally felhasználói fiók létrehozása szoftvereszközök is használhatja, vagy Rally szoftver tooprovision által nyújtott API-k az Azure AD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="30d5e-224">You can use any other Rally Software user account creation tools or APIs provided by Rally Software tooprovision Azure AD user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="30d5e-225">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="30d5e-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="30d5e-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooRally szoftver megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="30d5e-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooRally Software.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="30d5e-228">**tooassign Britta Simon tooRally szoftver, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="30d5e-228">**tooassign Britta Simon tooRally Software, perform hello following steps:**</span></span>

1. <span data-ttu-id="30d5e-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="30d5e-231">Hello alkalmazások listában válassza ki a **Rally szoftver**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-231">In hello applications list, select **Rally Software**.</span></span>

    ![hello Rally szoftverhivatkozás hello alkalmazások listájában](./media/active-directory-saas-rally-software-tutorial/tutorial_rallysoftware_app.png)  

3. <span data-ttu-id="30d5e-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="30d5e-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="30d5e-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="30d5e-235">Click **Add** button.</span></span> <span data-ttu-id="30d5e-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30d5e-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="30d5e-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="30d5e-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="30d5e-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30d5e-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="30d5e-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="30d5e-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="30d5e-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="30d5e-241">Test single sign-on</span></span>

<span data-ttu-id="30d5e-242">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="30d5e-242">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="30d5e-243">Hello Rally szoftver csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Rally alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="30d5e-243">When you click hello Rally Software tile in hello Access Panel, you should get automatically signed-on tooyour Rally Software application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30d5e-244">További források</span><span class="sxs-lookup"><span data-stu-id="30d5e-244">Additional resources</span></span>

* [<span data-ttu-id="30d5e-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="30d5e-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="30d5e-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="30d5e-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-rally-software-tutorial/tutorial_general_203.png

