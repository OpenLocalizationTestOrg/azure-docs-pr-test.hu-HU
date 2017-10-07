---
title: "Oktatóanyag: Azure Active Directoryval integrált legfontosabb feladatai közé tartoznak OnDemand |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a legfontosabb feladatai közé tartoznak OnDemand között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: f57c5fef-49b0-4591-91ef-fc0de6d654ab
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: jeedes
ms.openlocfilehash: 85fd6eb4e93010d8f7477df236403e205e9f2d83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-cornerstone-ondemand"></a><span data-ttu-id="adcf2-103">Oktatóanyag: Azure Active Directoryval integrált legfontosabb feladatai közé tartoznak kötegmérete</span><span class="sxs-lookup"><span data-stu-id="adcf2-103">Tutorial: Azure Active Directory integration with Cornerstone OnDemand</span></span>

<span data-ttu-id="adcf2-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate legfontosabb feladatai közé tartoznak OnDemand az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="adcf2-104">In this tutorial, you learn how toointegrate Cornerstone OnDemand with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="adcf2-105">Legfontosabb feladatai közé tartoznak OnDemand integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="adcf2-105">Integrating Cornerstone OnDemand with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="adcf2-106">Megadhatja a hozzáférés tooCornerstone OnDemand rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="adcf2-106">You can control in Azure AD who has access tooCornerstone OnDemand</span></span>
- <span data-ttu-id="adcf2-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooCornerstone OnDemand (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="adcf2-107">You can enable your users tooautomatically get signed-on tooCornerstone OnDemand (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="adcf2-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="adcf2-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="adcf2-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="adcf2-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="adcf2-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="adcf2-110">Prerequisites</span></span>

<span data-ttu-id="adcf2-111">tooconfigure OnDemand legfontosabb feladatai közé tartoznak az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="adcf2-111">tooconfigure Azure AD integration with Cornerstone OnDemand, you need hello following items:</span></span>

- <span data-ttu-id="adcf2-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="adcf2-112">An Azure AD subscription</span></span>
- <span data-ttu-id="adcf2-113">A legfontosabb feladatai közé tartoznak OnDemand egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="adcf2-113">A Cornerstone OnDemand single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="adcf2-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="adcf2-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="adcf2-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="adcf2-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="adcf2-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="adcf2-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="adcf2-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="adcf2-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="adcf2-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="adcf2-118">Scenario description</span></span>
<span data-ttu-id="adcf2-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="adcf2-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="adcf2-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="adcf2-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="adcf2-121">Hello gyűjteményből legfontosabb feladatai közé tartoznak OnDemand hozzáadása</span><span class="sxs-lookup"><span data-stu-id="adcf2-121">Adding Cornerstone OnDemand from hello gallery</span></span>
2. <span data-ttu-id="adcf2-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="adcf2-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-cornerstone-ondemand-from-hello-gallery"></a><span data-ttu-id="adcf2-123">Hello gyűjteményből legfontosabb feladatai közé tartoznak OnDemand hozzáadása</span><span class="sxs-lookup"><span data-stu-id="adcf2-123">Adding Cornerstone OnDemand from hello gallery</span></span>
<span data-ttu-id="adcf2-124">tooconfigure hello integrációja OnDemand legfontosabb feladatai közé tartoznak az Azure AD-be, meg kell tooadd legfontosabb feladatai közé tartoznak OnDemand hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="adcf2-124">tooconfigure hello integration of Cornerstone OnDemand into Azure AD, you need tooadd Cornerstone OnDemand from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="adcf2-125">**tooadd legfontosabb feladatai közé tartoznak OnDemand hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="adcf2-125">**tooadd Cornerstone OnDemand from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="adcf2-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="adcf2-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="adcf2-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="adcf2-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="adcf2-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="adcf2-133">Hello keresési mezőbe, írja be a **legfontosabb feladatai közé tartoznak OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-133">In hello search box, type **Cornerstone OnDemand**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_search.png)

5. <span data-ttu-id="adcf2-135">A hello eredmények panelen válassza ki a **legfontosabb feladatai közé tartoznak OnDemand**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-135">In hello results panel, select **Cornerstone OnDemand**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="adcf2-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="adcf2-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="adcf2-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a legfontosabb feladatai közé tartoznak OnDemand "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="adcf2-138">In this section, you configure and test Azure AD single sign-on with Cornerstone OnDemand based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="adcf2-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói OnDemand legfontosabb feladatai közé tartoznak a tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="adcf2-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Cornerstone OnDemand is tooa user in Azure AD.</span></span> <span data-ttu-id="adcf2-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello OnDemand legfontosabb feladatai közé tartoznak a hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="adcf2-140">In other words, a link relationship between an Azure AD user and hello related user in Cornerstone OnDemand needs toobe established.</span></span>

<span data-ttu-id="adcf2-141">A legfontosabb feladatai közé tartoznak OnDemand, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="adcf2-141">In Cornerstone OnDemand, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="adcf2-142">tooconfigure és az Azure AD az egyszeri bejelentkezés legfontosabb feladatai közé tartoznak OnDemand-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="adcf2-142">tooconfigure and test Azure AD single sign-on with Cornerstone OnDemand, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="adcf2-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="adcf2-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="adcf2-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="adcf2-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="adcf2-145">**[A legfontosabb feladatai közé tartoznak OnDemand tesztfelhasználó létrehozása](#creating-a-cornerstone-ondemand-test-user)**  -toohave egy megfelelője a Britta Simon a legfontosabb feladatai közé tartoznak OnDemand, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="adcf2-145">**[Creating a Cornerstone OnDemand test user](#creating-a-cornerstone-ondemand-test-user)** - toohave a counterpart of Britta Simon in Cornerstone OnDemand that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="adcf2-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="adcf2-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="adcf2-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="adcf2-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="adcf2-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="adcf2-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="adcf2-149">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és a legfontosabb feladatai közé tartoznak OnDemand alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="adcf2-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Cornerstone OnDemand application.</span></span>

<span data-ttu-id="adcf2-150">**az Azure AD tooconfigure egyszeri bejelentkezést az OnDemand legfontosabb feladatai közé tartoznak, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="adcf2-150">**tooconfigure Azure AD single sign-on with Cornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="adcf2-151">Az Azure portál, a hello hello **legfontosabb feladatai közé tartoznak OnDemand** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-151">In hello Azure portal, on hello **Cornerstone OnDemand** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="adcf2-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="adcf2-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_samlbase.png)

3. <span data-ttu-id="adcf2-155">A hello **legfontosabb feladatai közé tartoznak OnDemand tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="adcf2-155">On hello **Cornerstone OnDemand Domain and URLs** section, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_url.png)

    <span data-ttu-id="adcf2-157">a.</span><span class="sxs-lookup"><span data-stu-id="adcf2-157">a.</span></span> <span data-ttu-id="adcf2-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="adcf2-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    <span data-ttu-id="adcf2-159">b.</span><span class="sxs-lookup"><span data-stu-id="adcf2-159">b.</span></span> <span data-ttu-id="adcf2-160">A **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.csod.com`</span><span class="sxs-lookup"><span data-stu-id="adcf2-160">In **Identifier** textbox, type a URL using hello following pattern: `https://<company>.csod.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="adcf2-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="adcf2-161">These values are not real.</span></span> <span data-ttu-id="adcf2-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="adcf2-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="adcf2-163">Ügyfél [legfontosabb feladatai közé tartoznak OnDemand ügyfél-támogatási csoport](mailTo:moreinfo@csod.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="adcf2-163">Contact [Cornerstone OnDemand Client support team](mailTo:moreinfo@csod.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="adcf2-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="adcf2-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_certificate.png) 

5. <span data-ttu-id="adcf2-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="adcf2-168">A hello **legfontosabb feladatai közé tartoznak OnDemand konfigurációs** területén kattintson **konfigurálása legfontosabb feladatai közé tartoznak OnDemand** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="adcf2-168">On hello **Cornerstone OnDemand Configuration** section, click **Configure Cornerstone OnDemand** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="adcf2-169">Másolás hello **Sign-Out és SAML-alapú egyszeri bejelentkezés szolgáltatás URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="adcf2-169">Copy hello **Sign-Out URL and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_configure.png) 

7. <span data-ttu-id="adcf2-171">tooconfigure egyszeri bejelentkezést a **legfontosabb feladatai közé tartoznak OnDemand** oldalon kell letöltött toosend hello **tanúsítvány**, **Sign-Out URL-cím** és **SAML egyetlen Bejelentkezési URL-címe** túl[legfontosabb feladatai közé tartoznak OnDemand támogatási csoport](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="adcf2-171">tooconfigure single sign-on on **Cornerstone OnDemand** side, you need toosend hello downloaded **Certificate**, **Sign-Out URL** and **SAML Single Sign-On Service URL**  too[Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span> <span data-ttu-id="adcf2-172">Maguk állítják be ezt a beállítást toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="adcf2-172">They set this setting toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="adcf2-173">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="adcf2-173">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="adcf2-174">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="adcf2-174">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="adcf2-175">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="adcf2-175">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="adcf2-176">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="adcf2-176">Creating an Azure AD test user</span></span>
<span data-ttu-id="adcf2-177">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="adcf2-177">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="adcf2-179">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="adcf2-179">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="adcf2-180">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-180">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="adcf2-182">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-182">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="adcf2-184">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="adcf2-184">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="adcf2-186">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="adcf2-186">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-cornerstone-ondemand-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="adcf2-188">a.</span><span class="sxs-lookup"><span data-stu-id="adcf2-188">a.</span></span> <span data-ttu-id="adcf2-189">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-189">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="adcf2-190">b.</span><span class="sxs-lookup"><span data-stu-id="adcf2-190">b.</span></span> <span data-ttu-id="adcf2-191">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="adcf2-191">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="adcf2-192">c.</span><span class="sxs-lookup"><span data-stu-id="adcf2-192">c.</span></span> <span data-ttu-id="adcf2-193">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-193">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="adcf2-194">d.</span><span class="sxs-lookup"><span data-stu-id="adcf2-194">d.</span></span> <span data-ttu-id="adcf2-195">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-195">Click **Create**.</span></span>
 
### <a name="creating-a-cornerstone-ondemand-test-user"></a><span data-ttu-id="adcf2-196">A legfontosabb feladatai közé tartoznak OnDemand tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="adcf2-196">Creating a Cornerstone OnDemand test user</span></span>

<span data-ttu-id="adcf2-197">Rendelés tooenable az Azure AD felhasználók toolog legfontosabb feladatai közé tartoznak OnDemand be, a azok ki kell építenie az OnDemand legfontosabb feladatai közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="adcf2-197">In order tooenable Azure AD users toolog into Cornerstone OnDemand, they must be provisioned into Cornerstone OnDemand.</span></span> <span data-ttu-id="adcf2-198">Legfontosabb feladatai közé tartoznak OnDemand hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="adcf2-198">In hello case of Cornerstone OnDemand, provisioning is a manual task.</span></span>

<span data-ttu-id="adcf2-199">tooconfigure felhasználók átadásához, a Küldés hello információk (pl.: név, E-mail) hello Azure AD-felhasználó meg kívánja jeleníteni tooprovision toohello [legfontosabb feladatai közé tartoznak OnDemand támogatási csoport](mailTo:moreinfo@csod.com).</span><span class="sxs-lookup"><span data-stu-id="adcf2-199">tooconfigure user provisioning, send hello information (e.g.: Name, Email) about hello Azure AD user you want tooprovision toohello [Cornerstone OnDemand support team](mailTo:moreinfo@csod.com).</span></span>

>[!NOTE]
><span data-ttu-id="adcf2-200">Bármely más legfontosabb feladatai közé tartoznak OnDemand felhasználói fiók létrehozása eszközök vagy legfontosabb feladatai közé tartoznak OnDemand tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="adcf2-200">You can use any other Cornerstone OnDemand user account creation tools or APIs provided by Cornerstone OnDemand tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="adcf2-201">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="adcf2-201">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="adcf2-202">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooCornerstone OnDemand megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="adcf2-202">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooCornerstone OnDemand.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="adcf2-204">**tooassign Britta Simon tooCornerstone OnDemand, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="adcf2-204">**tooassign Britta Simon tooCornerstone OnDemand, perform hello following steps:**</span></span>

1. <span data-ttu-id="adcf2-205">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-205">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="adcf2-207">Hello alkalmazások listában válassza ki a **legfontosabb feladatai közé tartoznak OnDemand**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-207">In hello applications list, select **Cornerstone OnDemand**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_cornerstoneondemand_app.png) 

3. <span data-ttu-id="adcf2-209">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="adcf2-209">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="adcf2-211">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="adcf2-211">Click **Add** button.</span></span> <span data-ttu-id="adcf2-212">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="adcf2-212">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="adcf2-214">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="adcf2-214">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="adcf2-215">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="adcf2-215">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="adcf2-216">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="adcf2-216">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="adcf2-217">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="adcf2-217">Testing single sign-on</span></span>

<span data-ttu-id="adcf2-218">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="adcf2-218">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="adcf2-219">Ha a hozzáférési Panel hello hello legfontosabb feladatai közé tartoznak OnDemand csempe gombra kattint, automatikusan bejelentkezett tooyour legfontosabb feladatai közé tartoznak OnDemand alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="adcf2-219">When you click hello Cornerstone OnDemand tile in hello Access Panel, you should get automatically signed-on tooyour Cornerstone OnDemand application.</span></span>
<span data-ttu-id="adcf2-220">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="adcf2-220">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="adcf2-221">További források</span><span class="sxs-lookup"><span data-stu-id="adcf2-221">Additional resources</span></span>

* [<span data-ttu-id="adcf2-222">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="adcf2-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="adcf2-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="adcf2-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-cornerstone-ondemand-tutorial/tutorial_general_203.png

