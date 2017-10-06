---
title: "Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA) |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Zscaler személyes hozzáférési (ZPA) között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 83711115-1c4f-4dd7-907b-3da24b37c89e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: jeedes
ms.openlocfilehash: 0370cff60c8ac15bd1919acccc924da1e50dc45b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-private-access-zpa"></a><span data-ttu-id="5d642-103">Oktatóanyag: Azure Active Directory-integrációval rendelkező Zscaler személyes hozzáférési (ZPA)</span><span class="sxs-lookup"><span data-stu-id="5d642-103">Tutorial: Azure Active Directory integration with Zscaler Private Access (ZPA)</span></span>

<span data-ttu-id="5d642-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Zscaler személyes hozzáférési (ZPA) az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="5d642-104">In this tutorial, you learn how toointegrate Zscaler Private Access (ZPA) with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5d642-105">Zscaler személyes hozzáférési (ZPA) integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="5d642-105">Integrating Zscaler Private Access (ZPA) with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="5d642-106">Megadhatja a hozzáférés tooZscaler titkos (ZPA) rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5d642-106">You can control in Azure AD who has access tooZscaler Private Access (ZPA)</span></span>
- <span data-ttu-id="5d642-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooZscaler titkos a hozzáférési (ZPA) a (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="5d642-107">You can enable your users tooautomatically get signed-on tooZscaler Private Access (ZPA) (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="5d642-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="5d642-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="5d642-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5d642-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5d642-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5d642-110">Prerequisites</span></span>

<span data-ttu-id="5d642-111">tooconfigure az Azure AD-integrációs rendelkező Zscaler személyes hozzáférési (ZPA), a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="5d642-111">tooconfigure Azure AD integration with Zscaler Private Access (ZPA), you need hello following items:</span></span>

- <span data-ttu-id="5d642-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5d642-112">An Azure AD subscription</span></span>
- <span data-ttu-id="5d642-113">Egy Zscaler személyes hozzáférési (ZPA) egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="5d642-113">A Zscaler Private Access (ZPA) single-sign on enabled subscription</span></span>


> [!NOTE]
> <span data-ttu-id="5d642-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="5d642-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>


<span data-ttu-id="5d642-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="5d642-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="5d642-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5d642-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="5d642-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5d642-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="5d642-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5d642-118">Scenario description</span></span>
<span data-ttu-id="5d642-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5d642-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="5d642-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5d642-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5d642-121">Hello gyűjteményből Zscaler személyes hozzáférési (ZPA) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5d642-121">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
2. <span data-ttu-id="5d642-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d642-122">Configuring and testing Azure AD single sign-on</span></span>


## <a name="adding-zscaler-private-access-zpa-from-hello-gallery"></a><span data-ttu-id="5d642-123">Hello gyűjteményből Zscaler személyes hozzáférési (ZPA) hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5d642-123">Adding Zscaler Private Access (ZPA) from hello gallery</span></span>
<span data-ttu-id="5d642-124">tooconfigure hello integráció a Zscaler személyes hozzáférési (ZPA) az Azure AD-be, meg kell tooadd Zscaler személyes hozzáférési (ZPA) hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="5d642-124">tooconfigure hello integration of Zscaler Private Access (ZPA) into Azure AD, you need tooadd Zscaler Private Access (ZPA) from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="5d642-125">**tooadd Zscaler személyes hozzáférési (ZPA) hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="5d642-125">**tooadd Zscaler Private Access (ZPA) from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d642-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d642-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="5d642-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="5d642-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="5d642-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d642-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="5d642-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="5d642-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="5d642-133">Hello keresési mezőbe, írja be a **Zscaler személyes hozzáférési (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="5d642-133">In hello search box, type **Zscaler Private Access (ZPA)**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_001.png)

5. <span data-ttu-id="5d642-135">A hello eredmények panelen válassza a **Zscaler személyes hozzáférési (ZPA)**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-135">In hello results panel, select **Zscaler Private Access (ZPA)**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_0001.png)


##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="5d642-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="5d642-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="5d642-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA) "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="5d642-138">In this section, you configure and test Azure AD single sign-on with Zscaler Private Access (ZPA) based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5d642-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói a Zscaler személyes hozzáférési (ZPA) tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5d642-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Zscaler Private Access (ZPA) is tooa user in Azure AD.</span></span> <span data-ttu-id="5d642-140">Ez azt jelenti hello kapcsolódó felhasználó a Zscaler személyes hozzáférési (ZPA) és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="5d642-140">In other words, a link relationship between an Azure AD user and hello related user in Zscaler Private Access (ZPA) needs toobe established.</span></span>

<span data-ttu-id="5d642-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a Zscaler személyes hozzáférési (ZPA).</span><span class="sxs-lookup"><span data-stu-id="5d642-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Zscaler Private Access (ZPA).</span></span>

<span data-ttu-id="5d642-142">tooconfigure és az Azure AD az egyszeri bejelentkezés tesztelése a Zscaler személyes hozzáférési (ZPA), a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="5d642-142">tooconfigure and test Azure AD single sign-on with Zscaler Private Access (ZPA), you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="5d642-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5d642-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="5d642-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="5d642-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="5d642-145">**[Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása](#creating-a-zscaler-private-access-(zpa)-test-user)**  -toohave Britta Simon a Zscaler személyes hozzáférési (ZPA), amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="5d642-145">**[Creating a Zscaler Private Access (ZPA) test user](#creating-a-zscaler-private-access-(zpa)-test-user)** - toohave a counterpart of Britta Simon in Zscaler Private Access (ZPA) that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="5d642-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="5d642-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="5d642-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="5d642-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="5d642-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5d642-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="5d642-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az Zscaler személyes hozzáférési (ZPA) alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5d642-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your Zscaler Private Access (ZPA) application.</span></span>

<span data-ttu-id="5d642-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Zscaler személyes hozzáférési (ZPA), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5d642-150">**tooconfigure Azure AD single sign-on with Zscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="5d642-151">Hello Azure felügyeleti portálon, a hello **Zscaler személyes hozzáférési (ZPA)** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="5d642-151">In hello Azure Management portal, on hello **Zscaler Private Access (ZPA)** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="5d642-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="5d642-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_300.png)
    
3. <span data-ttu-id="5d642-155">A hello **Zscaler személyes hozzáférési (ZPA) tartományhoz és URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d642-155">On hello **Zscaler Private Access (ZPA) Domain and URLs** section, perform hello following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_01.png)

    <span data-ttu-id="5d642-157">a.</span><span class="sxs-lookup"><span data-stu-id="5d642-157">a.</span></span> <span data-ttu-id="5d642-158">A hello **URL-cím bejelentkezési** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span><span class="sxs-lookup"><span data-stu-id="5d642-158">In hello **Sign On URL** textbox, type a URL using hello following pattern: `https://samlsp.private.zscaler.com/auth/login?domain=<your-domain-name>`</span></span>

    <span data-ttu-id="5d642-159">b.</span><span class="sxs-lookup"><span data-stu-id="5d642-159">b.</span></span> <span data-ttu-id="5d642-160">A hello **azonosító** szövegmező, típus:`https://samlsp.private.zscaler.com/auth/metadata`</span><span class="sxs-lookup"><span data-stu-id="5d642-160">In hello **Identifier** textbox, type: `https://samlsp.private.zscaler.com/auth/metadata`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="5d642-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="5d642-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="5d642-162">Ezeket az értékeket a tényleges hello bejelentkezési URL-cím és azonosító tooupdate rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="5d642-162">You have tooupdate these values with hello actual Sign On URL and Identifier.</span></span> <span data-ttu-id="5d642-163">Itt javasoljuk, hogy toouse hello egyedi értéket URL-címének hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5d642-163">Here we suggest you toouse hello unique value of URL in hello Identifier.</span></span> <span data-ttu-id="5d642-164">Ügyfél [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="5d642-164">Contact [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooget these values.</span></span>

4. <span data-ttu-id="5d642-165">A hello **SAML-aláíró tanúsítványa** kattintson **hozzon létre új tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="5d642-165">On hello **SAML Signing Certificate** section, click **Create new certificate**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_400.png)   

5. <span data-ttu-id="5d642-167">A hello **új tanúsítvány létrehozása** párbeszédpanelen hello naptár ikonra, és válassza ki az **lejárati dátum**.</span><span class="sxs-lookup"><span data-stu-id="5d642-167">On hello **Create New Certificate** dialog, click hello calendar icon and select an **expiry date**.</span></span> <span data-ttu-id="5d642-168">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-168">Then click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_500.png)

6. <span data-ttu-id="5d642-170">A hello **SAML-aláíró tanúsítványa** szakaszban jelölje be **új tanúsítvány aktiválásához** kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-170">On hello **SAML Signing Certificate** section, select **Make new certificate active** and click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_02.png)

7. <span data-ttu-id="5d642-172">A hello előugró ablak **helyettesítő tanúsítvány** ablak, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="5d642-172">On hello pop-up **Rollover certificate** window, click **OK**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_600.png)

8. <span data-ttu-id="5d642-174">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5d642-174">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_03.png) 

9. <span data-ttu-id="5d642-176">Egy másik webes böngészőablakban jelentkezzen be a Zscaler személyes hozzáférési (ZPA) vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="5d642-176">In a different web browser window, log into your Zscaler Private Access (ZPA) company site as an administrator.</span></span>

10. <span data-ttu-id="5d642-177">Keresse meg a túl**rendszergazda** majd **Idp konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="5d642-177">Navigate too**Administrator** and then click **Idp Configuration**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_04.png)

11. <span data-ttu-id="5d642-179">A hello **Idp konfigurációs** kattintson **hozzáadása új IDP konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="5d642-179">In hello **Idp Configuration** section, click **Add New IDP Configuration**.</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_05.png)

12. <span data-ttu-id="5d642-181">A hello **új IDP konfigurációs** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d642-181">In hello **New IDP Configuration** section, perform hello following steps:</span></span>

    ![Alkalmazás ügyféloldali egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_06.png)

    <span data-ttu-id="5d642-183">a.</span><span class="sxs-lookup"><span data-stu-id="5d642-183">a.</span></span> <span data-ttu-id="5d642-184">Kattintson a **fájl kiválasztása** , és töltse fel a letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="5d642-184">Click **Select File** and upload your downloaded metadata file.</span></span>

    <span data-ttu-id="5d642-185">b.</span><span class="sxs-lookup"><span data-stu-id="5d642-185">b.</span></span> <span data-ttu-id="5d642-186">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-186">Click **Save** button.</span></span>
    


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="5d642-187">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d642-187">Creating an Azure AD test user</span></span>
<span data-ttu-id="5d642-188">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="5d642-188">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="5d642-190">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="5d642-190">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="5d642-191">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="5d642-191">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="5d642-193">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="5d642-193">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="5d642-195">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d642-195">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="5d642-197">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="5d642-197">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-zscalerprivateaccess-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="5d642-199">a.</span><span class="sxs-lookup"><span data-stu-id="5d642-199">a.</span></span> <span data-ttu-id="5d642-200">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5d642-200">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="5d642-201">b.</span><span class="sxs-lookup"><span data-stu-id="5d642-201">b.</span></span> <span data-ttu-id="5d642-202">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="5d642-202">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="5d642-203">c.</span><span class="sxs-lookup"><span data-stu-id="5d642-203">c.</span></span> <span data-ttu-id="5d642-204">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5d642-204">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="5d642-205">d.</span><span class="sxs-lookup"><span data-stu-id="5d642-205">d.</span></span> <span data-ttu-id="5d642-206">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-206">Click **Create**.</span></span> 



### <a name="creating-a-zscaler-private-access-zpa-test-user"></a><span data-ttu-id="5d642-207">Zscaler személyes hozzáférési (ZPA) tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5d642-207">Creating a Zscaler Private Access (ZPA) test user</span></span>

<span data-ttu-id="5d642-208">Ebben a szakaszban egy Britta Simon a Zscaler személyes hozzáférési (ZPA) nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5d642-208">In this section, you create a user called Britta Simon in Zscaler Private Access (ZPA).</span></span> <span data-ttu-id="5d642-209">Adjon együttműködve [Zscaler személyes hozzáférési (ZPA) támogatási csoport](https://help.zscaler.com/zpa-submit-ticket) tooadd hello felhasználók hello Zscaler személyes hozzáférési (ZPA) platform.</span><span class="sxs-lookup"><span data-stu-id="5d642-209">Please work with [Zscaler Private Access (ZPA) support team](https://help.zscaler.com/zpa-submit-ticket) tooadd hello users in hello Zscaler Private Access (ZPA) platform.</span></span>


### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="5d642-210">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="5d642-210">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="5d642-211">Ebben a szakaszban engedélyezése Britta Simon toouse Azure egyszeri bejelentkezéshez használt hozzáférés tooZscaler titkos (ZPA) megadása.</span><span class="sxs-lookup"><span data-stu-id="5d642-211">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooZscaler Private Access (ZPA).</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="5d642-213">**tooassign Britta Simon tooZscaler személyes hozzáférési (ZPA), hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="5d642-213">**tooassign Britta Simon tooZscaler Private Access (ZPA), perform hello following steps:**</span></span>

1. <span data-ttu-id="5d642-214">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5d642-214">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="5d642-216">Hello alkalmazások listában válassza ki a **Zscaler személyes hozzáférési (ZPA)**.</span><span class="sxs-lookup"><span data-stu-id="5d642-216">In hello applications list, select **Zscaler Private Access (ZPA)**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_zscalerprivateaccess_50.png) 

3. <span data-ttu-id="5d642-218">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="5d642-218">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="5d642-220">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="5d642-220">Click **Add** button.</span></span> <span data-ttu-id="5d642-221">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d642-221">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="5d642-223">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="5d642-223">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="5d642-224">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d642-224">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="5d642-225">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5d642-225">Click **Assign** button on **Add Assignment** dialog.</span></span>
    


### <a name="testing-single-sign-on"></a><span data-ttu-id="5d642-226">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5d642-226">Testing single sign-on</span></span>

<span data-ttu-id="5d642-227">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="5d642-227">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="5d642-228">A hozzáférési Panel hello hello Zscaler személyes hozzáférési (ZPA) csempére kattintva kapja meg automatikusan bejelentkezett tooyour Zscaler személyes hozzáférési (ZPA) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="5d642-228">When you click hello Zscaler Private Access (ZPA) tile in hello Access Panel, you should get automatically signed-on tooyour Zscaler Private Access (ZPA) application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="5d642-229">További források</span><span class="sxs-lookup"><span data-stu-id="5d642-229">Additional resources</span></span>

* [<span data-ttu-id="5d642-230">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="5d642-230">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5d642-231">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5d642-231">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-zscalerprivateaccess-tutorial/tutorial_general_203.png