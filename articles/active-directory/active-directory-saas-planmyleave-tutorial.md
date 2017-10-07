---
title: "Oktatóanyag: Azure Active Directoryval integrált PlanMyLeave |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és PlanMyLeave között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b0d31cbe-7ae2-488b-9cf3-4927391fa744
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/01/2017
ms.author: jeedes
ms.openlocfilehash: 44a6782e44ef22fc957544960be1742f9eed6e51
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-planmyleave"></a><span data-ttu-id="486ab-103">Oktatóanyag: Azure Active Directoryval integrált PlanMyLeave</span><span class="sxs-lookup"><span data-stu-id="486ab-103">Tutorial: Azure Active Directory integration with PlanMyLeave</span></span>

<span data-ttu-id="486ab-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate PlanMyLeave az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="486ab-104">In this tutorial, you learn how toointegrate PlanMyLeave with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="486ab-105">PlanMyLeave integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="486ab-105">Integrating PlanMyLeave with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="486ab-106">Megadhatja a hozzáférés tooPlanMyLeave rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="486ab-106">You can control in Azure AD who has access tooPlanMyLeave</span></span>
- <span data-ttu-id="486ab-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPlanMyLeave (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="486ab-107">You can enable your users tooautomatically get signed-on tooPlanMyLeave (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="486ab-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="486ab-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="486ab-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="486ab-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="486ab-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="486ab-110">Prerequisites</span></span>

<span data-ttu-id="486ab-111">az Azure AD integrálása PlanMyLeave tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="486ab-111">tooconfigure Azure AD integration with PlanMyLeave, you need hello following items:</span></span>

- <span data-ttu-id="486ab-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="486ab-112">An Azure AD subscription</span></span>
- <span data-ttu-id="486ab-113">Egy PlanMyLeave egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="486ab-113">A PlanMyLeave single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="486ab-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="486ab-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="486ab-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="486ab-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="486ab-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="486ab-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="486ab-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="486ab-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="486ab-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="486ab-118">Scenario description</span></span>
<span data-ttu-id="486ab-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="486ab-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="486ab-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="486ab-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="486ab-121">Hello gyűjteményből PlanMyLeave hozzáadása</span><span class="sxs-lookup"><span data-stu-id="486ab-121">Adding PlanMyLeave from hello gallery</span></span>
2. <span data-ttu-id="486ab-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="486ab-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-planmyleave-from-hello-gallery"></a><span data-ttu-id="486ab-123">Hello gyűjteményből PlanMyLeave hozzáadása</span><span class="sxs-lookup"><span data-stu-id="486ab-123">Adding PlanMyLeave from hello gallery</span></span>
<span data-ttu-id="486ab-124">tooconfigure hello integrációja PlanMyLeave az Azure AD-be, meg kell tooadd PlanMyLeave hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="486ab-124">tooconfigure hello integration of PlanMyLeave into Azure AD, you need tooadd PlanMyLeave from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="486ab-125">**tooadd PlanMyLeave hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="486ab-125">**tooadd PlanMyLeave from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="486ab-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="486ab-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="486ab-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="486ab-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="486ab-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="486ab-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="486ab-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="486ab-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="486ab-133">Hello keresési mezőbe, írja be a **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="486ab-133">In hello search box, type **PlanMyLeave**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_001.png)

5. <span data-ttu-id="486ab-135">A hello eredmények panelen válassza ki a **PlanMyLeave**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-135">In hello results panel, select **PlanMyLeave**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="486ab-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="486ab-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="486ab-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="486ab-138">In this section, you configure and test Azure AD single sign-on with PlanMyLeave based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="486ab-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó PlanMyLeave tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="486ab-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in PlanMyLeave is tooa user in Azure AD.</span></span> <span data-ttu-id="486ab-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello PlanMyLeave közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="486ab-140">In other words, a link relationship between an Azure AD user and hello related user in PlanMyLeave needs toobe established.</span></span>

<span data-ttu-id="486ab-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a PlanMyLeave.</span><span class="sxs-lookup"><span data-stu-id="486ab-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in PlanMyLeave.</span></span>

<span data-ttu-id="486ab-142">tooconfigure és az Azure AD az egyszeri bejelentkezés PlanMyLeave-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="486ab-142">tooconfigure and test Azure AD single sign-on with PlanMyLeave, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="486ab-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="486ab-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="486ab-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="486ab-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="486ab-145">**[PlanMyLeave tesztfelhasználó létrehozása](#creating-a-planmyleave-test-user)**  -toohave Britta Simon PlanMyLeave, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="486ab-145">**[Creating a PlanMyLeave test user](#creating-a-planmyleave-test-user)** - toohave a counterpart of Britta Simon in PlanMyLeave that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="486ab-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="486ab-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="486ab-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="486ab-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="486ab-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="486ab-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="486ab-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az PlanMyLeave alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="486ab-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your PlanMyLeave application.</span></span>

<span data-ttu-id="486ab-150">**az Azure AD tooconfigure egyszeri bejelentkezést a PlanMyLeave, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="486ab-150">**tooconfigure Azure AD single sign-on with PlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="486ab-151">Hello Azure felügyeleti portálon, a hello **PlanMyLeave** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="486ab-151">In hello Azure Management portal, on hello **PlanMyLeave** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="486ab-153">A hello **egyszeri bejelentkezés** párbeszédpanel lap, **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="486ab-153">On hello **Single sign-on** dialog page, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_01.png)

3. <span data-ttu-id="486ab-155">A hello **PlanMyLeave tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="486ab-155">On hello **PlanMyLeave Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_02.png)

    <span data-ttu-id="486ab-157">a.</span><span class="sxs-lookup"><span data-stu-id="486ab-157">a.</span></span> <span data-ttu-id="486ab-158">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company-name>.planmyleave.com/Login.aspx`</span><span class="sxs-lookup"><span data-stu-id="486ab-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com/Login.aspx`</span></span>
    
    <span data-ttu-id="486ab-159">b.</span><span class="sxs-lookup"><span data-stu-id="486ab-159">b.</span></span> <span data-ttu-id="486ab-160">A hello **azonosítóját** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company-name>.planmyleave.com`</span><span class="sxs-lookup"><span data-stu-id="486ab-160">In hello **Identifer** textbox, type a URL using hello following pattern: `https://<company-name>.planmyleave.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="486ab-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="486ab-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="486ab-162">Ezeket az értékeket a tényleges hello bejelentkezési URL-cím és azonosító tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="486ab-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="486ab-163">Ügyfél [PlanMyLeave támogatási csoport](mailto:support@planmyleave.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="486ab-163">Contact [PlanMyLeave support team](mailto:support@planmyleave.com) tooget these values.</span></span>

4. <span data-ttu-id="486ab-164">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="486ab-164">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_03.png)     

5. <span data-ttu-id="486ab-166">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="486ab-166">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="486ab-167">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-167">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_300.png)

6. <span data-ttu-id="486ab-169">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-169">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_04.png)

7. <span data-ttu-id="486ab-171">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="486ab-171">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="486ab-173">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="486ab-173">On hello **SAML Signing Certificate** section, click **Certificate (base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_05.png) 

9. <span data-ttu-id="486ab-175">A hello **PlanMyLeave konfigurációs** kattintson **konfigurálása PlanMyLeave** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="486ab-175">On hello **PlanMyLeave Configuration** section, click **Configure PlanMyLeave** tooopen **Configure sign-on** window.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_06.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_07.png)

10. <span data-ttu-id="486ab-178">Egy másik webes böngészőablakban bejelentkezni a PlanMyLeave Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="486ab-178">In a different web browser window, log into your PlanMyLeave tenant as an administrator.</span></span>

11. <span data-ttu-id="486ab-179">Nyissa meg túl**rendszerbeállítás**.</span><span class="sxs-lookup"><span data-stu-id="486ab-179">Go too**System Setup**.</span></span> <span data-ttu-id="486ab-180">Végül a hello **biztonságkezelés** szakaszban kattintson **vállalati SAML beállítások** .</span><span class="sxs-lookup"><span data-stu-id="486ab-180">Then on hello **Security Management** section click **Company SAML settings** .</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_002.png) 

12. <span data-ttu-id="486ab-182">A hello **SAML beállítások** területen kattintson a szerkesztőben ikonra.</span><span class="sxs-lookup"><span data-stu-id="486ab-182">On hello **SAML Settings** section, click editor icon.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_003.png)

13. <span data-ttu-id="486ab-184">A hello **SAML beállítások** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="486ab-184">On hello **Update SAML Settings** section, perform hello following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_004.png)

    <span data-ttu-id="486ab-186">a.</span><span class="sxs-lookup"><span data-stu-id="486ab-186">a.</span></span>  <span data-ttu-id="486ab-187">A hello **bejelentkezési URL-cím** szövegmező, írja be a hello értéket **SAML-alapú egyszeri bejelentkezési URL-címe** az Azure AD alkalmazás-konfigurációs ablakban.</span><span class="sxs-lookup"><span data-stu-id="486ab-187">In hello **Login URL** textbox, put hello value of **SAML Single Sign-On Service URL** from Azure AD application configuration window.</span></span>

    <span data-ttu-id="486ab-188">b.</span><span class="sxs-lookup"><span data-stu-id="486ab-188">b.</span></span>  <span data-ttu-id="486ab-189">Nyissa meg a letöltött fájlt a Jegyzettömbben. csak hello tartalom közötti hello---megkezdéséhez tanúsítvány---és---vége tanúsítvány---azt a vágólapra másolja, majd illessze be toohello **tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="486ab-189">Open your downloaded certificate file in notepad, copy only hello content between hello ---Begin Certificate--- and ---End certificate---- of it into your clipboard, and then paste it toohello **Certificate** textbox.</span></span>

    <span data-ttu-id="486ab-190">c.</span><span class="sxs-lookup"><span data-stu-id="486ab-190">c.</span></span> <span data-ttu-id="486ab-191">Állítsa be"**engedélyezése van**"túl"**Igen**".</span><span class="sxs-lookup"><span data-stu-id="486ab-191">Set "**Is Enable**" too"**Yes**".</span></span>

    <span data-ttu-id="486ab-192">d.</span><span class="sxs-lookup"><span data-stu-id="486ab-192">d.</span></span> <span data-ttu-id="486ab-193">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-193">Click **Save**.</span></span>



### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="486ab-194">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="486ab-194">Creating an Azure AD test user</span></span>
<span data-ttu-id="486ab-195">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="486ab-195">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="486ab-197">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="486ab-197">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="486ab-198">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="486ab-198">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="486ab-200">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="486ab-200">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="486ab-202">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="486ab-202">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="486ab-204">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="486ab-204">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-planmyleave-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="486ab-206">a.</span><span class="sxs-lookup"><span data-stu-id="486ab-206">a.</span></span> <span data-ttu-id="486ab-207">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="486ab-207">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="486ab-208">b.</span><span class="sxs-lookup"><span data-stu-id="486ab-208">b.</span></span> <span data-ttu-id="486ab-209">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="486ab-209">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="486ab-210">c.</span><span class="sxs-lookup"><span data-stu-id="486ab-210">c.</span></span> <span data-ttu-id="486ab-211">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="486ab-211">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="486ab-212">d.</span><span class="sxs-lookup"><span data-stu-id="486ab-212">d.</span></span> <span data-ttu-id="486ab-213">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-213">Click **Create**.</span></span> 



### <a name="creating-a-planmyleave-test-user"></a><span data-ttu-id="486ab-214">PlanMyLeave tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="486ab-214">Creating a PlanMyLeave test user</span></span>

<span data-ttu-id="486ab-215">hello ebben a szakaszban célja toocreate PlanMyLeave Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="486ab-215">hello objective of this section is toocreate a user called Britta Simon in PlanMyLeave.</span></span> <span data-ttu-id="486ab-216">PlanMyLeave támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="486ab-216">PlanMyLeave supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="486ab-217">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="486ab-217">There is no action item for you in this section.</span></span> <span data-ttu-id="486ab-218">Új felhasználó létrejön egy kísérlet tooaccess PlanMyLeave során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="486ab-218">A new user will be created during an attempt tooaccess PlanMyLeave if it doesn't exist yet.</span></span>

> [!NOTE]
> <span data-ttu-id="486ab-219">Ha egy felhasználó toocreate manuálisan kell, toocontact kell [PlanMyLeave támogatási csoport](mailto:support@planmyleave.com).</span><span class="sxs-lookup"><span data-stu-id="486ab-219">If you need toocreate an user manually, you need toocontact [PlanMyLeave support team](mailto:support@planmyleave.com).</span></span>



### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="486ab-220">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="486ab-220">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="486ab-221">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooPlanMyLeave megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="486ab-221">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooPlanMyLeave.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="486ab-223">**tooassign Britta Simon tooPlanMyLeave, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="486ab-223">**tooassign Britta Simon tooPlanMyLeave, perform hello following steps:**</span></span>

1. <span data-ttu-id="486ab-224">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="486ab-224">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="486ab-226">Hello alkalmazások listában válassza ki a **PlanMyLeave**.</span><span class="sxs-lookup"><span data-stu-id="486ab-226">In hello applications list, select **PlanMyLeave**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-planmyleave-tutorial/tutorial_planmyleave_50.png) 

3. <span data-ttu-id="486ab-228">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="486ab-228">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="486ab-230">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="486ab-230">Click **Add** button.</span></span> <span data-ttu-id="486ab-231">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="486ab-231">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="486ab-233">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="486ab-233">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="486ab-234">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="486ab-234">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="486ab-235">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="486ab-235">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="486ab-236">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="486ab-236">Testing single sign-on</span></span>

<span data-ttu-id="486ab-237">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="486ab-237">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="486ab-238">Ha a hozzáférési Panel hello hello PlanMyLeave csempe gombra kattint, automatikusan bejelentkezett tooyour PlanMyLeave alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="486ab-238">When you click hello PlanMyLeave tile in hello Access Panel, you should get automatically signed-on tooyour PlanMyLeave application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="486ab-239">További források</span><span class="sxs-lookup"><span data-stu-id="486ab-239">Additional resources</span></span>

* [<span data-ttu-id="486ab-240">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="486ab-240">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="486ab-241">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="486ab-241">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-planmyleave-tutorial/tutorial_general_203.png