---
title: "Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az Online ArcGIS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: a9e132a4-29e7-48bf-beb9-4148e617c8a9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: jeedes
ms.openlocfilehash: f3dd55d798cf3256fb2758e011f33946baa405ce
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-arcgis-online"></a><span data-ttu-id="8bf4d-103">Oktatóanyag: Azure Active Directory Online szolgáltatással való integráció ArcGIS</span><span class="sxs-lookup"><span data-stu-id="8bf4d-103">Tutorial: Azure Active Directory integration with ArcGIS Online</span></span>

<span data-ttu-id="8bf4d-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ArcGIS kapcsolódik az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="8bf4d-104">In this tutorial, you learn how toointegrate ArcGIS Online with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="8bf4d-105">ArcGIS Online integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-105">Integrating ArcGIS Online with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="8bf4d-106">Megadhatja a hozzáférés tooArcGIS Online rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="8bf4d-106">You can control in Azure AD who has access tooArcGIS Online</span></span>
- <span data-ttu-id="8bf4d-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooArcGIS Online (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="8bf4d-107">You can enable your users tooautomatically get signed-on tooArcGIS Online (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="8bf4d-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="8bf4d-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="8bf4d-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="8bf4d-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with ArcGIS Online, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in ArcGIS Online.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="8bf4d-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8bf4d-110">Prerequisites</span></span>

<span data-ttu-id="8bf4d-111">tooconfigure ArcGIS Online az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-111">tooconfigure Azure AD integration with ArcGIS Online, you need hello following items:</span></span>

- <span data-ttu-id="8bf4d-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="8bf4d-112">An Azure AD subscription</span></span>
- <span data-ttu-id="8bf4d-113">Egy ArcGIS Online egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="8bf4d-113">A ArcGIS Online single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="8bf4d-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="8bf4d-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="8bf4d-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="8bf4d-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8bf4d-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="8bf4d-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-118">Scenario description</span></span>
<span data-ttu-id="8bf4d-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="8bf4d-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="8bf4d-121">Hello gyűjteményből ArcGIS Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-121">Adding ArcGIS Online from hello gallery</span></span>
2. <span data-ttu-id="8bf4d-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8bf4d-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-arcgis-online-from-hello-gallery"></a><span data-ttu-id="8bf4d-123">Hello gyűjteményből ArcGIS Online hozzáadása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-123">Adding ArcGIS Online from hello gallery</span></span>
<span data-ttu-id="8bf4d-124">tooconfigure hello integrációs ArcGIS online az Azure AD-be, meg kell tooadd ArcGIS Online hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-124">tooconfigure hello integration of ArcGIS Online into Azure AD, you need tooadd ArcGIS Online from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="8bf4d-125">**hello gyűjteményből Online ArcGIS tooadd hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8bf4d-125">**tooadd ArcGIS Online from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf4d-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="8bf4d-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="8bf4d-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="8bf4d-131">Kattintson a **új alkalmazás** hello felül hello párbeszédpanel tooadd új alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="8bf4d-133">Hello keresési mezőbe, írja be a **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-133">In hello search box, type **ArcGIS Online**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_search.png)

5. <span data-ttu-id="8bf4d-135">A hello eredmények panelen válassza ki a **ArcGIS Online**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-135">In hello results panel, select **ArcGIS Online**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="8bf4d-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="8bf4d-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="8bf4d-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés ArcGIS Online "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="8bf4d-138">In this section, you configure and test Azure AD single sign-on with ArcGIS Online based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="8bf4d-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói ArcGIS online tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ArcGIS Online is tooa user in Azure AD.</span></span> <span data-ttu-id="8bf4d-140">Ez azt jelenti hello kapcsolódó felhasználói ArcGIS online és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-140">In other words, a link relationship between an Azure AD user and hello related user in ArcGIS Online needs toobe established.</span></span>

<span data-ttu-id="8bf4d-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** ArcGIS Online-ban.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ArcGIS Online.</span></span>

<span data-ttu-id="8bf4d-142">tooconfigure és a ArcGIS Online az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-142">tooconfigure and test Azure AD single sign-on with ArcGIS Online, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="8bf4d-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="8bf4d-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="8bf4d-145">**[Egy ArcGIS Online tesztfelhasználó létrehozása](#creating-an-arcgis-online-test-user)**  -toohave egy megfelelője a Britta Simon ArcGIS Online felhasználói ábrázolása csatolt toohello az Azure AD-ban.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-145">**[Creating an ArcGIS Online test user](#creating-an-arcgis-online-test-user)** - toohave a counterpart of Britta Simon in ArcGIS Online that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="8bf4d-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="8bf4d-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="8bf4d-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="8bf4d-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az ArcGIS Online alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your ArcGIS Online application.</span></span>

<span data-ttu-id="8bf4d-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ArcGIS Online, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="8bf4d-150">**tooconfigure Azure AD single sign-on with ArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf4d-151">Az Azure portál, a hello hello **ArcGIS Online** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-151">In hello Azure portal, on hello **ArcGIS Online** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="8bf4d-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_samlbase.png)

3. <span data-ttu-id="8bf4d-155">A hello **ArcGIS Online tartomány és az URL-címek** csoportjában hajtsa végre a következő lépés hello:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-155">On hello **ArcGIS Online Domain and URLs** section, perform hello following step:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_url.png)

    <span data-ttu-id="8bf4d-157">A hello **bejelentkezési URL-cím** szövegmezőhöz típusú hello érték a következő mintát hello használata:`https://<company>.maps.arcgis.com`</span><span class="sxs-lookup"><span data-stu-id="8bf4d-157">In hello **Sign-on URL** textbox, type hello value using hello following pattern: `https://<company>.maps.arcgis.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="8bf4d-158">Ez az érték nincs valós hello.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-158">This value is not hello real.</span></span> <span data-ttu-id="8bf4d-159">Frissítse ezt az értéket hello tényleges bejelentkezési URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-159">Update this value with hello actual Sign-On URL.</span></span> <span data-ttu-id="8bf4d-160">Ügyfél [ArcGIS Online ügyfél-támogatási csoport](http://support.esri.com/) tooget ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-160">Contact [ArcGIS Online Client support team](http://support.esri.com/) tooget this value.</span></span> 

4. <span data-ttu-id="8bf4d-161">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-161">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_certificate.png) 

5. <span data-ttu-id="8bf4d-163">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-163">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="8bf4d-165">Egy másik webes böngészőablakban jelentkezzen be a ArcGIS vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-165">In a different web browser window, log into your ArcGIS company site as an administrator.</span></span>

7. <span data-ttu-id="8bf4d-166">Kattintson a **beállítások szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-166">Click **EDIT SETTINGS**.</span></span>

    <span data-ttu-id="8bf4d-167">![Beállítások szerkesztése](./media/active-directory-saas-arcgis-tutorial/ic784742.png "beállításainak szerkesztése")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-167">![Edit Settings](./media/active-directory-saas-arcgis-tutorial/ic784742.png "Edit Settings")</span></span>

8. <span data-ttu-id="8bf4d-168">Kattintson a **biztonsági**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-168">Click **Security**.</span></span>

    <span data-ttu-id="8bf4d-169">![Biztonsági](./media/active-directory-saas-arcgis-tutorial/ic784743.png "biztonsági")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-169">![Security](./media/active-directory-saas-arcgis-tutorial/ic784743.png "Security")</span></span>

9. <span data-ttu-id="8bf4d-170">A **vállalati bejelentkezések**, kattintson a **IDENTITÁSSZOLGÁLTATÓ beállítása**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-170">Under **Enterprise Logins**, click **SET IDENTITY PROVIDER**.</span></span>

    <span data-ttu-id="8bf4d-171">![Vállalati bejelentkezések](./media/active-directory-saas-arcgis-tutorial/ic784744.png "vállalati bejelentkezések")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-171">![Enterprise Logins](./media/active-directory-saas-arcgis-tutorial/ic784744.png "Enterprise Logins")</span></span>

10. <span data-ttu-id="8bf4d-172">A hello **identitásszolgáltató beállítása** konfiguráció lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-172">On hello **Set Identity Provider** configuration page, perform hello following steps:</span></span>
   
    <span data-ttu-id="8bf4d-173">![Identitásszolgáltató beállítása](./media/active-directory-saas-arcgis-tutorial/ic784745.png "identitásszolgáltató beállítása")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-173">![Set Identity Provider](./media/active-directory-saas-arcgis-tutorial/ic784745.png "Set Identity Provider")</span></span>
   
    <span data-ttu-id="8bf4d-174">a.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-174">a.</span></span> <span data-ttu-id="8bf4d-175">A hello **neve** szövegmező, írja be a szervezet nevét.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-175">In hello **Name** textbox, type your organization’s name.</span></span>

    <span data-ttu-id="8bf4d-176">b.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-176">b.</span></span> <span data-ttu-id="8bf4d-177">A **használatával hello vállalati identitásszolgáltató tartozó metaadatok lesznek megadott**, jelölje be **A fájl**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-177">For **Metadata for hello Enterprise Identity Provider will be supplied using**, select **A File**.</span></span>

    <span data-ttu-id="8bf4d-178">c.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-178">c.</span></span> <span data-ttu-id="8bf4d-179">tooupload a letöltött metaadatfájl, kattintson a **fájl kiválasztása**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-179">tooupload your downloaded metadata file, click **Choose file**.</span></span>

    <span data-ttu-id="8bf4d-180">d.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-180">d.</span></span> <span data-ttu-id="8bf4d-181">Kattintson a **SET IDENTITÁSSZOLGÁLTATÓ**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-181">Click **SET IDENTITY PROVIDER**.</span></span>

> [!TIP]
> <span data-ttu-id="8bf4d-182">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="8bf4d-182">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="8bf4d-183">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-183">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="8bf4d-184">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="8bf4d-184">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="8bf4d-185">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-185">Creating an Azure AD test user</span></span>
<span data-ttu-id="8bf4d-186">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-186">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="8bf4d-188">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="8bf4d-188">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf4d-189">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-189">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="8bf4d-191">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-191">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="8bf4d-193">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-193">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="8bf4d-195">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-195">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-arcgis-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="8bf4d-197">a.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-197">a.</span></span> <span data-ttu-id="8bf4d-198">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-198">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="8bf4d-199">b.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-199">b.</span></span> <span data-ttu-id="8bf4d-200">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-200">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="8bf4d-201">c.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-201">c.</span></span> <span data-ttu-id="8bf4d-202">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-202">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="8bf4d-203">d.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-203">d.</span></span> <span data-ttu-id="8bf4d-204">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-204">Click **Create**.</span></span>
 
### <a name="creating-an-arcgis-online-test-user"></a><span data-ttu-id="8bf4d-205">Egy ArcGIS Online tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="8bf4d-205">Creating an ArcGIS Online test user</span></span>

<span data-ttu-id="8bf4d-206">A sorrend tooenable az Azure AD felhasználók toolog történő ArcGIS Online azok kell üzembe ArcGIS Online be.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-206">In order tooenable Azure AD users toolog into ArcGIS Online, they must be provisioned into ArcGIS Online.</span></span>  
<span data-ttu-id="8bf4d-207">Az ArcGIS Online hello esetben kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-207">In hello case of ArcGIS Online, provisioning is a manual task.</span></span>

<span data-ttu-id="8bf4d-208">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8bf4d-208">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf4d-209">Jelentkezzen be tooyour **ArcGIS** bérlő.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-209">Log in tooyour **ArcGIS** tenant.</span></span>

2. <span data-ttu-id="8bf4d-210">Kattintson a **tagok MEGHÍVÁSA**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-210">Click **INVITE MEMBERS**.</span></span>
   
    <span data-ttu-id="8bf4d-211">![Tagok meghívása](./media/active-directory-saas-arcgis-tutorial/ic784747.png "tagok meghívása")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-211">![Invite Members](./media/active-directory-saas-arcgis-tutorial/ic784747.png "Invite Members")</span></span>

3. <span data-ttu-id="8bf4d-212">Válassza ki **tagok automatikus hozzáadása nélkül e-mail**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-212">Select **Add members automatically without sending an email**, and then click **NEXT**.</span></span>
   
    <span data-ttu-id="8bf4d-213">![Tagok hozzáadása automatikusan](./media/active-directory-saas-arcgis-tutorial/ic784748.png "tagok automatikus hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-213">![Add Members Automatically](./media/active-directory-saas-arcgis-tutorial/ic784748.png "Add Members Automatically")</span></span>

4. <span data-ttu-id="8bf4d-214">A hello **tagok** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="8bf4d-214">On hello **Members** dialog page, perform hello following steps:</span></span>
   
     <span data-ttu-id="8bf4d-215">![Adja hozzá, és tekintse át](./media/active-directory-saas-arcgis-tutorial/ic784749.png "hozzáadása, és nézze meg")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-215">![Add and review](./media/active-directory-saas-arcgis-tutorial/ic784749.png "Add and review")</span></span>
    
     <span data-ttu-id="8bf4d-216">a.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-216">a.</span></span> <span data-ttu-id="8bf4d-217">Adja meg a hello **E-mail**, **Utónév**, és **Vezetéknév** tooprovision kívánt fiók érvényes aad-ben.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-217">Enter hello **Email**, **First Name**, and **Last Name** of a valid AAD account you want tooprovision.</span></span>
  
     <span data-ttu-id="8bf4d-218">b.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-218">b.</span></span> <span data-ttu-id="8bf4d-219">Kattintson a **hozzáadása, és NÉZZE meg**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-219">Click **ADD AND REVIEW**.</span></span>
5. <span data-ttu-id="8bf4d-220">Tekintse át a hello adatokat adta-e, és kattintson **tagok hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-220">Review hello data you have entered, and then click **ADD MEMBERS**.</span></span>
   
    <span data-ttu-id="8bf4d-221">![Tag hozzáadása](./media/active-directory-saas-arcgis-tutorial/ic784750.png "tag hozzáadása")</span><span class="sxs-lookup"><span data-stu-id="8bf4d-221">![Add member](./media/active-directory-saas-arcgis-tutorial/ic784750.png "Add member")</span></span>
        
    > [!NOTE]
    > <span data-ttu-id="8bf4d-222">hello Azure Active Directory fióktulajdonos fog egy e-maileket, és kövesse a fiókjuk egy hivatkozás tooconfirm mielőtt aktívvá válik.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-222">hello Azure Active Directory account holder will receive an email and follow a link tooconfirm their account before it becomes active.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="8bf4d-223">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="8bf4d-223">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="8bf4d-224">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés Online hozzáférés tooArcGIS megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-224">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooArcGIS Online.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="8bf4d-226">**tooassign Britta Simon tooArcGIS Online, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="8bf4d-226">**tooassign Britta Simon tooArcGIS Online, perform hello following steps:**</span></span>

1. <span data-ttu-id="8bf4d-227">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-227">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="8bf4d-229">Hello alkalmazások listában válassza ki a **ArcGIS Online**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-229">In hello applications list, select **ArcGIS Online**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-arcgis-tutorial/tutorial_arcgisonline_app.png) 

3. <span data-ttu-id="8bf4d-231">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-231">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="8bf4d-233">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-233">Click **Add** button.</span></span> <span data-ttu-id="8bf4d-234">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-234">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="8bf4d-236">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-236">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="8bf4d-237">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-237">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="8bf4d-238">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-238">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="8bf4d-239">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="8bf4d-239">Testing single sign-on</span></span>

<span data-ttu-id="8bf4d-240">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-240">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="8bf4d-241">Hello ArcGIS Online csempe a hozzáférési Panel hello kattintáskor automatikusan bejelentkezett tooyour ArcGIS Online alkalmazás kapja meg.</span><span class="sxs-lookup"><span data-stu-id="8bf4d-241">When you click hello ArcGIS Online tile in hello Access Panel, you should get automatically signed-on tooyour ArcGIS Online application.</span></span>
<span data-ttu-id="8bf4d-242">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="8bf4d-242">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8bf4d-243">További források</span><span class="sxs-lookup"><span data-stu-id="8bf4d-243">Additional resources</span></span>

* [<span data-ttu-id="8bf4d-244">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="8bf4d-244">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="8bf4d-245">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="8bf4d-245">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-arcgis-tutorial/tutorial_general_203.png

