---
title: "Oktatóanyag: Azure Active Directoryval integrált nagyítás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Nagyítás között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 0ebdab6c-83a8-4737-a86a-974f37269c31
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: jeedes
ms.openlocfilehash: 623a1f428ad1f0aa2c8205b79d61720cad5fc6a5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zoom"></a><span data-ttu-id="34ff8-103">Oktatóanyag: Azure Active Directoryval integrált nagyítás</span><span class="sxs-lookup"><span data-stu-id="34ff8-103">Tutorial: Azure Active Directory integration with Zoom</span></span>

<span data-ttu-id="34ff8-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate nagyítás, az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="34ff8-104">In this tutorial, you learn how toointegrate Zoom with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="34ff8-105">A Nagyítás integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-105">Integrating Zoom with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="34ff8-106">Az Azure AD hozzáférési tooZoom rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="34ff8-106">You can control in Azure AD who has access tooZoom.</span></span>
- <span data-ttu-id="34ff8-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZoom (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="34ff8-107">You can enable your users tooautomatically get signed-on tooZoom (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="34ff8-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="34ff8-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="34ff8-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="34ff8-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="34ff8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="34ff8-110">Prerequisites</span></span>

<span data-ttu-id="34ff8-111">Nagyítás tooconfigure az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="34ff8-111">tooconfigure Azure AD integration with Zoom, you need hello following items:</span></span>

- <span data-ttu-id="34ff8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="34ff8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="34ff8-113">A Nagyítás egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="34ff8-113">A Zoom single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="34ff8-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="34ff8-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="34ff8-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="34ff8-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="34ff8-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="34ff8-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="34ff8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="34ff8-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="34ff8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="34ff8-118">Scenario description</span></span>
<span data-ttu-id="34ff8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="34ff8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="34ff8-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="34ff8-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="34ff8-121">A Nagyítás hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="34ff8-121">Adding Zoom from hello gallery</span></span>
2. <span data-ttu-id="34ff8-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="34ff8-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-zoom-from-hello-gallery"></a><span data-ttu-id="34ff8-123">A Nagyítás hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="34ff8-123">Adding Zoom from hello gallery</span></span>
<span data-ttu-id="34ff8-124">tooconfigure hello integrációs Nagyítás az Azure AD-be, meg kell tooadd nagyítás hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="34ff8-124">tooconfigure hello integration of Zoom into Azure AD, you need tooadd Zoom from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="34ff8-125">**tooadd nagyítás hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="34ff8-125">**tooadd Zoom from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="34ff8-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="34ff8-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="34ff8-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="34ff8-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="34ff8-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="34ff8-133">Hello keresési mezőbe, írja be a **nagyítás**, jelölje be **nagyítás** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-133">In hello search box, type **Zoom**, select **Zoom** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Nagyítás hello eredményeinek listája](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="34ff8-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="34ff8-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="34ff8-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezést a nagyítás "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="34ff8-136">In this section, you configure and test Azure AD single sign-on with Zoom based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="34ff8-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói nagyítás tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="34ff8-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zoom is tooa user in Azure AD.</span></span> <span data-ttu-id="34ff8-138">Más szóval hivatkozás közötti kapcsolat egy Azure AD-felhasználó és a kapcsolódó felhasználói hello a Nagyítás igények toobe létrehozott.</span><span class="sxs-lookup"><span data-stu-id="34ff8-138">In other words, a link relationship between an Azure AD user and hello related user in Zoom needs toobe established.</span></span>

<span data-ttu-id="34ff8-139">A nagyítás, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="34ff8-139">In Zoom, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="34ff8-140">tooconfigure és a Nagyítás az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="34ff8-140">tooconfigure and test Azure AD single sign-on with Zoom, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="34ff8-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="34ff8-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="34ff8-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="34ff8-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="34ff8-143">**[A Nagyítás tesztfelhasználó létrehozása](#create-a-zoom-test-user)**  -toohave egy megfelelője a Britta Simon nagyítás, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="34ff8-143">**[Create a Zoom test user](#create-a-zoom-test-user)** - toohave a counterpart of Britta Simon in Zoom that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="34ff8-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="34ff8-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="34ff8-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="34ff8-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="34ff8-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="34ff8-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="34ff8-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a Nagyítás alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="34ff8-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Zoom application.</span></span>

<span data-ttu-id="34ff8-148">**Nagyítás, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="34ff8-148">**tooconfigure Azure AD single sign-on with Zoom, perform hello following steps:**</span></span>

1. <span data-ttu-id="34ff8-149">Az Azure portál, a hello hello **nagyítás** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-149">In hello Azure portal, on hello **Zoom** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="34ff8-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="34ff8-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_samlbase.png)

3. <span data-ttu-id="34ff8-153">A hello **nagyítás tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-153">On hello **Zoom Domain and URLs** section, perform hello following steps:</span></span>

    ![Nagyítás tartomány és az URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_url.png)

    <span data-ttu-id="34ff8-155">a.</span><span class="sxs-lookup"><span data-stu-id="34ff8-155">a.</span></span> <span data-ttu-id="34ff8-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="34ff8-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname>.zoom.us`</span></span>

    <span data-ttu-id="34ff8-157">b.</span><span class="sxs-lookup"><span data-stu-id="34ff8-157">b.</span></span> <span data-ttu-id="34ff8-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname>.zoom.us`</span><span class="sxs-lookup"><span data-stu-id="34ff8-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<companyname>.zoom.us`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="34ff8-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="34ff8-159">These values are not real.</span></span> <span data-ttu-id="34ff8-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="34ff8-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="34ff8-161">Ügyfél [nagyítás ügyfél-támogatási csoport](https://support.zoom.us/hc) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="34ff8-161">Contact [Zoom Client support team](https://support.zoom.us/hc) tooget these values.</span></span> 
 
4. <span data-ttu-id="34ff8-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="34ff8-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_certificate.png) 

5. <span data-ttu-id="34ff8-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-zoom-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="34ff8-166">A hello **nagyítás konfigurációs** területén kattintson **konfigurálása nagyítás** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="34ff8-166">On hello **Zoom Configuration** section, click **Configure Zoom** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="34ff8-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="34ff8-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![A Nagyítás konfiguráció](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_configure.png) 

7. <span data-ttu-id="34ff8-169">Egy másik webes böngészőablakban jelentkezzen tooyour nagyítás vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="34ff8-169">In a different web browser window, log in tooyour Zoom company site as an administrator.</span></span>

8. <span data-ttu-id="34ff8-170">Kattintson a hello **egyszeri bejelentkezés** fülre.</span><span class="sxs-lookup"><span data-stu-id="34ff8-170">Click hello **Single Sign-On** tab.</span></span>
   
    <span data-ttu-id="34ff8-171">![Egyszeri bejelentkezés lapon](./media/active-directory-saas-zoom-tutorial/IC784700.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="34ff8-171">![Single sign-on tab](./media/active-directory-saas-zoom-tutorial/IC784700.png "Single sign-on")</span></span>

9. <span data-ttu-id="34ff8-172">Hello kattintson **biztonsági ellenőrzést** lapot, és folytassa a toohello **egyszeri bejelentkezés** beállításait.</span><span class="sxs-lookup"><span data-stu-id="34ff8-172">Click hello **Security Control** tab, and then go toohello **Single Sign-On** settings.</span></span>

10. <span data-ttu-id="34ff8-173">Az egyszeri bejelentkezés szakasz hello hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-173">In hello Single Sign-On section, perform hello following steps:</span></span>
   
    <span data-ttu-id="34ff8-174">![Az egyszeri bejelentkezés szakasz](./media/active-directory-saas-zoom-tutorial/IC784701.png "egyszeri bejelentkezés")</span><span class="sxs-lookup"><span data-stu-id="34ff8-174">![Single sign-on section](./media/active-directory-saas-zoom-tutorial/IC784701.png "Single sign-on")</span></span>
   
    <span data-ttu-id="34ff8-175">a.</span><span class="sxs-lookup"><span data-stu-id="34ff8-175">a.</span></span> <span data-ttu-id="34ff8-176">A hello **bejelentkezési lap URL** szövegmezőhöz Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="34ff8-176">In hello **Sign-in page URL** textbox, paste hello value of **SAML Single Sign-On Service URL**, which you have copied from Azure portal.</span></span>
   
    <span data-ttu-id="34ff8-177">b.</span><span class="sxs-lookup"><span data-stu-id="34ff8-177">b.</span></span> <span data-ttu-id="34ff8-178">A hello **kijelentkezési URL-címe** szövegmezőhöz Beillesztés hello értékének **Sign-Out URL-cím**, amely Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="34ff8-178">In hello **Sign-out page URL** textbox, paste hello value of **Sign-Out URL**, which you have copied from Azure portal.</span></span>
     
    <span data-ttu-id="34ff8-179">c.</span><span class="sxs-lookup"><span data-stu-id="34ff8-179">c.</span></span> <span data-ttu-id="34ff8-180">Nyissa meg a base-64 kódolású tanúsítvány a Jegyzettömbben, a vágólapra tartalmának másolása hello és toohello Beillesztés **szolgáltató identitástanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="34ff8-180">Open your base-64 encoded certificate in notepad, copy hello content of it into your clipboard, and then paste it toohello **Identity provider certificate** textbox.</span></span>

    <span data-ttu-id="34ff8-181">d.</span><span class="sxs-lookup"><span data-stu-id="34ff8-181">d.</span></span> <span data-ttu-id="34ff8-182">A hello **kibocsátó** szövegmezőhöz Beillesztés hello értékének **SAML Entitásazonosító**, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="34ff8-182">In hello **Issuer** textbox, paste hello value of **SAML Entity ID**, which you have copied from Azure portal.</span></span> 

    <span data-ttu-id="34ff8-183">e.</span><span class="sxs-lookup"><span data-stu-id="34ff8-183">e.</span></span> <span data-ttu-id="34ff8-184">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-184">Click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="34ff8-185">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="34ff8-185">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="34ff8-186">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="34ff8-186">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="34ff8-187">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="34ff8-187">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="34ff8-188">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="34ff8-188">Create an Azure AD test user</span></span>

<span data-ttu-id="34ff8-189">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="34ff8-189">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="34ff8-191">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="34ff8-191">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="34ff8-192">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-192">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-zoom-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="34ff8-194">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-194">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-zoom-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="34ff8-196">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="34ff8-196">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-zoom-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="34ff8-198">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-198">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-zoom-tutorial/create_aaduser_04.png)

    <span data-ttu-id="34ff8-200">a.</span><span class="sxs-lookup"><span data-stu-id="34ff8-200">a.</span></span> <span data-ttu-id="34ff8-201">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-201">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="34ff8-202">b.</span><span class="sxs-lookup"><span data-stu-id="34ff8-202">b.</span></span> <span data-ttu-id="34ff8-203">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="34ff8-203">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="34ff8-204">c.</span><span class="sxs-lookup"><span data-stu-id="34ff8-204">c.</span></span> <span data-ttu-id="34ff8-205">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="34ff8-205">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="34ff8-206">d.</span><span class="sxs-lookup"><span data-stu-id="34ff8-206">d.</span></span> <span data-ttu-id="34ff8-207">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-207">Click **Create**.</span></span>
 
### <a name="create-a-zoom-test-user"></a><span data-ttu-id="34ff8-208">A Nagyítás tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="34ff8-208">Create a Zoom test user</span></span>

<span data-ttu-id="34ff8-209">A sorrend tooenable az Azure AD felhasználók toolog a tooZoom azok kell üzembe Nagyítás be.</span><span class="sxs-lookup"><span data-stu-id="34ff8-209">In order tooenable Azure AD users toolog in tooZoom, they must be provisioned into Zoom.</span></span> <span data-ttu-id="34ff8-210">A Nagyítás hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="34ff8-210">In hello case of Zoom, provisioning is a manual task.</span></span>

### <a name="tooprovision-a-user-account-perform-hello-following-steps"></a><span data-ttu-id="34ff8-211">tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-211">tooprovision a user account, perform hello following steps:</span></span>

1. <span data-ttu-id="34ff8-212">Jelentkezzen be tooyour **nagyítás** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="34ff8-212">Log in tooyour **Zoom** company site as an administrator.</span></span>
 
2. <span data-ttu-id="34ff8-213">Kattintson a hello **felhasználóifiók-kezelés** fülre, majd **felhasználókezelés**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-213">Click hello **Account Management** tab, and then click **User Management**.</span></span>

3. <span data-ttu-id="34ff8-214">Hello felhasználókezelés szakaszban, kattintson **felhasználók hozzáadása az**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-214">In hello User Management section, click **Add users**.</span></span>
   
    <span data-ttu-id="34ff8-215">![Felhasználókezelés](./media/active-directory-saas-zoom-tutorial/IC784703.png "felhasználók kezelése")</span><span class="sxs-lookup"><span data-stu-id="34ff8-215">![User management](./media/active-directory-saas-zoom-tutorial/IC784703.png "User management")</span></span>

4. <span data-ttu-id="34ff8-216">A hello **felhasználók hozzáadása az** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="34ff8-216">On hello **Add users** page, perform hello following steps:</span></span>
   
    <span data-ttu-id="34ff8-217">![Felhasználók hozzáadása az](./media/active-directory-saas-zoom-tutorial/IC784704.png "felhasználók hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="34ff8-217">![Add users](./media/active-directory-saas-zoom-tutorial/IC784704.png "Add users")</span></span>
   
    <span data-ttu-id="34ff8-218">a.</span><span class="sxs-lookup"><span data-stu-id="34ff8-218">a.</span></span> <span data-ttu-id="34ff8-219">Mint **felhasználótípust**, jelölje be **alapvető**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-219">As **User Type**, select **Basic**.</span></span>

    <span data-ttu-id="34ff8-220">b.</span><span class="sxs-lookup"><span data-stu-id="34ff8-220">b.</span></span> <span data-ttu-id="34ff8-221">A hello **e-mailek** szövegmezőhöz típusú hello e-mail cím egy érvényes Azure AD-fiókot szeretne tooprovision.</span><span class="sxs-lookup"><span data-stu-id="34ff8-221">In hello **Emails** textbox, type hello email address of a valid Azure AD account you want tooprovision.</span></span>

    <span data-ttu-id="34ff8-222">c.</span><span class="sxs-lookup"><span data-stu-id="34ff8-222">c.</span></span> <span data-ttu-id="34ff8-223">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-223">Click **Add**.</span></span>

> [!NOTE]
> <span data-ttu-id="34ff8-224">Bármely más nagyítás felhasználói fiók létrehozása eszközök vagy nagyítás tooprovision Azure Active Directory által nyújtott API-k felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="34ff8-224">You can use any other Zoom user account creation tools or APIs provided by Zoom tooprovision Azure Active Directory user accounts.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="34ff8-225">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="34ff8-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="34ff8-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooZoom megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="34ff8-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooZoom.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="34ff8-228">**tooassign Britta Simon tooZoom, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="34ff8-228">**tooassign Britta Simon tooZoom, perform hello following steps:**</span></span>

1. <span data-ttu-id="34ff8-229">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-229">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="34ff8-231">Hello alkalmazások listában válassza ki a **nagyítás**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-231">In hello applications list, select **Zoom**.</span></span>

    ![hello nagyítás hivatkozásra hello alkalmazások listája](./media/active-directory-saas-zoom-tutorial/tutorial_zoom_app.png)  

3. <span data-ttu-id="34ff8-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="34ff8-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="34ff8-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="34ff8-235">Click **Add** button.</span></span> <span data-ttu-id="34ff8-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="34ff8-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="34ff8-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="34ff8-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="34ff8-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="34ff8-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="34ff8-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="34ff8-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="34ff8-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="34ff8-241">Test single sign-on</span></span>

<span data-ttu-id="34ff8-242">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="34ff8-242">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="34ff8-243">Hello nagyítás csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour nagyítás alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="34ff8-243">When you click hello Zoom tile in hello Access Panel, you should get automatically signed-on tooyour Zoom application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="34ff8-244">További források</span><span class="sxs-lookup"><span data-stu-id="34ff8-244">Additional resources</span></span>

* [<span data-ttu-id="34ff8-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="34ff8-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="34ff8-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="34ff8-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zoom-tutorial/tutorial_general_203.png

