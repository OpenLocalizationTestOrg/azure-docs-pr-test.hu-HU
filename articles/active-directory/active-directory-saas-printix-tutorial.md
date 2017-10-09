---
title: "Oktatóanyag: Azure Active Directoryval integrált Printix |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Printix között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 4aea7320-b2d5-49e0-9b63-aeaff0f6fe66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: jeedes
ms.openlocfilehash: 654810116091eb52912b377cc97afef803ee816e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-printix"></a><span data-ttu-id="f984d-103">Oktatóanyag: Azure Active Directoryval integrált Printix</span><span class="sxs-lookup"><span data-stu-id="f984d-103">Tutorial: Azure Active Directory integration with Printix</span></span>

<span data-ttu-id="f984d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Printix az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f984d-104">In this tutorial, you learn how toointegrate Printix with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f984d-105">Printix integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f984d-105">Integrating Printix with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f984d-106">Megadhatja a hozzáférés tooPrintix rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="f984d-106">You can control in Azure AD who has access tooPrintix</span></span>
- <span data-ttu-id="f984d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPrintix (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="f984d-107">You can enable your users tooautomatically get signed-on tooPrintix (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="f984d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f984d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="f984d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f984d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f984d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f984d-110">Prerequisites</span></span>

<span data-ttu-id="f984d-111">az Azure AD integrálása Printix tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="f984d-111">tooconfigure Azure AD integration with Printix, you need hello following items:</span></span>

- <span data-ttu-id="f984d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f984d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f984d-113">Egy Printix egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f984d-113">A Printix single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f984d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f984d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f984d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f984d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f984d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f984d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f984d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f984d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f984d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f984d-118">Scenario description</span></span>
<span data-ttu-id="f984d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f984d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f984d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f984d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f984d-121">Hello gyűjteményből Printix hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f984d-121">Adding Printix from hello gallery</span></span>
2. <span data-ttu-id="f984d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f984d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-printix-from-hello-gallery"></a><span data-ttu-id="f984d-123">Hello gyűjteményből Printix hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f984d-123">Adding Printix from hello gallery</span></span>
<span data-ttu-id="f984d-124">tooconfigure hello integrációja Printix az Azure AD-be, meg kell tooadd Printix hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f984d-124">tooconfigure hello integration of Printix into Azure AD, you need tooadd Printix from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f984d-125">**tooadd Printix hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f984d-125">**tooadd Printix from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f984d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f984d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="f984d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f984d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f984d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f984d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="f984d-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f984d-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="f984d-133">Hello keresési mezőbe, írja be a **Printix**.</span><span class="sxs-lookup"><span data-stu-id="f984d-133">In hello search box, type **Printix**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/tutorial_printix_search.png)

5. <span data-ttu-id="f984d-135">A hello eredmények panelen válassza ki a **Printix**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-135">In hello results panel, select **Printix**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/tutorial_printix_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="f984d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f984d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="f984d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján Printix.</span><span class="sxs-lookup"><span data-stu-id="f984d-138">In this section, you configure and test Azure AD single sign-on with Printix based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f984d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Printix tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f984d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Printix is tooa user in Azure AD.</span></span> <span data-ttu-id="f984d-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Printix közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="f984d-140">In other words, a link relationship between an Azure AD user and hello related user in Printix needs toobe established.</span></span>

<span data-ttu-id="f984d-141">Printix, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f984d-141">In Printix, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f984d-142">tooconfigure és az Azure AD az egyszeri bejelentkezés Printix-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f984d-142">tooconfigure and test Azure AD single sign-on with Printix, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f984d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f984d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f984d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f984d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f984d-145">**[Printix tesztfelhasználó létrehozása](#creating-a-printix-test-user)**  -toohave egy megfelelője a Britta Simon a Printix, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f984d-145">**[Creating a Printix test user](#creating-a-printix-test-user)** - toohave a counterpart of Britta Simon in Printix that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f984d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f984d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f984d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f984d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="f984d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f984d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="f984d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Printix alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f984d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Printix application.</span></span>

<span data-ttu-id="f984d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a Printix, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f984d-150">**tooconfigure Azure AD single sign-on with Printix, perform hello following steps:**</span></span>

1. <span data-ttu-id="f984d-151">Az Azure portál, a hello hello **Printix** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f984d-151">In hello Azure portal, on hello **Printix** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="f984d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f984d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_samlbase.png)

3. <span data-ttu-id="f984d-155">A hello **Printix tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f984d-155">On hello **Printix Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_url.png)

    <span data-ttu-id="f984d-157">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<subdomain>.printix.net`</span><span class="sxs-lookup"><span data-stu-id="f984d-157">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<subdomain>.printix.net`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="f984d-158">hello érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="f984d-158">hello value is not real.</span></span> <span data-ttu-id="f984d-159">Frissítés hello értékének hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f984d-159">Update hello value with hello actual Sign-On URL.</span></span> <span data-ttu-id="f984d-160">Ügyfél [Printix ügyfél-támogatási csoport](mailto:support@printix.net) tooget hello érték.</span><span class="sxs-lookup"><span data-stu-id="f984d-160">Contact [Printix Client support team](mailto:support@printix.net) tooget hello value.</span></span> 
 
4. <span data-ttu-id="f984d-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f984d-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_certificate.png) 

5. <span data-ttu-id="f984d-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f984d-165">Bejelentkezés tooyour Printix Bérlői rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="f984d-165">Sign-on tooyour Printix tenant as an administrator.</span></span>

7. <span data-ttu-id="f984d-166">Hello hello felső menüben kattintson hello hello felső, jobb alsó sarkában, és válasszon "**hitelesítési**".</span><span class="sxs-lookup"><span data-stu-id="f984d-166">In hello menu on hello top, click hello icon at hello upper right corner and select "**Authentication**".</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_06.png)

8. <span data-ttu-id="f984d-168">A hello **telepítő** lapon jelölje be **engedélyezése Azure vagy Office 365-hitelesítés**</span><span class="sxs-lookup"><span data-stu-id="f984d-168">On hello **Setup** tab, select **Enable Azure/Office 365 authentication**</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_07.png)

9. <span data-ttu-id="f984d-170">A hello **Azure** lapon bemeneti összevonási metaadatainak URL-cím toohello szövegmező az "**összevonási metaadat-dokumentum**".</span><span class="sxs-lookup"><span data-stu-id="f984d-170">On hello **Azure** tab, input federation metadata URL toohello textbox of "**Federation metadata document**".</span></span> 

    <span data-ttu-id="f984d-171">Hello metaadatok XML-fájl, amely túl letöltötte az Azure AD csatolása[Printix támogatási csoport](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="f984d-171">Attach hello metadata xml file which you downloaded from Azure AD too[Printix support team](mailto:support@printix.net).</span></span> <span data-ttu-id="f984d-172">Ezután hello XML-fájl feltöltése, és adjon meg egy összevonási metaadatainak URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="f984d-172">Then they upload hello xml file and provide a federation metadata URL.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_08.png)
   
10. <span data-ttu-id="f984d-174">Kattintson a hello "**tesztelése**"gombra, majd kattintson a"**OK**" gombra, ha hello teszt sikeres volt.</span><span class="sxs-lookup"><span data-stu-id="f984d-174">Click hello "**Test**" button and click "**OK**" button if hello test was successful.</span></span>
   
     <span data-ttu-id="f984d-175">Az Azure active directory lapon megjelenik a hello kattintás után **tesztelése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-175">Azure active directory page will show after clicking hello **test** button.</span></span> <span data-ttu-id="f984d-176">"a hello teszt sikerült" Itt azt jelenti, hogy ez jelenik az Azure vizsgálati fiókja a hello hitelesítő adatok megadása után üzenet "beállítások tesztelt OK". Kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-176">"hello test was successful" here means after entering hello credentials of your Azure test account it will pop up a message "Settings tested OK".Then click hello **OK** button.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_09.png)

11. <span data-ttu-id="f984d-178">Hello kattintson **mentése** gombra kattint, a "**hitelesítési**" lapon.</span><span class="sxs-lookup"><span data-stu-id="f984d-178">Click hello **Save** button on "**Authentication**" page.</span></span>


> [!TIP]
> <span data-ttu-id="f984d-179">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f984d-179">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f984d-180">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f984d-180">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f984d-181">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f984d-181">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="f984d-182">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f984d-182">Creating an Azure AD test user</span></span>
<span data-ttu-id="f984d-183">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f984d-183">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="f984d-185">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f984d-185">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f984d-186">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f984d-186">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="f984d-188">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f984d-188">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="f984d-190">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f984d-190">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="f984d-192">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f984d-192">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-printix-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="f984d-194">a.</span><span class="sxs-lookup"><span data-stu-id="f984d-194">a.</span></span> <span data-ttu-id="f984d-195">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f984d-195">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="f984d-196">b.</span><span class="sxs-lookup"><span data-stu-id="f984d-196">b.</span></span> <span data-ttu-id="f984d-197">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="f984d-197">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="f984d-198">c.</span><span class="sxs-lookup"><span data-stu-id="f984d-198">c.</span></span> <span data-ttu-id="f984d-199">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="f984d-199">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="f984d-200">d.</span><span class="sxs-lookup"><span data-stu-id="f984d-200">d.</span></span> <span data-ttu-id="f984d-201">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-201">Click **Create**.</span></span>
 
### <a name="creating-a-printix-test-user"></a><span data-ttu-id="f984d-202">Printix tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f984d-202">Creating a Printix test user</span></span>

<span data-ttu-id="f984d-203">hello ebben a szakaszban célja toocreate Printix Britta Simon nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="f984d-203">hello objective of this section is toocreate a user called Britta Simon in Printix.</span></span> <span data-ttu-id="f984d-204">Printix támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="f984d-204">Printix supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="f984d-205">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="f984d-205">There is no action item for you in this section.</span></span> <span data-ttu-id="f984d-206">Új felhasználó jön létre egy kísérlet tooaccess Printix során, ha még nem létezik.</span><span class="sxs-lookup"><span data-stu-id="f984d-206">A new user is created during an attempt tooaccess Printix if it doesn't exist yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="f984d-207">A felhasználó toocreate manuálisan kell, ha szüksége van-e toocontact hello [Printix támogatási csoport](mailto:support@printix.net).</span><span class="sxs-lookup"><span data-stu-id="f984d-207">If you need toocreate a user manually, you need toocontact hello [Printix support team](mailto:support@printix.net).</span></span>
> 

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="f984d-208">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f984d-208">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="f984d-209">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooPrintix megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="f984d-209">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPrintix.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="f984d-211">**tooassign Britta Simon tooPrintix, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f984d-211">**tooassign Britta Simon tooPrintix, perform hello following steps:**</span></span>

1. <span data-ttu-id="f984d-212">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f984d-212">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f984d-214">Hello alkalmazások listában válassza ki a **Printix**.</span><span class="sxs-lookup"><span data-stu-id="f984d-214">In hello applications list, select **Printix**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-printix-tutorial/tutorial_printix_app.png) 

3. <span data-ttu-id="f984d-216">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f984d-216">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="f984d-218">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f984d-218">Click **Add** button.</span></span> <span data-ttu-id="f984d-219">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f984d-219">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="f984d-221">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f984d-221">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f984d-222">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f984d-222">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f984d-223">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f984d-223">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="f984d-224">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f984d-224">Testing single sign-on</span></span>

<span data-ttu-id="f984d-225">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="f984d-225">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f984d-226">Ha a hozzáférési Panel hello hello Printix csempe gombra kattint, automatikusan bejelentkezett tooyour Printix alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f984d-226">When you click hello Printix tile in hello Access Panel, you should get automatically signed-on tooyour Printix application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f984d-227">További források</span><span class="sxs-lookup"><span data-stu-id="f984d-227">Additional resources</span></span>

* [<span data-ttu-id="f984d-228">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f984d-228">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f984d-229">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f984d-229">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-printix-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-printix-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-printix-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-printix-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-printix-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-printix-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-printix-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-printix-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-printix-tutorial/tutorial_general_203.png

