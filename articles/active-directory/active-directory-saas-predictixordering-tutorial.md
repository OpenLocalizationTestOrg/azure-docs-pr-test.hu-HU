---
title: "Oktatóanyag: Azure Active Directoryval integrált Predictix rendezés |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a rendezés Predictix között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 2fe2f976-e97f-4368-9695-3e1624409e8b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.openlocfilehash: 0418ef24d7942b6b751c0b4d64e7bd1fba1d6a56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-predictix-ordering"></a><span data-ttu-id="f34b3-103">Oktatóanyag: Azure Active Directoryval integrált Predictix rendezése</span><span class="sxs-lookup"><span data-stu-id="f34b3-103">Tutorial: Azure Active Directory integration with Predictix Ordering</span></span>

<span data-ttu-id="f34b3-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Predictix rendelési az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="f34b3-104">In this tutorial, you learn how toointegrate Predictix Ordering with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="f34b3-105">Predictix rendelési integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="f34b3-105">Integrating Predictix Ordering with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="f34b3-106">Szabályozhatja az Azure AD hozzáférési tooPredictix rendelkező rendezés.</span><span class="sxs-lookup"><span data-stu-id="f34b3-106">You can control in Azure AD who has access tooPredictix Ordering.</span></span>
- <span data-ttu-id="f34b3-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooPredictix rendezés (egyszeri bejelentkezés) a saját Azure AD-fiókok.</span><span class="sxs-lookup"><span data-stu-id="f34b3-107">You can enable your users tooautomatically get signed-on tooPredictix Ordering (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="f34b3-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="f34b3-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="f34b3-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="f34b3-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f34b3-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f34b3-110">Prerequisites</span></span>

<span data-ttu-id="f34b3-111">tooconfigure Predictix rendelési integrálása az Azure AD, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="f34b3-111">tooconfigure Azure AD integration with Predictix Ordering, you need hello following items:</span></span>

- <span data-ttu-id="f34b3-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="f34b3-112">An Azure AD subscription</span></span>
- <span data-ttu-id="f34b3-113">Egy Predictix rendelési egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="f34b3-113">A Predictix Ordering single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="f34b3-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="f34b3-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="f34b3-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="f34b3-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="f34b3-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="f34b3-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="f34b3-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="f34b3-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="f34b3-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="f34b3-118">Scenario description</span></span>
<span data-ttu-id="f34b3-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="f34b3-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="f34b3-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="f34b3-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="f34b3-121">Hozzáadás a Predictix rendelési hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f34b3-121">Adding Predictix Ordering from hello gallery</span></span>
2. <span data-ttu-id="f34b3-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="f34b3-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-predictix-ordering-from-hello-gallery"></a><span data-ttu-id="f34b3-123">Hozzáadás a Predictix rendelési hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="f34b3-123">Adding Predictix Ordering from hello gallery</span></span>
<span data-ttu-id="f34b3-124">tooconfigure hello integrációja Predictix rendelési az Azure AD-be, meg kell tooadd Predictix rendelési hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="f34b3-124">tooconfigure hello integration of Predictix Ordering into Azure AD, you need tooadd Predictix Ordering from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="f34b3-125">**tooadd Predictix rendelési hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f34b3-125">**tooadd Predictix Ordering from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="f34b3-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="f34b3-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="f34b3-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="f34b3-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="f34b3-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="f34b3-133">Hello keresési mezőbe, írja be a **Predictix rendelési**, jelölje be **Predictix rendelési** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-133">In hello search box, type **Predictix Ordering**, select **Predictix Ordering** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Predictix hello eredménylistában rendezése](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="f34b3-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f34b3-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="f34b3-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Predictix rendelési "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="f34b3-136">In this section, you configure and test Azure AD single sign-on with Predictix Ordering based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="f34b3-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Predictix sorrendben tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="f34b3-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Predictix Ordering is tooa user in Azure AD.</span></span> <span data-ttu-id="f34b3-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Predictix sorrendben közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="f34b3-138">In other words, a link relationship between an Azure AD user and hello related user in Predictix Ordering needs toobe established.</span></span>

<span data-ttu-id="f34b3-139">Rendezés Predictix, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="f34b3-139">In Predictix Ordering, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="f34b3-140">tooconfigure és az Azure AD az egyszeri bejelentkezés teszthez Predictix rendezés, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="f34b3-140">tooconfigure and test Azure AD single sign-on with Predictix Ordering, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="f34b3-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="f34b3-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="f34b3-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="f34b3-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="f34b3-143">**[Predictix rendelési tesztfelhasználó létrehozása](#create-a-predictix-ordering-test-user)**  -toohave egy megfelelője a Britta Simon sorrendben Predictix, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="f34b3-143">**[Create a Predictix Ordering test user](#create-a-predictix-ordering-test-user)** - toohave a counterpart of Britta Simon in Predictix Ordering that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="f34b3-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f34b3-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="f34b3-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="f34b3-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="f34b3-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f34b3-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="f34b3-147">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az Predictix rendelési alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f34b3-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Predictix Ordering application.</span></span>

<span data-ttu-id="f34b3-148">**az Azure AD tooconfigure egyszeri bejelentkezés Predictix rendezés használatakor a hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="f34b3-148">**tooconfigure Azure AD single sign-on with Predictix Ordering, perform hello following steps:**</span></span>

1. <span data-ttu-id="f34b3-149">Az Azure portál, a hello hello **Predictix rendelési** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-149">In hello Azure portal, on hello **Predictix Ordering** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="f34b3-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="f34b3-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_samlbase.png)

3. <span data-ttu-id="f34b3-153">A hello **Predictix rendelési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f34b3-153">On hello **Predictix Ordering Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információkat Predictix rendelési tartomány és az URL-címek](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_url.png)

    <span data-ttu-id="f34b3-155">a.</span><span class="sxs-lookup"><span data-stu-id="f34b3-155">a.</span></span> <span data-ttu-id="f34b3-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<companyname-pricing>.ordering.predictix.com/sso/request`</span><span class="sxs-lookup"><span data-stu-id="f34b3-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<companyname-pricing>.ordering.predictix.com/sso/request`</span></span>

    <span data-ttu-id="f34b3-157">b.</span><span class="sxs-lookup"><span data-stu-id="f34b3-157">b.</span></span> <span data-ttu-id="f34b3-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="f34b3-158">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span> 
    | |
    |--|
    | `https://<companyname-pricing>.dev.ordering.predictix.com` |
    | `https://<companyname-pricing>.ordering.predictix.com` |

    > [!NOTE] 
    > <span data-ttu-id="f34b3-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="f34b3-159">These values are not real.</span></span> <span data-ttu-id="f34b3-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="f34b3-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="f34b3-161">Ügyfél [Predictix rendelési ügyfél-támogatási csoport](https://www.predix.io/support/) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="f34b3-161">Contact [Predictix Ordering Client support team](https://www.predix.io/support/) tooget these values.</span></span> 
 
4. <span data-ttu-id="f34b3-162">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f34b3-162">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_certificate.png) 

5. <span data-ttu-id="f34b3-164">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-164">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-predictixordering-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="f34b3-166">A hello **Predictix rendelési konfigurációs** területén kattintson **Predictix rendelési konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="f34b3-166">On hello **Predictix Ordering Configuration** section, click **Configure Predictix Ordering** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="f34b3-167">Másolás hello **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="f34b3-167">Copy hello **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Predictix rendelési konfiguráció](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_configure.png) 

7. <span data-ttu-id="f34b3-169">tooconfigure egyszeri bejelentkezést a **Predictix rendelési** oldalon kell letöltött toosend hello **tanúsítvány (Base64)**, **Sign-Out URL-címet, a SAML entitás azonosítója és a SAML-alapú egyszeri bejelentkezési URL-címe**  túl[terméktámogatási csapathoz rendelési Predictix](https://www.predix.io/support/).</span><span class="sxs-lookup"><span data-stu-id="f34b3-169">tooconfigure single sign-on on **Predictix Ordering** side, you need toosend hello downloaded **Certificate (Base64)**, **Sign-Out URL, SAML Entity ID, and SAML Single Sign-On Service URL** too[Predictix Ordering support team](https://www.predix.io/support/).</span></span> <span data-ttu-id="f34b3-170">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="f34b3-170">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="f34b3-171">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="f34b3-171">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="f34b3-172">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="f34b3-172">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="f34b3-173">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="f34b3-173">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="f34b3-174">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="f34b3-174">Create an Azure AD test user</span></span>
<span data-ttu-id="f34b3-175">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="f34b3-175">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="f34b3-177">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="f34b3-177">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="f34b3-178">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-178">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="f34b3-180">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-180">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="f34b3-182">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="f34b3-182">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="f34b3-184">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="f34b3-184">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-predictixordering-tutorial/create_aaduser_04.png)

   <span data-ttu-id="f34b3-186">a.</span><span class="sxs-lookup"><span data-stu-id="f34b3-186">a.</span></span> <span data-ttu-id="f34b3-187">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-187">In hello **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="f34b3-188">b.</span><span class="sxs-lookup"><span data-stu-id="f34b3-188">b.</span></span> <span data-ttu-id="f34b3-189">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="f34b3-189">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

   <span data-ttu-id="f34b3-190">c.</span><span class="sxs-lookup"><span data-stu-id="f34b3-190">c.</span></span> <span data-ttu-id="f34b3-191">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="f34b3-191">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

   <span data-ttu-id="f34b3-192">d.</span><span class="sxs-lookup"><span data-stu-id="f34b3-192">d.</span></span> <span data-ttu-id="f34b3-193">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-193">Click **Create**.</span></span>
 
### <a name="create-a-predictix-ordering-test-user"></a><span data-ttu-id="f34b3-194">Predictix rendelési tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="f34b3-194">Create a Predictix Ordering test user</span></span>

<span data-ttu-id="f34b3-195">Ebben a szakaszban Britta Simon meghívta Predictix rendelési felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f34b3-195">In this section, you create a user called Britta Simon in Predictix Ordering.</span></span> <span data-ttu-id="f34b3-196">Együttműködve [terméktámogatási csapathoz rendelési Predictix](https://www.predix.io/support/) tooadd hello felhasználók hello Predictix rendelési platform.</span><span class="sxs-lookup"><span data-stu-id="f34b3-196">Work with [Predictix Ordering support team](https://www.predix.io/support/) tooadd hello users in hello Predictix Ordering platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="f34b3-197">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="f34b3-197">Assign hello Azure AD test user</span></span>

<span data-ttu-id="f34b3-198">Ebben a szakaszban hozzáférés tooPredictix megadásával engedélyeznie Britta Simon toouse Azure egyszeri bejelentkezés rendezés.</span><span class="sxs-lookup"><span data-stu-id="f34b3-198">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooPredictix Ordering.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="f34b3-200">**tooassign Britta Simon tooPredictix rendezés, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="f34b3-200">**tooassign Britta Simon tooPredictix Ordering, perform hello following steps:**</span></span>

1. <span data-ttu-id="f34b3-201">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-201">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="f34b3-203">Hello alkalmazások listában válassza ki a **Predictix rendelési**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-203">In hello applications list, select **Predictix Ordering**.</span></span>

    ![hello Predictix rendelési hivatkozásra hello alkalmazások listája](./media/active-directory-saas-predictixordering-tutorial/tutorial_predictixordering_app.png)  

3. <span data-ttu-id="f34b3-205">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="f34b3-205">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="f34b3-207">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="f34b3-207">Click **Add** button.</span></span> <span data-ttu-id="f34b3-208">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f34b3-208">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="f34b3-210">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="f34b3-210">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="f34b3-211">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f34b3-211">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="f34b3-212">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="f34b3-212">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="f34b3-213">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="f34b3-213">Test single sign-on</span></span>

<span data-ttu-id="f34b3-214">hello ebben a szakaszban célja tootest az egyszeri bejelentkezés konfigurációs használatával hello a hozzáférési Panel.</span><span class="sxs-lookup"><span data-stu-id="f34b3-214">hello objective of this section is tootest your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="f34b3-215">Hello Predictix rendelési csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour Predictix rendelési alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="f34b3-215">When you click hello Predictix Ordering tile in hello Access Panel, you should get automatically signed-on tooyour Predictix Ordering application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="f34b3-216">További források</span><span class="sxs-lookup"><span data-stu-id="f34b3-216">Additional resources</span></span>

* [<span data-ttu-id="f34b3-217">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="f34b3-217">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="f34b3-218">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="f34b3-218">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-predictixordering-tutorial/tutorial_general_203.png

