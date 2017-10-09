---
title: "Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Splunk vállalati és Splunk felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: b3e2b4a9-749c-4895-813d-db46f8dfdbf8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/09/2017
ms.author: jeedes
ms.openlocfilehash: 9bb6817cb31dce684cd9cc1c567fa3efc8906ad6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="546d9-103">Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő</span><span class="sxs-lookup"><span data-stu-id="546d9-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="546d9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Splunk vállalati és Splunk felhőalapú Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="546d9-104">In this tutorial, you learn how toointegrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="546d9-105">Splunk vállalati és Splunk felhő integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="546d9-106">Szabályozhatja az Azure AD, aki hozzáféréssel rendelkezik tooSplunk vállalati és Splunk felhő</span><span class="sxs-lookup"><span data-stu-id="546d9-106">You can control in Azure AD who has access tooSplunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="546d9-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSplunk vállalati és Splunk felhő egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="546d9-107">You can enable your users tooautomatically get signed-on tooSplunk Enterprise and Splunk Cloud single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="546d9-108">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="546d9-108">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="546d9-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="546d9-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="546d9-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="546d9-110">Prerequisites</span></span>

<span data-ttu-id="546d9-111">az Azure AD-integráció a Splunk vállalati és Splunk felhő tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-111">tooconfigure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need hello following items:</span></span>

- <span data-ttu-id="546d9-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="546d9-112">An Azure AD subscription</span></span>
- <span data-ttu-id="546d9-113">Egy Splunk vállalati vagy Splunk felhő SSO engedélyezve előfizetés</span><span class="sxs-lookup"><span data-stu-id="546d9-113">A Splunk Enterprise or Splunk Cloud SSO enabled subscription</span></span>


>[!NOTE]
><span data-ttu-id="546d9-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="546d9-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="546d9-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="546d9-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="546d9-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="546d9-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="546d9-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="546d9-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="546d9-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="546d9-118">Scenario description</span></span>
<span data-ttu-id="546d9-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="546d9-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="546d9-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="546d9-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="546d9-121">Hello gyűjteményből Splunk vállalati és Splunk felhő hozzáadása</span><span class="sxs-lookup"><span data-stu-id="546d9-121">Adding Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
2. <span data-ttu-id="546d9-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="546d9-122">Configuring and testing Azure AD SSO</span></span>


## <a name="add-splunk-enterprise-and-splunk-cloud-from-hello-gallery"></a><span data-ttu-id="546d9-123">Adja hozzá a Splunk vállalati és Splunk felhő hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="546d9-123">Add Splunk Enterprise and Splunk Cloud from hello gallery</span></span>
<span data-ttu-id="546d9-124">tooconfigure hello Splunk vállalati és integrációját Splunk felhő az Azure AD-be, meg kell a tooadd Splunk vállalati, és Splunk felhő hello gyűjtemény tooyour listája felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="546d9-124">tooconfigure hello integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need tooadd Splunk Enterprise and Splunk Cloud from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="546d9-125">**tooadd Splunk vállalati és Splunk felhő hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="546d9-125">**tooadd Splunk Enterprise and Splunk Cloud from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="546d9-126">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="546d9-126">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]

2. <span data-ttu-id="546d9-128">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="546d9-128">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="546d9-129">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="546d9-129">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="546d9-131">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="546d9-131">Click **Add** at hello bottom of hello page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="546d9-133">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="546d9-133">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>

    ![Alkalmazások][4]

6. <span data-ttu-id="546d9-135">Hello keresési mezőbe, írja be a **Splunk vállalati vagy Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="546d9-135">In hello search box, type **Splunk Enterprise or Splunk Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. <span data-ttu-id="546d9-137">Hello eredmények ablaktábláján jelöljön ki **Splunk vállalati és Splunk felhő**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="546d9-137">In hello results pane, select **Splunk Enterprise and Splunk Cloud**, and then click **Complete** tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="546d9-139">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="546d9-139">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="546d9-140">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a vállalati Splunk tesztelése és konfigurálása, és Splunk felhő "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="546d9-140">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="546d9-141">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Splunk vállalati és Splunk felhő tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="546d9-141">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Splunk Enterprise and Splunk Cloud is tooa user in Azure AD.</span></span> <span data-ttu-id="546d9-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Splunk vállalati és Splunk felhő közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="546d9-142">In other words, a link relationship between an Azure AD user and hello related user in Splunk Enterprise and Splunk Cloud needs toobe established.</span></span>

<span data-ttu-id="546d9-143">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** Splunk vállalati és Splunk felhő.</span><span class="sxs-lookup"><span data-stu-id="546d9-143">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in Splunk Enterprise and Splunk Cloud.</span></span>

<span data-ttu-id="546d9-144">tooconfigure és Splunk vállalati és Splunk felhőalapú Azure AD az egyszeri bejelentkezés tesztelési, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="546d9-144">tooconfigure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="546d9-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="546d9-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="546d9-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="546d9-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="546d9-147">**[Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  -toohave Britta Simon Splunk vállalati és Splunk felhőben található, az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="546d9-147">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - toohave a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="546d9-148">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="546d9-148">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="546d9-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="546d9-149">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="546d9-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="546d9-150">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="546d9-151">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést hello a klasszikus portálon, és egyszeri bejelentkezés konfigurálása az Splunk vállalati és Splunk felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="546d9-151">In this section, you enable Azure AD SSO in hello classic portal and configure SSO in your Splunk Enterprise and Splunk Cloud application.</span></span>


<span data-ttu-id="546d9-152">**Splunk vállalati és Splunk felhőalapú Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="546d9-152">**tooconfigure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="546d9-153">Hello klasszikus portál, a hello **Splunk vállalati és Splunk felhő** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="546d9-153">In hello classic portal, on hello **Splunk Enterprise and Splunk Cloud** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="546d9-155">A hello **hová a tooSplunk vállalati felhasználók toosign és Splunk felhő** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="546d9-155">On hello **How would you like users toosign on tooSplunk Enterprise and Splunk Cloud** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. <span data-ttu-id="546d9-157">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-157">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. <span data-ttu-id="546d9-159">A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign a tooyour Splunk vállalati és mintát a következő hello használó Splunk felhőalapú alkalmazások által használt típus hello URL:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="546d9-159">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour Splunk Enterprise and Splunk Cloud application using hello following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
  2. <span data-ttu-id="546d9-160">A hello **azonosító** szövegmezőhöz típus hello kiszolgáló URL-CÍMÉT a Splunk.</span><span class="sxs-lookup"><span data-stu-id="546d9-160">In hello **Identifier** textbox, type hello URL of your Splunk Server.</span></span>
  3. <span data-ttu-id="546d9-161">A hello **válasz URL-CÍMEN** szövegmezőhöz típus hello URL-cím a következő mintát hello elé:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="546d9-161">In hello **Reply URL** textbox, type hello URL with hello following pattern: `https://<splunkserver>/saml/acs`</span></span>
  4. <span data-ttu-id="546d9-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="546d9-162">Click **Next**.</span></span>
 
4. <span data-ttu-id="546d9-163">A hello **Splunk vállalati és Splunk felhő egyszeri bejelentkezés konfigurálásáról** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-163">On hello **Configure single sign-on at Splunk Enterprise and Splunk Cloud** page, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. <span data-ttu-id="546d9-165">Kattintson a **metaadatok letöltése**, majd mentse hello fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="546d9-165">Click **Download metadata**, and then save hello file on your computer.</span></span>
  2. <span data-ttu-id="546d9-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="546d9-166">Click **Next**.</span></span>

5. <span data-ttu-id="546d9-167">az alkalmazáshoz konfigurált SSO tooget Forduljon Splunk vállalati és Splunk felhő támogatási csoport, és adja meg hello következő:</span><span class="sxs-lookup"><span data-stu-id="546d9-167">tooget SSO configured for your application, contact Splunk Enterprise and Splunk Cloud support team and provide them with hello following:</span></span>

    * <span data-ttu-id="546d9-168">letöltött hello **federaton metaadatok**</span><span class="sxs-lookup"><span data-stu-id="546d9-168">hello downloaded **federaton metadata**</span></span>
6. <span data-ttu-id="546d9-169">A klasszikus portálon hello, válassza ki a hello egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="546d9-169">In hello classic portal, select hello single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="546d9-171">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="546d9-171">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="546d9-173">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="546d9-173">Create an Azure AD test user</span></span>
<span data-ttu-id="546d9-174">Ebben a szakaszban a tesztfelhasználó hello Britta Simon neve a klasszikus portálon létrehozta.</span><span class="sxs-lookup"><span data-stu-id="546d9-174">In this section, you create a test user in hello classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="546d9-176">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="546d9-176">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="546d9-177">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="546d9-177">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="546d9-179">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="546d9-179">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="546d9-180">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="546d9-180">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="546d9-182">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="546d9-182">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="546d9-184">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-184">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="546d9-186">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="546d9-186">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="546d9-187">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="546d9-187">In hello User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="546d9-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="546d9-188">Click **Next**.</span></span>

6.  <span data-ttu-id="546d9-189">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-189">On hello **User Profile** dialog page, perform hello following steps:</span></span>
  
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. <span data-ttu-id="546d9-191">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="546d9-191">In hello **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="546d9-192">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="546d9-192">In hello **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="546d9-193">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="546d9-193">In hello **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="546d9-194">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="546d9-194">In hello **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="546d9-195">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="546d9-195">Click **Next**.</span></span>

7. <span data-ttu-id="546d9-196">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="546d9-196">On hello **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="546d9-198">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="546d9-198">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. <span data-ttu-id="546d9-200">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="546d9-200">Write down hello value of hello **New Password**.</span></span>
  2. <span data-ttu-id="546d9-201">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="546d9-201">Click **Complete**.</span></span>   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="546d9-202">Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="546d9-202">Create a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="546d9-203">Ebben a szakaszban Britta Simon Splunk vállalati és Splunk felhő nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="546d9-203">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="546d9-204">Támogatási csoport tooadd hello felhasználók hello Splunk vállalati és Splunk Cloud platform adjon működik az Splunk vállalati és Splunk felhő.</span><span class="sxs-lookup"><span data-stu-id="546d9-204">Please work with Splunk Enterprise and Splunk Cloud support team tooadd hello users in hello Splunk Enterprise and Splunk Cloud platform.</span></span>


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="546d9-205">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="546d9-205">Assign hello Azure AD test user</span></span>

<span data-ttu-id="546d9-206">Ebben a szakaszban Britta Simon toouse saját hozzáférés tooSplunk vállalati és Splunk felhőalapú Azure SSOy engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="546d9-206">In this section, you enable Britta Simon toouse Azure SSOy granting her access tooSplunk Enterprise and Splunk Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="546d9-208">**tooassign Britta Simon tooSplunk vállalati és Splunk felhő, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="546d9-208">**tooassign Britta Simon tooSplunk Enterprise and Splunk Cloud, perform hello following steps:**</span></span>

1. <span data-ttu-id="546d9-209">Klasszikus portál hello tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="546d9-209">On hello classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="546d9-211">Hello alkalmazások listában válassza ki a **Splunk vállalati és Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="546d9-211">In hello applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. <span data-ttu-id="546d9-213">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="546d9-213">In hello menu on hello top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203]

4. <span data-ttu-id="546d9-215">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="546d9-215">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="546d9-216">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="546d9-216">In hello toolbar on hello bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="546d9-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="546d9-218">Test single sign-on</span></span>

<span data-ttu-id="546d9-219">Ebben a szakaszban az Azure AD SSOonfiguration hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="546d9-219">In this section, you test your Azure AD SSOonfiguration using hello Access Panel.</span></span>

<span data-ttu-id="546d9-220">Hello Splunk vállalati és a hozzáférési Panel hello Splunk felhő csempe kattintáskor automatikusan bejelentkezett tooyour Splunk vállalati és Splunk felhő alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="546d9-220">When you click hello Splunk Enterprise and Splunk Cloud tile in hello Access Panel, you should get automatically signed-on tooyour Splunk Enterprise and Splunk Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="546d9-221">További források</span><span class="sxs-lookup"><span data-stu-id="546d9-221">Additional resources</span></span>

* [<span data-ttu-id="546d9-222">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="546d9-222">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="546d9-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="546d9-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_general_205.png
