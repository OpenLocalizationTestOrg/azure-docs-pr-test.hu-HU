---
title: "Oktatóanyag: Azure Active Directoryval integrált Insperity ExpensAble |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a Insperity ExpensAble között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c579c453-580e-417d-8a5e-9b6b352795c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: jeedes
ms.openlocfilehash: 204d5435c6ba1f23bb98701381e165a9a66add62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-insperity-expensable"></a><span data-ttu-id="492cf-103">Oktatóanyag: Azure Active Directoryval integrált Insperity ExpensAble</span><span class="sxs-lookup"><span data-stu-id="492cf-103">Tutorial: Azure Active Directory integration with Insperity ExpensAble</span></span>

<span data-ttu-id="492cf-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Insperity ExpensAble az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="492cf-104">In this tutorial, you learn how toointegrate Insperity ExpensAble with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="492cf-105">Insperity ExpensAble integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="492cf-105">Integrating Insperity ExpensAble with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="492cf-106">Megadhatja a hozzáférés tooInsperity ExpensAble rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="492cf-106">You can control in Azure AD who has access tooInsperity ExpensAble</span></span>
- <span data-ttu-id="492cf-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooInsperity ExpensAble (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="492cf-107">You can enable your users tooautomatically get signed-on tooInsperity ExpensAble (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="492cf-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="492cf-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="492cf-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="492cf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="492cf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="492cf-110">Prerequisites</span></span>

<span data-ttu-id="492cf-111">tooconfigure Insperity ExpensAble az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="492cf-111">tooconfigure Azure AD integration with Insperity ExpensAble, you need hello following items:</span></span>

- <span data-ttu-id="492cf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="492cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="492cf-113">Egy Insperity ExpensAble egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="492cf-113">An Insperity ExpensAble single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="492cf-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="492cf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="492cf-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="492cf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="492cf-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="492cf-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="492cf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="492cf-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="492cf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="492cf-118">Scenario description</span></span>
<span data-ttu-id="492cf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="492cf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="492cf-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="492cf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="492cf-121">Hello gyűjteményből Insperity ExpensAble hozzáadása</span><span class="sxs-lookup"><span data-stu-id="492cf-121">Adding Insperity ExpensAble from hello gallery</span></span>
2. <span data-ttu-id="492cf-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="492cf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-insperity-expensable-from-hello-gallery"></a><span data-ttu-id="492cf-123">Hello gyűjteményből Insperity ExpensAble hozzáadása</span><span class="sxs-lookup"><span data-stu-id="492cf-123">Adding Insperity ExpensAble from hello gallery</span></span>
<span data-ttu-id="492cf-124">tooconfigure hello integrációja Insperity ExpensAble az Azure AD-be, meg kell tooadd Insperity ExpensAble hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="492cf-124">tooconfigure hello integration of Insperity ExpensAble into Azure AD, you need tooadd Insperity ExpensAble from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="492cf-125">**hello gyűjteményből ExpensAble Insperity tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="492cf-125">**tooadd Insperity ExpensAble from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="492cf-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="492cf-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="492cf-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="492cf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="492cf-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="492cf-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="492cf-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="492cf-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="492cf-133">Hello keresési mezőbe, írja be a **Insperity ExpensAble**.</span><span class="sxs-lookup"><span data-stu-id="492cf-133">In hello search box, type **Insperity ExpensAble**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_search.png)

5. <span data-ttu-id="492cf-135">A hello eredmények panelen válassza ki a **Insperity ExpensAble**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="492cf-135">In hello results panel, select **Insperity ExpensAble**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="492cf-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="492cf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="492cf-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Insperity ExpensAble "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="492cf-138">In this section, you configure and test Azure AD single sign-on with Insperity ExpensAble based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="492cf-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó Insperity ExpensAble tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="492cf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Insperity ExpensAble is tooa user in Azure AD.</span></span> <span data-ttu-id="492cf-140">Ez azt jelenti hello kapcsolódó felhasználó a Insperity ExpensAble és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="492cf-140">In other words, a link relationship between an Azure AD user and hello related user in Insperity ExpensAble needs toobe established.</span></span>

<span data-ttu-id="492cf-141">A Insperity ExpensAble rendelje hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="492cf-141">In Insperity ExpensAble, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="492cf-142">tooconfigure és -teszthez az Azure AD az egyszeri bejelentkezés Insperity ExpensAble, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="492cf-142">tooconfigure and test Azure AD single sign-on with Insperity ExpensAble, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="492cf-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="492cf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="492cf-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="492cf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="492cf-145">**[Egy Insperity ExpensAble tesztfelhasználó létrehozása](#creating-an-insperity-expensable-test-user)**  -toohave egy megfelelője a Britta Simon a Insperity ExpensAble csatolt toohello az Azure AD felhasználói ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="492cf-145">**[Creating an Insperity ExpensAble test user](#creating-an-insperity-expensable-test-user)** - toohave a counterpart of Britta Simon in Insperity ExpensAble that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="492cf-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="492cf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="492cf-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="492cf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="492cf-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="492cf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="492cf-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Insperity ExpensAble alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="492cf-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Insperity ExpensAble application.</span></span>

<span data-ttu-id="492cf-150">**tooconfigure az Azure AD egyszeri bejelentkezést a Insperity ExpensAble, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="492cf-150">**tooconfigure Azure AD single sign-on with Insperity ExpensAble, perform hello following steps:**</span></span>

1. <span data-ttu-id="492cf-151">Az Azure portál, a hello hello **Insperity ExpensAble** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="492cf-151">In hello Azure portal, on hello **Insperity ExpensAble** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="492cf-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="492cf-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_samlbase.png)

3. <span data-ttu-id="492cf-155">A hello **Insperity ExpensAble tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="492cf-155">On hello **Insperity ExpensAble Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_url.png)

    <span data-ttu-id="492cf-157">a.</span><span class="sxs-lookup"><span data-stu-id="492cf-157">a.</span></span> <span data-ttu-id="492cf-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`</span><span class="sxs-lookup"><span data-stu-id="492cf-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://server.expensable.com/esapp/Authenticate?companyId=<company ID>`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="492cf-159">Ez az érték nincs valós.</span><span class="sxs-lookup"><span data-stu-id="492cf-159">This value is not real.</span></span> <span data-ttu-id="492cf-160">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="492cf-160">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="492cf-161">Ügyfél [Insperity ExpensAble ügyfél-támogatási csoport](http://expensable.com/support/support-overview) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="492cf-161">Contact [Insperity ExpensAble Client support team](http://expensable.com/support/support-overview) tooget this value.</span></span> 
 
4. <span data-ttu-id="492cf-162">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="492cf-162">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_certificate.png) 

5. <span data-ttu-id="492cf-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="492cf-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="492cf-166">A hello **Insperity ExpensAble konfigurációs** kattintson **Insperity ExpensAble konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="492cf-166">On hello **Insperity ExpensAble Configuration** section, click **Configure Insperity ExpensAble** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="492cf-167">Másolás hello **SAML Entitásazonosító és SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="492cf-167">Copy hello **SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_configure.png) 

7. <span data-ttu-id="492cf-169">tooconfigure egyszeri bejelentkezést a **Insperity ExpensAble** oldalon kell letöltött toosend hello **metaadatainak XML-kódja**, **SAML-alapú egyszeri bejelentkezési URL-címe** és **SAML Entitásazonosító** túl[Insperity ExpensAble támogatási csoport](http://expensable.com/support/support-overview).</span><span class="sxs-lookup"><span data-stu-id="492cf-169">tooconfigure single sign-on on **Insperity ExpensAble** side, you need toosend hello downloaded **Metadata XML**, **SAML Single Sign-On Service URL** and **SAML Entity ID** too[Insperity ExpensAble support team](http://expensable.com/support/support-overview).</span></span> <span data-ttu-id="492cf-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="492cf-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="492cf-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="492cf-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="492cf-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="492cf-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="492cf-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="492cf-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="492cf-174">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="492cf-174">Creating an Azure AD test user</span></span>
<span data-ttu-id="492cf-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="492cf-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="492cf-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="492cf-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="492cf-178">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="492cf-178">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="492cf-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="492cf-180">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="492cf-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="492cf-182">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="492cf-184">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="492cf-184">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-insperityexpensable-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="492cf-186">a.</span><span class="sxs-lookup"><span data-stu-id="492cf-186">a.</span></span> <span data-ttu-id="492cf-187">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="492cf-187">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="492cf-188">b.</span><span class="sxs-lookup"><span data-stu-id="492cf-188">b.</span></span> <span data-ttu-id="492cf-189">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="492cf-189">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="492cf-190">c.</span><span class="sxs-lookup"><span data-stu-id="492cf-190">c.</span></span> <span data-ttu-id="492cf-191">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="492cf-191">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="492cf-192">d.</span><span class="sxs-lookup"><span data-stu-id="492cf-192">d.</span></span> <span data-ttu-id="492cf-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="492cf-193">Click **Create**.</span></span>
 
### <a name="creating-an-insperity-expensable-test-user"></a><span data-ttu-id="492cf-194">Egy Insperity ExpensAble tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="492cf-194">Creating an Insperity ExpensAble test user</span></span>

<span data-ttu-id="492cf-195">hello ebben a szakaszban célja toocreate Britta Simon Insperity ExpensAble nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="492cf-195">hello objective of this section is toocreate a user called Britta Simon in Insperity ExpensAble.</span></span> <span data-ttu-id="492cf-196">Adjon együttműködve [Insperity ExpensAble támogatási csoport](http://expensable.com/support/support-overview) tooadd hello felhasználók hello Insperity ExpensAble fiók.</span><span class="sxs-lookup"><span data-stu-id="492cf-196">Please work with [Insperity ExpensAble support team](http://expensable.com/support/support-overview) tooadd hello users in hello Insperity ExpensAble account.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="492cf-197">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="492cf-197">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="492cf-198">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooInsperity ExpensAble megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="492cf-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooInsperity ExpensAble.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="492cf-200">**tooassign Britta Simon tooInsperity ExpensAble, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="492cf-200">**tooassign Britta Simon tooInsperity ExpensAble, perform hello following steps:**</span></span>

1. <span data-ttu-id="492cf-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="492cf-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="492cf-203">Hello alkalmazások listában válassza ki a **Insperity ExpensAble**.</span><span class="sxs-lookup"><span data-stu-id="492cf-203">In hello applications list, select **Insperity ExpensAble**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-insperityexpensable-tutorial/tutorial_insperityexpensable_app.png) 

3. <span data-ttu-id="492cf-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="492cf-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="492cf-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="492cf-207">Click **Add** button.</span></span> <span data-ttu-id="492cf-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="492cf-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="492cf-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="492cf-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="492cf-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="492cf-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="492cf-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="492cf-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="492cf-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="492cf-213">Testing single sign-on</span></span>

<span data-ttu-id="492cf-214">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="492cf-214">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="492cf-215">Hello Insperity ExpensAble csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Insperity ExpensAble alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="492cf-215">When you click hello Insperity ExpensAble tile in hello Access Panel, you should get automatically signed-on tooyour Insperity ExpensAble application.</span></span>
<span data-ttu-id="492cf-216">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="492cf-216">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="492cf-217">További források</span><span class="sxs-lookup"><span data-stu-id="492cf-217">Additional resources</span></span>

* [<span data-ttu-id="492cf-218">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="492cf-218">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="492cf-219">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="492cf-219">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_04.png
[100]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_100.png
[200]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-insperityexpensable-tutorial/tutorial_general_203.png

