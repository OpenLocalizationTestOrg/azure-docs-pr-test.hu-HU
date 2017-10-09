---
title: "Oktatóanyag: Azure Active Directoryval integrált Mixpanel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Mixpanel között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a2df26ef-d441-44ac-a9f3-b37bf9709bcb
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 8da8aaefee3558c37babe3e10aeba4224ceffa16
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-mixpanel"></a><span data-ttu-id="d8194-103">Oktatóanyag: Azure Active Directoryval integrált Mixpanel</span><span class="sxs-lookup"><span data-stu-id="d8194-103">Tutorial: Azure Active Directory integration with Mixpanel</span></span>

<span data-ttu-id="d8194-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Mixpanel az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d8194-104">In this tutorial, you learn how toointegrate Mixpanel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d8194-105">Mixpanel integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d8194-105">Integrating Mixpanel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d8194-106">Megadhatja a hozzáférés tooMixpanel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d8194-106">You can control in Azure AD who has access tooMixpanel</span></span>
- <span data-ttu-id="d8194-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooMixpanel (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d8194-107">You can enable your users tooautomatically get signed-on tooMixpanel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d8194-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d8194-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d8194-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d8194-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d8194-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d8194-110">Prerequisites</span></span>

<span data-ttu-id="d8194-111">az Azure AD integrálása Mixpanel tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d8194-111">tooconfigure Azure AD integration with Mixpanel, you need hello following items:</span></span>

- <span data-ttu-id="d8194-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d8194-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d8194-113">Egy Mixpanel egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d8194-113">A Mixpanel single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d8194-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d8194-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d8194-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d8194-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d8194-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d8194-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d8194-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d8194-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d8194-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d8194-118">Scenario description</span></span>
<span data-ttu-id="d8194-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d8194-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d8194-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d8194-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d8194-121">Hello gyűjteményből Mixpanel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d8194-121">Adding Mixpanel from hello gallery</span></span>
2. <span data-ttu-id="d8194-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d8194-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-mixpanel-from-hello-gallery"></a><span data-ttu-id="d8194-123">Hello gyűjteményből Mixpanel hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d8194-123">Adding Mixpanel from hello gallery</span></span>
<span data-ttu-id="d8194-124">tooconfigure hello integrációja Mixpanel az Azure AD-be, meg kell tooadd Mixpanel hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d8194-124">tooconfigure hello integration of Mixpanel into Azure AD, you need tooadd Mixpanel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d8194-125">**tooadd Mixpanel hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d8194-125">**tooadd Mixpanel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8194-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d8194-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d8194-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d8194-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d8194-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d8194-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d8194-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d8194-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d8194-133">Hello keresési mezőbe, írja be a **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="d8194-133">In hello search box, type **Mixpanel**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_search.png)

5. <span data-ttu-id="d8194-135">A hello eredmények panelen válassza ki a **Mixpanel**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d8194-135">In hello results panel, select **Mixpanel**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d8194-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d8194-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d8194-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Mixpanel.</span><span class="sxs-lookup"><span data-stu-id="d8194-138">In this section, you configure and test Azure AD single sign-on with Mixpanel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d8194-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Mixpanel tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d8194-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Mixpanel is tooa user in Azure AD.</span></span> <span data-ttu-id="d8194-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Mixpanel közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d8194-140">In other words, a link relationship between an Azure AD user and hello related user in Mixpanel needs toobe established.</span></span>

<span data-ttu-id="d8194-141">Mixpanel, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d8194-141">In Mixpanel, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d8194-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Mixpanel-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d8194-142">tooconfigure and test Azure AD single sign-on with Mixpanel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d8194-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d8194-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d8194-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d8194-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d8194-145">**[Mixpanel tesztfelhasználó létrehozása](#creating-a-mixpanel-test-user)**  -toohave egy megfelelője a Britta Simon a Mixpanel, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d8194-145">**[Creating a Mixpanel test user](#creating-a-mixpanel-test-user)** - toohave a counterpart of Britta Simon in Mixpanel that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d8194-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d8194-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d8194-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d8194-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d8194-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d8194-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d8194-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Mixpanel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d8194-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Mixpanel application.</span></span>

<span data-ttu-id="d8194-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Mixpanel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d8194-150">**tooconfigure Azure AD single sign-on with Mixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8194-151">Az Azure portál, a hello hello **Mixpanel** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d8194-151">In hello Azure portal, on hello **Mixpanel** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d8194-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d8194-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_samlbase.png)

3. <span data-ttu-id="d8194-155">A hello **Mixpanel tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d8194-155">On hello **Mixpanel Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_url.png)

     <span data-ttu-id="d8194-157">A hello **bejelentkezési URL-cím** szövegmező, adja meg az URL-címet:`https://mixpanel.com/login/`</span><span class="sxs-lookup"><span data-stu-id="d8194-157">In hello **Sign-on URL** textbox, type a URL as: `https://mixpanel.com/login/`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d8194-158">Regisztrálja a [https://mixpanel.com/register/](https://mixpanel.com/register/) a bejelentkezési hitelesítő adatok és a kapcsolattartási hello tooset [Mixpanel támogatási csoport](mailto:support@mixpanel.com) tooenable egyszeri bejelentkezési beállítások a bérlő számára.</span><span class="sxs-lookup"><span data-stu-id="d8194-158">Please register at [https://mixpanel.com/register/](https://mixpanel.com/register/) tooset up your login credentials and  contact hello [Mixpanel support team](mailto:support@mixpanel.com) tooenable SSO settings for your tenant.</span></span> <span data-ttu-id="d8194-159">Is letölthető az URL-cím bejelentkezési érték szükség esetén a Mixpanel támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="d8194-159">You can also get your Sign On URL value if necessary from your Mixpanel support team.</span></span> 
 
4. <span data-ttu-id="d8194-160">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d8194-160">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_certificate.png) 

5. <span data-ttu-id="d8194-162">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d8194-162">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d8194-164">A hello **Mixpanel konfigurációs** kattintson **konfigurálása Mixpanel** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d8194-164">On hello **Mixpanel Configuration** section, click **Configure Mixpanel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d8194-165">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d8194-165">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_configure.png) 

7. <span data-ttu-id="d8194-167">Egy másik böngészőablakban, bejelentkezés tooyour Mixpanel alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d8194-167">In a different browser window, sign-on tooyour Mixpanel application as an administrator.</span></span>

8. <span data-ttu-id="d8194-168">A hello lap alján, kattintson hello kissé **áttételi** hello bal oldali sarokban látható ikonra.</span><span class="sxs-lookup"><span data-stu-id="d8194-168">On bottom of hello page, click hello little **gear** icon in hello left corner.</span></span> 
   
    ![Mixpanel egyszeri bejelentkezés](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_06.png) 

9. <span data-ttu-id="d8194-170">Kattintson a hello **biztonságos hozzáférését** fülre, majd **beállításainak módosítása**.</span><span class="sxs-lookup"><span data-stu-id="d8194-170">Click hello **Access security** tab, and then click **Change settings**.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_08.png) 

10. <span data-ttu-id="d8194-172">A hello **módosítania kell a tanúsítványt** párbeszédpanel lap, kattintson a **fájl kiválasztása** tooupload a letöltött tanúsítvány, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="d8194-172">On hello **Change your certificate** dialog page, click **Choose file** tooupload your downloaded certificate, and then click **NEXT**.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_09.png) 

11.  <span data-ttu-id="d8194-174">Szövegmezőjének hello hitelesítési URL-címet a hello **módosítsa a hitelesítési URL-címet** párbeszédpanel lap, Beillesztés hello értékének **SAML-alapú egyszeri bejelentkezési URL-címe** Azure-portálon másolta, és kattintson a **Következő**.</span><span class="sxs-lookup"><span data-stu-id="d8194-174">In hello authentication URL textbox on hello **Change your authentication  URL** dialog page, paste hello value of **SAML Single Sign-On Service URL** which you have copied from Azure portal, and then click **NEXT**.</span></span>
   
   ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_10.png) 

12. <span data-ttu-id="d8194-176">Kattintson a **Done** (Kész) gombra.</span><span class="sxs-lookup"><span data-stu-id="d8194-176">Click **Done**.</span></span>

> [!TIP]
> <span data-ttu-id="d8194-177">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d8194-177">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d8194-178">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d8194-178">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d8194-179">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d8194-179">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d8194-180">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8194-180">Creating an Azure AD test user</span></span>
<span data-ttu-id="d8194-181">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d8194-181">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d8194-183">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d8194-183">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8194-184">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d8194-184">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d8194-186">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d8194-186">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d8194-188">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8194-188">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d8194-190">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d8194-190">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-mixpanel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d8194-192">a.</span><span class="sxs-lookup"><span data-stu-id="d8194-192">a.</span></span> <span data-ttu-id="d8194-193">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d8194-193">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d8194-194">b.</span><span class="sxs-lookup"><span data-stu-id="d8194-194">b.</span></span> <span data-ttu-id="d8194-195">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d8194-195">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d8194-196">c.</span><span class="sxs-lookup"><span data-stu-id="d8194-196">c.</span></span> <span data-ttu-id="d8194-197">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d8194-197">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d8194-198">d.</span><span class="sxs-lookup"><span data-stu-id="d8194-198">d.</span></span> <span data-ttu-id="d8194-199">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d8194-199">Click **Create**.</span></span>
 
### <a name="creating-a-mixpanel-test-user"></a><span data-ttu-id="d8194-200">Mixpanel tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d8194-200">Creating a Mixpanel test user</span></span>

<span data-ttu-id="d8194-201">hello ebben a szakaszban célja toocreate Mixpanel Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="d8194-201">hello objective of this section is toocreate a user called Britta Simon in Mixpanel.</span></span> 

1. <span data-ttu-id="d8194-202">Bejelentkezés tooyour Mixpanel vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d8194-202">Sign on tooyour Mixpanel company site as an administrator.</span></span>

2. <span data-ttu-id="d8194-203">A hello a hello lap alján, kattintson a kevés fogaskerék gombra a hello bal sarok tooopen hello hello **beállítások** ablak.</span><span class="sxs-lookup"><span data-stu-id="d8194-203">On hello bottom of hello page, click hello little gear button on hello left corner tooopen hello **Settings** window.</span></span>

3. <span data-ttu-id="d8194-204">Kattintson a hello **Team** fülre.</span><span class="sxs-lookup"><span data-stu-id="d8194-204">Click hello **Team** tab.</span></span>

4. <span data-ttu-id="d8194-205">A hello **csapattag** szövegmezőhöz hello Azure adja meg Britta tartozó e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="d8194-205">In hello **team member** textbox, type Britta's email address in hello Azure.</span></span>
   
    ![Mixpanel beállítások](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_11.png) 

5. <span data-ttu-id="d8194-207">Kattintson a **meghívása**.</span><span class="sxs-lookup"><span data-stu-id="d8194-207">Click **Invite**.</span></span> 

> [!Note]
> <span data-ttu-id="d8194-208">hello felhasználó kap egy e-mailek tooset hello-profil mentése.</span><span class="sxs-lookup"><span data-stu-id="d8194-208">hello user will get an email tooset up hello profile.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d8194-209">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d8194-209">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d8194-210">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooMixpanel megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d8194-210">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooMixpanel.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d8194-212">**tooassign Britta Simon tooMixpanel, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d8194-212">**tooassign Britta Simon tooMixpanel, perform hello following steps:**</span></span>

1. <span data-ttu-id="d8194-213">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d8194-213">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d8194-215">Hello alkalmazások listában válassza ki a **Mixpanel**.</span><span class="sxs-lookup"><span data-stu-id="d8194-215">In hello applications list, select **Mixpanel**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-mixpanel-tutorial/tutorial_mixpanel_app.png) 

3. <span data-ttu-id="d8194-217">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d8194-217">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d8194-219">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d8194-219">Click **Add** button.</span></span> <span data-ttu-id="d8194-220">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8194-220">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d8194-222">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d8194-222">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d8194-223">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8194-223">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d8194-224">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d8194-224">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d8194-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d8194-225">Testing single sign-on</span></span>

<span data-ttu-id="d8194-226">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d8194-226">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d8194-227">Ha a hozzáférési Panel hello hello Mixpanel csempe gombra kattint, automatikusan bejelentkezett tooyour Mixpanel alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d8194-227">When you click hello Mixpanel tile in hello Access Panel, you should get automatically signed-on tooyour Mixpanel application.</span></span>
<span data-ttu-id="d8194-228">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d8194-228">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8194-229">További források</span><span class="sxs-lookup"><span data-stu-id="d8194-229">Additional resources</span></span>

* [<span data-ttu-id="d8194-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d8194-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d8194-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d8194-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-mixpanel-tutorial/tutorial_general_203.png

