---
title: "Oktatóanyag: Azure Active Directoryval integrált AnswerHub |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és AnswerHub között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 818b91d7-01df-4b36-9706-f167c710a73c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: jeedes
ms.openlocfilehash: 90b530da31abe7e6f18bfa2c5409f8ff1d4f1063
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-answerhub"></a><span data-ttu-id="d31f9-103">Oktatóanyag: Azure Active Directoryval integrált AnswerHub</span><span class="sxs-lookup"><span data-stu-id="d31f9-103">Tutorial: Azure Active Directory integration with AnswerHub</span></span>

<span data-ttu-id="d31f9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate AnswerHub az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d31f9-104">In this tutorial, you learn how toointegrate AnswerHub with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d31f9-105">AnswerHub integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-105">Integrating AnswerHub with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="d31f9-106">Megadhatja a hozzáférés tooAnswerHub rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d31f9-106">You can control in Azure AD who has access tooAnswerHub</span></span>
- <span data-ttu-id="d31f9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooAnswerHub (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="d31f9-107">You can enable your users tooautomatically get signed-on tooAnswerHub (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="d31f9-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="d31f9-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="d31f9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d31f9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d31f9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d31f9-110">Prerequisites</span></span>

<span data-ttu-id="d31f9-111">az Azure AD integrálása AnswerHub tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-111">tooconfigure Azure AD integration with AnswerHub, you need hello following items:</span></span>

- <span data-ttu-id="d31f9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d31f9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d31f9-113">Egy AnswerHub egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="d31f9-113">An AnswerHub single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="d31f9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d31f9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="d31f9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="d31f9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="d31f9-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d31f9-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="d31f9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d31f9-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d31f9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d31f9-118">Scenario description</span></span>
<span data-ttu-id="d31f9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d31f9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="d31f9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d31f9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d31f9-121">Hello gyűjteményből AnswerHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d31f9-121">Adding AnswerHub from hello gallery</span></span>
2. <span data-ttu-id="d31f9-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d31f9-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-answerhub-from-hello-gallery"></a><span data-ttu-id="d31f9-123">Hello gyűjteményből AnswerHub hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d31f9-123">Adding AnswerHub from hello gallery</span></span>
<span data-ttu-id="d31f9-124">tooconfigure hello integrációja AnswerHub az Azure AD-be, meg kell tooadd AnswerHub hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="d31f9-124">tooconfigure hello integration of AnswerHub into Azure AD, you need tooadd AnswerHub from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d31f9-125">**tooadd AnswerHub hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d31f9-125">**tooadd AnswerHub from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="d31f9-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="d31f9-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="d31f9-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="d31f9-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="d31f9-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="d31f9-133">Hello keresési mezőbe, írja be a **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-133">In hello search box, type **AnswerHub**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_search.png)

5. <span data-ttu-id="d31f9-135">A hello eredmények panelen válassza ki a **AnswerHub**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-135">In hello results panel, select **AnswerHub**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="d31f9-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="d31f9-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="d31f9-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján AnswerHub.</span><span class="sxs-lookup"><span data-stu-id="d31f9-138">In this section, you configure and test Azure AD single sign-on with AnswerHub based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="d31f9-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó AnswerHub tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d31f9-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in AnswerHub is tooa user in Azure AD.</span></span> <span data-ttu-id="d31f9-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello AnswerHub közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="d31f9-140">In other words, a link relationship between an Azure AD user and hello related user in AnswerHub needs toobe established.</span></span>

<span data-ttu-id="d31f9-141">AnswerHub, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d31f9-141">In AnswerHub, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="d31f9-142">tooconfigure és az Azure AD az egyszeri bejelentkezés AnswerHub-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="d31f9-142">tooconfigure and test Azure AD single sign-on with AnswerHub, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="d31f9-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d31f9-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="d31f9-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="d31f9-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="d31f9-145">**[Egy AnswerHub tesztfelhasználó létrehozása](#creating-an-answerhub-test-user)**  -toohave egy megfelelője a Britta Simon a AnswerHub, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="d31f9-145">**[Creating an AnswerHub test user](#creating-an-answerhub-test-user)** - toohave a counterpart of Britta Simon in AnswerHub that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="d31f9-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d31f9-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d31f9-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="d31f9-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="d31f9-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d31f9-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="d31f9-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel a hello Azure-portálon, és konfigurálása egyszeri bejelentkezéshez az AnswerHub alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="d31f9-149">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your AnswerHub application.</span></span>

<span data-ttu-id="d31f9-150">**az Azure AD tooconfigure egyszeri bejelentkezést a AnswerHub, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="d31f9-150">**tooconfigure Azure AD single sign-on with AnswerHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="d31f9-151">Az Azure portál, a hello hello **AnswerHub** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-151">In hello Azure portal, on hello **AnswerHub** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="d31f9-153">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="d31f9-153">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_samlbase.png)

3. <span data-ttu-id="d31f9-155">A hello **AnswerHub tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-155">On hello **AnswerHub Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_url.png)

    <span data-ttu-id="d31f9-157">a.</span><span class="sxs-lookup"><span data-stu-id="d31f9-157">a.</span></span> <span data-ttu-id="d31f9-158">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="d31f9-158">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<company>.answerhub.com`</span></span>

    <span data-ttu-id="d31f9-159">b.</span><span class="sxs-lookup"><span data-stu-id="d31f9-159">b.</span></span> <span data-ttu-id="d31f9-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<company>.answerhub.com`</span><span class="sxs-lookup"><span data-stu-id="d31f9-160">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<company>.answerhub.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="d31f9-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="d31f9-161">These values are not real.</span></span> <span data-ttu-id="d31f9-162">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="d31f9-162">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="d31f9-163">Ügyfél [AnswerHub ügyfél-támogatási csoport](mailto:success@answerhub.com) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="d31f9-163">Contact [AnswerHub Client support team](mailto:success@answerhub.com) tooget these values.</span></span> 
 
4. <span data-ttu-id="d31f9-164">A hello **SAML-aláíró tanúsítványa** kattintson **Certificate(Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d31f9-164">On hello **SAML Signing Certificate** section, click **Certificate(Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_certificate.png) 

5. <span data-ttu-id="d31f9-166">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-166">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d31f9-168">A hello **AnswerHub konfigurációs** kattintson **konfigurálása AnswerHub** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="d31f9-168">On hello **AnswerHub Configuration** section, click **Configure AnswerHub** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="d31f9-169">Másolás hello **Sign-Out URL-címet, és a SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="d31f9-169">Copy hello **Sign-Out URL, and SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_configure.png) 

7. <span data-ttu-id="d31f9-171">Egy másik webes böngészőablakban jelentkezzen be a AnswerHub vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d31f9-171">In a different web browser window, log into your AnswerHub company site as an administrator.</span></span>
   
    >[!NOTE]
    ><span data-ttu-id="d31f9-172">Ha AnswerHub segítségre van szüksége, forduljon a [AnswerHub tartozó támogatási csoport](mailto:success@answerhub.com.).</span><span class="sxs-lookup"><span data-stu-id="d31f9-172">If you need help configuring AnswerHub, contact [AnswerHub's support team](mailto:success@answerhub.com.).</span></span>
   
8. <span data-ttu-id="d31f9-173">Nyissa meg túl**felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-173">Go too**Administration**.</span></span>

9. <span data-ttu-id="d31f9-174">Kattintson a hello **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="d31f9-174">Click hello **User and Group** tab.</span></span>

10. <span data-ttu-id="d31f9-175">Hello navigációs ablakban a bal oldalt, a hello hello **közösségi beállítások** kattintson **SAML-alapú telepítő**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-175">In hello navigation pane on hello left side, in hello **Social Settings** section, click **SAML Setup**.</span></span>

11. <span data-ttu-id="d31f9-176">Kattintson a **IDP Config** fülre.</span><span class="sxs-lookup"><span data-stu-id="d31f9-176">Click **IDP Config** tab.</span></span>

12. <span data-ttu-id="d31f9-177">A hello **IDP Config** lapra, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-177">On hello **IDP Config** tab, perform hello following steps:</span></span>

     <span data-ttu-id="d31f9-178">![SAML-alapú telepítő](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML-alapú telepítő")</span><span class="sxs-lookup"><span data-stu-id="d31f9-178">![SAML Setup](./media/active-directory-saas-answerhub-tutorial/ic785172.png "SAML Setup")</span></span>  
  
     <span data-ttu-id="d31f9-179">a.</span><span class="sxs-lookup"><span data-stu-id="d31f9-179">a.</span></span> <span data-ttu-id="d31f9-180">A **IDP bejelentkezési URL-cím** szövegmezőhöz Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe** ami Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d31f9-180">In **IDP Login URL** textbox, paste **SAML Single Sign-On Service URL** which you have copied from Azure portal.</span></span>
  
     <span data-ttu-id="d31f9-181">b.</span><span class="sxs-lookup"><span data-stu-id="d31f9-181">b.</span></span> <span data-ttu-id="d31f9-182">A **IDP kijelentkezési URL-cím** szövegmezőhöz Beillesztés **Sign-Out URL-cím** érték, amely az Azure-portálon másolta.</span><span class="sxs-lookup"><span data-stu-id="d31f9-182">In **IDP Logout URL** textbox, paste **Sign-Out URL** value which you have copied from Azure portal.</span></span>
     
     <span data-ttu-id="d31f9-183">c.</span><span class="sxs-lookup"><span data-stu-id="d31f9-183">c.</span></span> <span data-ttu-id="d31f9-184">A **IDP azonosító formátuma** szövegmezőhöz hello felhasználói azonosító érték ugyanaz, mint a kijelölt Azure-portálon a **felhasználói attribútumok** szakasz.</span><span class="sxs-lookup"><span data-stu-id="d31f9-184">In **IDP Name Identifier Format** textbox, enter hello user Identifier value same as selected in Azure portal in **User Attributes** section.</span></span>
  
     <span data-ttu-id="d31f9-185">d.</span><span class="sxs-lookup"><span data-stu-id="d31f9-185">d.</span></span> <span data-ttu-id="d31f9-186">Kattintson a **a kulcsoknak és tanúsítványoknak**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-186">Click **Keys and Certificates**.</span></span>

13. <span data-ttu-id="d31f9-187">Hello a kulcsoknak és tanúsítványoknak lapon hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-187">On hello Keys and Certificates tab, perform hello following steps:</span></span>
    
     <span data-ttu-id="d31f9-188">![A kulcsoknak és tanúsítványoknak](./media/active-directory-saas-answerhub-tutorial/ic785173.png "a kulcsoknak és tanúsítványoknak")</span><span class="sxs-lookup"><span data-stu-id="d31f9-188">![Keys and Certificates](./media/active-directory-saas-answerhub-tutorial/ic785173.png "Keys and Certificates")</span></span>  
 
     <span data-ttu-id="d31f9-189">a.</span><span class="sxs-lookup"><span data-stu-id="d31f9-189">a.</span></span> <span data-ttu-id="d31f9-190">Nyissa meg a base-64 kódolású tanúsítványt, amely a Jegyzettömbben, a vágólapra tartalmának másolása hello Azure portálról letöltött és toohello Beillesztés **kiállító terjesztési hely nyilvános kulcsát (x 509-formátumú)** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="d31f9-190">Open your base-64 encoded certificate which you have downloaded from Azure portal in notepad, copy hello content of it into your clipboard, and then paste it toohello **IDP Public Key (x509 Format)** textbox.</span></span>
  
     <span data-ttu-id="d31f9-191">b.</span><span class="sxs-lookup"><span data-stu-id="d31f9-191">b.</span></span> <span data-ttu-id="d31f9-192">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-192">Click **Save**.</span></span>

14. <span data-ttu-id="d31f9-193">A hello **IDP Config** lapra, majd **mentése**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-193">On hello **IDP Config** tab, click **Save**.</span></span>

> [!TIP]
> <span data-ttu-id="d31f9-194">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="d31f9-194">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="d31f9-195">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="d31f9-195">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="d31f9-196">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="d31f9-196">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="d31f9-197">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d31f9-197">Creating an Azure AD test user</span></span>
<span data-ttu-id="d31f9-198">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="d31f9-198">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="d31f9-200">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="d31f9-200">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="d31f9-201">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-201">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d31f9-203">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-203">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d31f9-205">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d31f9-205">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d31f9-207">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d31f9-207">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-answerhub-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="d31f9-209">a.</span><span class="sxs-lookup"><span data-stu-id="d31f9-209">a.</span></span> <span data-ttu-id="d31f9-210">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-210">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="d31f9-211">b.</span><span class="sxs-lookup"><span data-stu-id="d31f9-211">b.</span></span> <span data-ttu-id="d31f9-212">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="d31f9-212">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="d31f9-213">c.</span><span class="sxs-lookup"><span data-stu-id="d31f9-213">c.</span></span> <span data-ttu-id="d31f9-214">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-214">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="d31f9-215">d.</span><span class="sxs-lookup"><span data-stu-id="d31f9-215">d.</span></span> <span data-ttu-id="d31f9-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-216">Click **Create**.</span></span>
 
### <a name="creating-an-answerhub-test-user"></a><span data-ttu-id="d31f9-217">Egy AnswerHub tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d31f9-217">Creating an AnswerHub test user</span></span>

<span data-ttu-id="d31f9-218">az Azure AD tooenable felhasználók toolog a tooAnswerHub, akkor ki kell építenie AnswerHub be.</span><span class="sxs-lookup"><span data-stu-id="d31f9-218">tooenable Azure AD users toolog in tooAnswerHub, they must be provisioned into AnswerHub.</span></span>  
<span data-ttu-id="d31f9-219">AnswerHub hello esetben egy kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d31f9-219">In hello case of AnswerHub, provisioning is a manual task.</span></span>

<span data-ttu-id="d31f9-220">**tooprovision egy felhasználói fiókot, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d31f9-220">**tooprovision a user account, perform hello following steps:**</span></span>

1. <span data-ttu-id="d31f9-221">Jelentkezzen be tooyour **AnswerHub** vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d31f9-221">Log in tooyour **AnswerHub** company site as administrator.</span></span>

2. <span data-ttu-id="d31f9-222">Nyissa meg túl**felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-222">Go too**Administration**.</span></span>

3. <span data-ttu-id="d31f9-223">Kattintson a hello **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="d31f9-223">Click hello **Users & Groups** tab.</span></span>

4. <span data-ttu-id="d31f9-224">Hello navigációs ablakban a bal oldalt, a hello hello **felhasználók kezelése** kattintson **létrehozása vagy importálása felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-224">In hello navigation pane on hello left side, in hello **Manage Users** section, click **Create or import users**.</span></span>
   
   <span data-ttu-id="d31f9-225">![Felhasználók és csoportok](./media/active-directory-saas-answerhub-tutorial/ic785175.png "felhasználók és csoportok")</span><span class="sxs-lookup"><span data-stu-id="d31f9-225">![Users & Groups](./media/active-directory-saas-answerhub-tutorial/ic785175.png "Users & Groups")</span></span>

5. <span data-ttu-id="d31f9-226">Típus hello **E-mail cím**, **felhasználónév** és **jelszó** egy érvényes Azure Active Directory-fiókot a kívánt tooprovision hello szövegmezők kapcsolatos, és kattintson a  **Mentés**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-226">Type hello **Email address**, **Username** and **Password** of a valid Azure Active Directory account you want tooprovision into hello related textboxes, and then click **Save**.</span></span>

>[!NOTE]
><span data-ttu-id="d31f9-227">Bármely más AnswerHub felhasználói fiók létrehozása eszközök vagy AnswerHub tooprovision által nyújtott API-k AAD felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="d31f9-227">You can use any other AnswerHub user account creation tools or APIs provided by AnswerHub tooprovision AAD user accounts.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="d31f9-228">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d31f9-228">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="d31f9-229">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooAnswerHub megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="d31f9-229">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooAnswerHub.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="d31f9-231">**tooassign Britta Simon tooAnswerHub, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="d31f9-231">**tooassign Britta Simon tooAnswerHub, perform hello following steps:**</span></span>

1. <span data-ttu-id="d31f9-232">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-232">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d31f9-234">Hello alkalmazások listában válassza ki a **AnswerHub**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-234">In hello applications list, select **AnswerHub**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-answerhub-tutorial/tutorial_answerhub_app.png) 

3. <span data-ttu-id="d31f9-236">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d31f9-236">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="d31f9-238">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d31f9-238">Click **Add** button.</span></span> <span data-ttu-id="d31f9-239">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d31f9-239">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="d31f9-241">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="d31f9-241">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="d31f9-242">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d31f9-242">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="d31f9-243">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="d31f9-243">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="d31f9-244">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d31f9-244">Testing single sign-on</span></span>

<span data-ttu-id="d31f9-245">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d31f9-245">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="d31f9-246">Ha a hozzáférési Panel hello hello AnswerHub csempe gombra kattint, automatikusan bejelentkezett tooyour AnswerHub alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="d31f9-246">When you click hello AnswerHub tile in hello Access Panel, you should get automatically signed-on tooyour AnswerHub application.</span></span>
<span data-ttu-id="d31f9-247">További információ a hozzáférési Panel hello: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d31f9-247">For more information about hello Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d31f9-248">További források</span><span class="sxs-lookup"><span data-stu-id="d31f9-248">Additional resources</span></span>

* [<span data-ttu-id="d31f9-249">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d31f9-249">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d31f9-250">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d31f9-250">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-answerhub-tutorial/tutorial_general_203.png

