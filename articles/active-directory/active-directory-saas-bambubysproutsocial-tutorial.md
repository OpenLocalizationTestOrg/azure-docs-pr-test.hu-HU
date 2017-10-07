---
title: "Oktatóanyag: Azure Active Directoryval integrált közösségi csíra által Bambu |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és a közösségi csíra által Bambu között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d2b9ddbc-cab7-40d6-aca1-5b171cab4199
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2017
ms.author: jeedes
ms.openlocfilehash: c9998772c032476f9a18054873022f55b4bb78be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bambu-by-sprout-social"></a><span data-ttu-id="7b199-103">Oktatóanyag: Azure Active Directoryval integrált közösségi csíra által Bambu</span><span class="sxs-lookup"><span data-stu-id="7b199-103">Tutorial: Azure Active Directory integration with Bambu by Sprout Social</span></span>

<span data-ttu-id="7b199-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Bambu által közösségi csíra az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="7b199-104">In this tutorial, you learn how toointegrate Bambu by Sprout Social with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="7b199-105">Közösségi csíra által Bambu integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="7b199-105">Integrating Bambu by Sprout Social with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="7b199-106">Megadhatja a hozzáférés tooBambu által közösségi csíra rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="7b199-106">You can control in Azure AD who has access tooBambu by Sprout Social</span></span>
- <span data-ttu-id="7b199-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooBambu közösségi csíra (egyszeri bejelentkezés) által az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="7b199-107">You can enable your users tooautomatically get signed-on tooBambu by Sprout Social (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="7b199-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="7b199-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="7b199-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="7b199-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

<!--## Overview

tooenable single sign-on with Bambu by Sprout Social, it must be configured toouse Azure Active Directory as an identity provider. This guide provides information and tips on how tooperform this configuration in Bambu by Sprout Social.

>[!Note]: 
>This embedded guide is brand new in hello new Azure portal, and we’d love toohear your thoughts. Use hello Feedback ? button at hello top of hello portal tooprovide feedback. hello older guide for using hello [Azure classic portal](https://manage.windowsazure.com) tooconfigure this application can be found [here](https://github.com/Azure/AzureAD-App-Docs/blob/master/articles/en-us/_/sso_configure.md).-->


## <a name="prerequisites"></a><span data-ttu-id="7b199-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7b199-110">Prerequisites</span></span>

<span data-ttu-id="7b199-111">tooconfigure Bambu közösségi csíra által az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="7b199-111">tooconfigure Azure AD integration with Bambu by Sprout Social, you need hello following items:</span></span>

- <span data-ttu-id="7b199-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="7b199-112">An Azure AD subscription</span></span>
- <span data-ttu-id="7b199-113">A közösségi csíra egyszeri bejelentkezést engedélyezett előfizetés által Bambu</span><span class="sxs-lookup"><span data-stu-id="7b199-113">A Bambu by Sprout Social single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="7b199-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="7b199-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="7b199-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="7b199-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="7b199-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="7b199-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="7b199-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7b199-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="7b199-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="7b199-118">Scenario description</span></span>
<span data-ttu-id="7b199-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="7b199-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="7b199-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="7b199-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="7b199-121">Hello gyűjteményből Bambu által csíra közösségi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7b199-121">Adding Bambu by Sprout Social from hello gallery</span></span>
2. <span data-ttu-id="7b199-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7b199-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-bambu-by-sprout-social-from-hello-gallery"></a><span data-ttu-id="7b199-123">Hello gyűjteményből Bambu által csíra közösségi hozzáadása</span><span class="sxs-lookup"><span data-stu-id="7b199-123">Adding Bambu by Sprout Social from hello gallery</span></span>
<span data-ttu-id="7b199-124">tooconfigure hello integrációja Bambu közösségi csíra által az Azure AD-be, meg kell tooadd által közösségi csíra Bambu hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="7b199-124">tooconfigure hello integration of Bambu by Sprout Social into Azure AD, you need tooadd Bambu by Sprout Social from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="7b199-125">**tooadd Bambu által közösségi csíra hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7b199-125">**tooadd Bambu by Sprout Social from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b199-126">A hello  **[Azure Portal](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7b199-126">In hello **[Azure Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="7b199-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="7b199-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="7b199-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7b199-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="7b199-131">Kattintson a **új alkalmazás** hello felül hello párbeszédpanel tooadd új alkalmazás gomb.</span><span class="sxs-lookup"><span data-stu-id="7b199-131">Click **New application** button on hello top of hello dialog tooadd new application.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="7b199-133">Hello keresési mezőbe, írja be a **által közösségi csíra Bambu**.</span><span class="sxs-lookup"><span data-stu-id="7b199-133">In hello search box, type **Bambu by Sprout Social**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_search.png)

5. <span data-ttu-id="7b199-135">Hello eredmények panelen, jelölje ki a **által közösségi csíra Bambu**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="7b199-135">In hello results panel, select **Bambu by Sprout Social**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="7b199-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="7b199-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="7b199-138">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést a Bambu által csíra közösségi "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="7b199-138">In this section, you configure and test Azure AD single sign-on with Bambu by Sprout Social based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="7b199-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó által közösségi csíra Bambu tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="7b199-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Bambu by Sprout Social is tooa user in Azure AD.</span></span> <span data-ttu-id="7b199-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello Bambu közösségi csíra által közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="7b199-140">In other words, a link relationship between an Azure AD user and hello related user in Bambu by Sprout Social needs toobe established.</span></span>

<span data-ttu-id="7b199-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a közösségi csíra által Bambu.</span><span class="sxs-lookup"><span data-stu-id="7b199-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Bambu by Sprout Social.</span></span>

<span data-ttu-id="7b199-142">tooconfigure és az Azure AD az egyszeri bejelentkezés által közösségi csíra Bambu-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="7b199-142">tooconfigure and test Azure AD single sign-on with Bambu by Sprout Social, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="7b199-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="7b199-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="7b199-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b199-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="7b199-145">**[Egy közösségi csíra teszt felhasználó Bambu létrehozása](#creating-a-bambu-by-sprout-social-test-user)**  -toohave Britta Simon Bambu által csíra közösségi, amely az Azure AD csatolt toohello ábrázolása rá, hogy az egy megfelelője.</span><span class="sxs-lookup"><span data-stu-id="7b199-145">**[Creating a Bambu by Sprout Social test user](#creating-a-bambu-by-sprout-social-test-user)** - toohave a counterpart of Britta Simon in Bambu by Sprout Social that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="7b199-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="7b199-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="7b199-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="7b199-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="7b199-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="7b199-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="7b199-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez a Bambu közösségi csíra alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7b199-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Bambu by Sprout Social application.</span></span>

<span data-ttu-id="7b199-150">**Bambu közösségi csíra által az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7b199-150">**tooconfigure Azure AD single sign-on with Bambu by Sprout Social, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b199-151">Az Azure portál, a hello hello **által közösségi csíra Bambu** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="7b199-151">In hello Azure portal, on hello **Bambu by Sprout Social** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="7b199-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="7b199-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_samlbase.png)

3. <span data-ttu-id="7b199-155">A hello **Bambu közösségi tartomány csíra és URL-címek** területen hello felhasználó nem rendelkezik tooperform olyan lépéseket, hello alkalmazás már előre integrálva van az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="7b199-155">On hello **Bambu by Sprout Social Domain and URLs** section, hello user does not have tooperform any steps as hello app is already pre-integrated with Azure.</span></span> 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_url.png)

4. <span data-ttu-id="7b199-157">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7b199-157">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_certificate.png) 

5. <span data-ttu-id="7b199-159">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b199-159">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_400.png)
    
6. <span data-ttu-id="7b199-161">A hello **közösségi konfigurációja csíra Bambu** területén kattintson **Bambu konfigurálása által a közösségi csíra** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="7b199-161">On hello **Bambu by Sprout Social Configuration** section, click **Configure Bambu by Sprout Social** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="7b199-162">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="7b199-162">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_configure.png) 

7. <span data-ttu-id="7b199-164">tooconfigure egyszeri bejelentkezést a **által közösségi csíra Bambu** oldalon kell letöltött toosend hello **metaadatainak XML-kódja** és **SAML-alapú egyszeri bejelentkezési URL-címe** túl[Csíra szociális támogatás által Bambu](mailto:support@getbambu.com).</span><span class="sxs-lookup"><span data-stu-id="7b199-164">tooconfigure single sign-on on **Bambu by Sprout Social** side, you need toosend hello downloaded **Metadata XML** and **SAML Single Sign-On Service URL** too[Bambu by Sprout Social support](mailto:support@getbambu.com).</span></span> <span data-ttu-id="7b199-165">Azok fog beállítása ennek rendelés toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="7b199-165">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>

> [!TIP]
> <span data-ttu-id="7b199-166">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="7b199-166">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="7b199-167">Az alkalmazás hozzáadása a hello után **Active Directory > Vállalati alkalmazások** területen egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott dokumentáció hello keresztül**Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="7b199-167">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click on hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="7b199-168">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="7b199-168">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

<!--### Next steps

tooensure users can sign-in tooBambu by Sprout Social after it has been configured toouse Azure Active Directory, review hello following tasks and topics:

- User accounts must be pre-provisioned into Bambu by Sprout Social prior toosign-in. tooset this up, see Provisioning.
 
- Users must be assigned access tooBambu by Sprout Social in Azure AD toosign-in. tooassign users, see Users.
 
- tooconfigure access polices for Bambu by Sprout Social users, see Access Policies.
 
- For additional information on deploying single sign-on toousers, see [this article](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis#deploying-azure-ad-integrated-applications-to-users).-->


### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="7b199-169">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b199-169">Creating an Azure AD test user</span></span>
<span data-ttu-id="7b199-170">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="7b199-170">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="7b199-172">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="7b199-172">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b199-173">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="7b199-173">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="7b199-175">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="7b199-175">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="7b199-177">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b199-177">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="7b199-179">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="7b199-179">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bambubysproutsocial-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="7b199-181">a.</span><span class="sxs-lookup"><span data-stu-id="7b199-181">a.</span></span> <span data-ttu-id="7b199-182">A hello **neve** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="7b199-182">In hello **Name** textbox, type **Britta Simon**.</span></span>

    <span data-ttu-id="7b199-183">b.</span><span class="sxs-lookup"><span data-stu-id="7b199-183">b.</span></span> <span data-ttu-id="7b199-184">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="7b199-184">In hello **User name** textbox, type hello **email address** of Britta Simon.</span></span>

    <span data-ttu-id="7b199-185">c.</span><span class="sxs-lookup"><span data-stu-id="7b199-185">c.</span></span> <span data-ttu-id="7b199-186">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="7b199-186">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="7b199-187">d.</span><span class="sxs-lookup"><span data-stu-id="7b199-187">d.</span></span> <span data-ttu-id="7b199-188">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="7b199-188">Click **Create**.</span></span>
 
### <a name="creating-a-bambu-by-sprout-social-test-user"></a><span data-ttu-id="7b199-189">Egy Bambu által közösségi csíra tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="7b199-189">Creating a Bambu by Sprout Social test user</span></span>

<span data-ttu-id="7b199-190">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="7b199-190">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="7b199-191">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="7b199-191">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="7b199-192">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooBambu által közösségi csíra megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="7b199-192">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooBambu by Sprout Social.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="7b199-194">**tooassign Britta Simon tooBambu által közösségi csíra hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="7b199-194">**tooassign Britta Simon tooBambu by Sprout Social, perform hello following steps:**</span></span>

1. <span data-ttu-id="7b199-195">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="7b199-195">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="7b199-197">Hello alkalmazások listában válassza ki a **által közösségi csíra Bambu**.</span><span class="sxs-lookup"><span data-stu-id="7b199-197">In hello applications list, select **Bambu by Sprout Social**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_bambubysproutsocial_app.png) 

3. <span data-ttu-id="7b199-199">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="7b199-199">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="7b199-201">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="7b199-201">Click **Add** button.</span></span> <span data-ttu-id="7b199-202">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b199-202">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="7b199-204">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="7b199-204">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="7b199-205">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b199-205">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="7b199-206">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="7b199-206">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="7b199-207">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="7b199-207">Testing single sign-on</span></span>

<span data-ttu-id="7b199-208">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="7b199-208">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="7b199-209">Ha a hozzáférési Panel hello közösségi csíra csempe által Bambu hello gombra kattint, automatikusan bejelentkezett tooyour Bambu közösségi csíra alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="7b199-209">When you click hello Bambu by Sprout Social tile in hello Access Panel, you should get automatically signed-on tooyour Bambu by Sprout Social application.</span></span> <span data-ttu-id="7b199-210">A hozzáférési Panel kapcsolatos további tudnivalókért lásd: [a hozzáférési Panel bemutatása](https://msdn.microsoft.com/library/dn308586).</span><span class="sxs-lookup"><span data-stu-id="7b199-210">For more details about the Access Panel, see [Introduction to the Access Panel](https://msdn.microsoft.com/library/dn308586).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="7b199-211">További források</span><span class="sxs-lookup"><span data-stu-id="7b199-211">Additional resources</span></span>

* [<span data-ttu-id="7b199-212">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="7b199-212">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="7b199-213">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="7b199-213">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-bambubysproutsocial-tutorial/tutorial_general_203.png

