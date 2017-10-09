---
title: "Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama Suite |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és SilkRoad élettartama Suite között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 3cd92319-7964-41eb-8712-444f5c8b4d15
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/10/2017
ms.author: jeedes
ms.openlocfilehash: 07367282ab42b7332f166d64743b4b447aec4935
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="e6ffb-103">Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama csomaggal</span><span class="sxs-lookup"><span data-stu-id="e6ffb-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="e6ffb-104">hello Ez az oktatóanyag célja tooshow, hogyan toointegrate SilkRoad élettartama Suite az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-104">hello objective of this tutorial is tooshow you how toointegrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="e6ffb-105">SilkRoad élettartama Suite integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-105">Integrating SilkRoad Life Suite with Azure AD provides you with hello following benefits:</span></span> 

* <span data-ttu-id="e6ffb-106">Megadhatja a hozzáférés tooSilkRoad élettartama Suite rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="e6ffb-106">You can control in Azure AD who has access tooSilkRoad Life Suite</span></span> 
* <span data-ttu-id="e6ffb-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSilkRoad élettartama Suite egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="e6ffb-107">You can enable your users tooautomatically get signed-on tooSilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="e6ffb-108">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-108">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6ffb-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e6ffb-109">Prerequisites</span></span>
<span data-ttu-id="e6ffb-110">tooconfigure az Azure AD-integrációs SilkRoad élettartama csomaggal, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-110">tooconfigure Azure AD integration with SilkRoad Life Suite, you need hello following items:</span></span>

* <span data-ttu-id="e6ffb-111">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="e6ffb-111">An Azure AD subscription</span></span>
* <span data-ttu-id="e6ffb-112">Egy SilkRoad élettartama Suite SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="e6ffb-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="e6ffb-113">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-113">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="e6ffb-114">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-114">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="e6ffb-115">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="e6ffb-116">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="e6ffb-117">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-117">Scenario Description</span></span>
<span data-ttu-id="e6ffb-118">hello Ez az oktatóanyag célja tooenable meg tootest Azure AD SSO tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-118">hello objective of this tutorial is tooenable you tootest Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="e6ffb-119">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-119">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="e6ffb-120">Hello gyűjteményből SilkRoad élettartama csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-120">Adding SilkRoad Life Suite from hello gallery</span></span> 
2. <span data-ttu-id="e6ffb-121">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-hello-gallery"></a><span data-ttu-id="e6ffb-122">Adja hozzá a SilkRoad élettartama Suite hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="e6ffb-122">Add SilkRoad Life Suite from hello gallery</span></span>
<span data-ttu-id="e6ffb-123">tooconfigure hello integrációja SilkRoad élettartama Suite az Azure AD-be, meg kell tooadd SilkRoad élettartama Suite hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-123">tooconfigure hello integration of SilkRoad Life Suite into Azure AD, you need tooadd SilkRoad Life Suite from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="e6ffb-124">**tooadd SilkRoad élettartama Suite hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e6ffb-124">**tooadd SilkRoad Life Suite from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="e6ffb-125">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-125">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="e6ffb-127">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-127">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="e6ffb-128">tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-128">tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="e6ffb-130">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-130">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="e6ffb-132">A hello **miről szeretne toodo** párbeszédpanel, kattintson **hello gyűjteményből alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-132">On hello **What do you want toodo** dialog, click **Add an application from hello gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="e6ffb-134">Hello keresési mezőbe, írja be a **SilkRoad élettartama Suite**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-134">In hello search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="e6ffb-136">Hello eredmények ablaktábláján jelöljön ki **SilkRoad élettartama Suite**, és kattintson a **Complete** tooadd hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-136">In hello results pane, select **SilkRoad Life Suite**, and then click **Complete** tooadd hello application.</span></span>
   
    ![Alkalmazások][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="e6ffb-138">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="e6ffb-139">hello ebben a szakaszban célja tooshow hogyan tooconfigure és tesztelési Azure AD SSO élettartama Suite SilkRoad alapján "Britta Simon" nevű tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-139">hello objective of this section is tooshow you how tooconfigure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="e6ffb-140">Az SSO toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SilkRoad élettartama Suite tooan felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-140">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SilkRoad Life Suite tooan user in Azure AD is.</span></span> <span data-ttu-id="e6ffb-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello SilkRoad élettartama csomagban található hivatkozás kapcsolatának kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-141">In other words, a link relationship between an Azure AD user and hello related user in SilkRoad Life Suite needs toobe established.</span></span>

<span data-ttu-id="e6ffb-142">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** SilkRoad élettartama csomagban található.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-142">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="e6ffb-143">tooconfigure és tesztelése az Azure AD az egyszeri bejelentkezés SilkRoad élettartama csomaggal, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-143">tooconfigure and test Azure AD single sign-on with SilkRoad Life Suite, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="e6ffb-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="e6ffb-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="e6ffb-146">**[SilkRoad élettartama Suite tesztfelhasználó létrehozása](#creating-a-silkroad-life-suite-test-user)**  -toohave Britta Simon SilkRoad élettartama Suite, amely az Azure AD csatolt toohello ábrázolása rá, hogy valami.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - toohave a counterpart of Britta Simon in SilkRoad Life Suite that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="e6ffb-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="e6ffb-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-148">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="e6ffb-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="e6ffb-150">hello ebben a szakaszban célja a klasszikus Azure portálon hello Azure AD SSO tooenable és tooconfigure SSO élettartama Suite SilkRoad alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-150">hello objective of this section is tooenable Azure AD SSO in hello Azure classic portal and tooconfigure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="e6ffb-151">**tooconfigure az Azure AD az egyszeri bejelentkezés SilkRoad élettartama csomaggal, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e6ffb-151">**tooconfigure Azure AD single sign-on with SilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="e6ffb-152">Bejelentkezés tooyour SilkRoad vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-152">Sign-on tooyour SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="e6ffb-153">tooobtain hozzáférés toohello SilkRoad élettartama Suite hitelesítési kérelmet a Microsoft Azure AD-összevonás konfigurálása forduljon SilkRoad támogatási vagy az Ön SilkRoad szolgáltatások képviselőjével.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-153">tooobtain access toohello SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="e6ffb-154">Nyissa meg túl**szolgáltató**, és kattintson a **összevonási részletek**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-154">Go too**Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][10] 

3. <span data-ttu-id="e6ffb-156">Kattintson a **töltse le az összevonási metaadatok**, majd mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-156">Click **Download Federation Metadata**, and then save hello metadata file on your computer.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][11] 

4. <span data-ttu-id="e6ffb-158">A klasszikus Azure portálon, a hello hello **SilkRoad élettartama Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** tooopen hello **konfigurálása egyszeri bejelentkezéshez**  párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-158">In hello Azure classic portal, on hello **SilkRoad Life Suite** application integration page, click **Configure single sign-on** tooopen hello **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 

5. <span data-ttu-id="e6ffb-160">A hello **hogyan szeretné tooSilkRoad élettartama Suite a felhasználók toosign** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-160">On hello **How would you like users toosign on tooSilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 

6. <span data-ttu-id="e6ffb-162">A hello **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-162">On hello **Configure App Settings** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][8]   
 1. <span data-ttu-id="e6ffb-164">A hello **URL-cím bejelentkezési** szövegmező, a felhasználók toosign tooyour SilkRoad élettartama Suite webhely által használt típus hello URL (pl.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-164">In hello **Sign On URL** textbox, type hello URL used by your users toosign-on tooyour SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="e6ffb-165">Nyissa meg hello letöltött **Silkroad** metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-165">Open hello downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="e6ffb-166">Keresse meg a hello **AssertionConsumerService** címkét, majd a Másolás hello **hely** attribútum.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-166">Locate hello **AssertionConsumerService** tag, and then copy hello **Location** attribute.</span></span>         
   
    ![Az Azure AD-egyszeri bejelentkezés][21] 
 4. <span data-ttu-id="e6ffb-168">Hello értéket beilleszteni hello **válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-168">Paste hello value into hello **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="e6ffb-169">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-169">Click **Next**.</span></span>

6. <span data-ttu-id="e6ffb-170">A hello **konfigurálhatja az egyszeri bejelentkezés SilkRoad élettartama Suite** lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-170">On hello **Configure single sign-on at SilkRoad Life Suite** page, perform hello following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]  
 1. <span data-ttu-id="e6ffb-172">Kattintson a letöltés tanúsítványt, és mentse a hello fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-172">Click Download certificate, and then save hello file on your computer.</span></span>  
 2. <span data-ttu-id="e6ffb-173">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-173">Click **Next**.</span></span>

7. <span data-ttu-id="e6ffb-174">Az a **SilkRoad** alkalmazás, kattintson a **hitelesítési források**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][12] 

8. <span data-ttu-id="e6ffb-176">Kattintson a **hitelesítési forrás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-176">Click **Add Authentication Source**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][13] 

9. <span data-ttu-id="e6ffb-178">A hello **hitelesítési forrás hozzáadása** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-178">In hello **Add Authentication Source** section, perform hello following steps:</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][14]  
 1. <span data-ttu-id="e6ffb-180">A **beállítás 2 - metaadatfájl**, kattintson a **Tallózás** tooupload hello letöltött metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-180">Under **Option 2 - Metadata File**, click **Browse** tooupload hello downloaded metadata file.</span></span>  
 2. <span data-ttu-id="e6ffb-181">Kattintson a **fájladatok használatával hozzon létre identitásszolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="e6ffb-182">A hello **hitelesítési források** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-182">In hello **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][15] 

11. <span data-ttu-id="e6ffb-184">A hello **szerkesztése hitelesítési forrás** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-184">On hello **Edit Authentication Source** dialog, perform hello following steps:</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][16] 
 1. <span data-ttu-id="e6ffb-186">Mint **engedélyezve**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="e6ffb-187">A hello **IdP leírás** szövegmező, adja meg a konfiguráció leírása (pl.: *Azure AD SSO*).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-187">In hello **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="e6ffb-188">A hello **IdP neve** szövegmező, írja be, amelyek adott tooyour konfiguráció nevét (pl.: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="e6ffb-188">In hello **IdP Name** textbox, type a name that is specific tooyour configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="e6ffb-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-189">Click **Save**.</span></span>

12. <span data-ttu-id="e6ffb-190">Tiltsa le a többi hitelesítési forrást.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-190">Disable all other authentication sources.</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][17]

13. <span data-ttu-id="e6ffb-192">A klasszikus Azure portálon, a hello hello **az egyszeri bejelentkezés megerősítő** lapján kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-192">In hello Azure classic portal, on hello **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Az Azure AD-egyszeri bejelentkezés][18]

14. <span data-ttu-id="e6ffb-194">A hello **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-194">On hello **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Az Azure AD-egyszeri bejelentkezés][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="e6ffb-196">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="e6ffb-196">Create an Azure AD test user</span></span>
<span data-ttu-id="e6ffb-197">hello ebben a szakaszban célja toocreate hello Britta Simon neve a klasszikus Azure portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-197">hello objective of this section is toocreate a test user in hello Azure classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="e6ffb-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="e6ffb-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="e6ffb-200">A hello **a klasszikus Azure portálon**, a hello bal oldali navigációs panelen, kattintson a **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-200">In hello **Azure classic portal**, on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="e6ffb-202">A hello **Directory** listában, jelölje be hello directory kívánt tooenable címtár-integráció.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-202">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>

3. <span data-ttu-id="e6ffb-203">toodisplay hello azoknak a felhasználóknak, hello menüben található hello felső részén kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-203">toodisplay hello list of users, in hello menu on hello top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="e6ffb-205">tooopen hello **felhasználó hozzáadása** párbeszédpanelen hello eszköztár hello alján, kattintson a **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-205">tooopen hello **Add User** dialog, in hello toolbar on hello bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="e6ffb-207">A hello **adja meg azt a felhasználó** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-207">On hello **Tell us about this user** dialog page, perform hello following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="e6ffb-209">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="e6ffb-210">A felhasználónév hello **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-210">In hello User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="e6ffb-211">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-211">Click **Next**.</span></span>

6. <span data-ttu-id="e6ffb-212">A hello **felhasználói profil** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-212">On hello **User Profile** dialog page, perform hello following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="e6ffb-214">A hello **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-214">In hello **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="e6ffb-215">A hello **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-215">In hello **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="e6ffb-216">A hello **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-216">In hello **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="e6ffb-217">A hello **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-217">In hello **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="e6ffb-218">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-218">Click **Next**.</span></span>

7. <span data-ttu-id="e6ffb-219">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-219">On hello **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="e6ffb-221">A hello **ideiglenes jelszó beszerzése** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="e6ffb-221">On hello **Get temporary password** dialog page, perform hello following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="e6ffb-223">Írja le hello hello értékének **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-223">Write down hello value of hello **New Password**.</span></span> 
 2. <span data-ttu-id="e6ffb-224">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="e6ffb-225">SilkRoad élettartama Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="e6ffb-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="e6ffb-226">hello ebben a szakaszban célja toocreate Britta Simon SilkRoad élettartama Suite nevű felhasználó.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-226">hello objective of this section is toocreate a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="e6ffb-227">Britta tartozó egyszeri bejelentkezési Azonosítóval kell rendelkeznie (hívják tooas egy *AuthParam*), amely megfelel a Britta **emailaddress** Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-227">Britta's must have an SSO ID (sometimes referred tooas an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="e6ffb-228">**toocreate Britta Simon meghívta SilkRoad élettartama Suite, a felhasználó hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e6ffb-228">**toocreate a user called Britta Simon in SilkRoad Life Suite, perform hello following steps:**</span></span>

- <span data-ttu-id="e6ffb-229">Kérje meg a SilkRoad élettartama Suite támogatási csoport toocreate, amely rendelkezik a felhasználó **egyszeri bejelentkezési azonosító** attribútum hello ugyanaz, mint hello érték **emailaddress** a Britta Simon Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-229">Ask your SilkRoad Life Suite support team toocreate a user that has as **SSO ID** attribute hello same value as hello **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="e6ffb-230">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="e6ffb-230">Assign hello Azure AD test user</span></span>
<span data-ttu-id="e6ffb-231">hello ebben a szakaszban célja tooenable Britta Simon toouse Azure SSO saját hozzáférés tooSilkRoad élettartama Suite megadásával.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-231">hello objective of this section is tooenable Britta Simon toouse Azure SSO by granting her access tooSilkRoad Life Suite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="e6ffb-233">**tooassign Britta Simon tooSilkRoad élettartama Suite, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="e6ffb-233">**tooassign Britta Simon tooSilkRoad Life Suite, perform hello following steps:**</span></span>

1. <span data-ttu-id="e6ffb-234">A hello Azure klasszikus portál tooopen hello alkalmazások megtekintése, hello könyvtár nézetben kattintson **alkalmazások** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-234">On hello Azure classic portal, tooopen hello applications view, in hello directory view, click **Applications** in hello top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="e6ffb-236">Hello alkalmazások listában válassza ki a **SilkRoad élettartama Suite**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-236">In hello applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Felhasználó hozzárendelése][202] 

3. <span data-ttu-id="e6ffb-238">Hello hello felső menüben kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-238">In hello menu on hello top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 

4. <span data-ttu-id="e6ffb-240">Hello felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-240">In hello Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="e6ffb-241">Hello alján hello eszköztárában kattintson **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-241">In hello toolbar on hello bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="e6ffb-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="e6ffb-243">Test single sign-on</span></span>
<span data-ttu-id="e6ffb-244">hello ebben a szakaszban célja tootest hozzáférési Panel az Azure AD SSO konfigurációs használatával hello.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-244">hello objective of this section is tootest your Azure AD SSO configuration using hello Access Panel.</span></span>  

<span data-ttu-id="e6ffb-245">Ha a hozzáférési Panel hello hello SilkRoad élettartama Suite csempe gombra kattint, automatikusan bejelentkezett tooyour SilkRoad élettartama Suite alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="e6ffb-245">When you click hello SilkRoad Life Suite tile in hello Access Panel, you should get automatically signed-on tooyour SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e6ffb-246">További források</span><span class="sxs-lookup"><span data-stu-id="e6ffb-246">Additional Resources</span></span>
* [<span data-ttu-id="e6ffb-247">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="e6ffb-247">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="e6ffb-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="e6ffb-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_01.png
[50]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_02.png

[6]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_05.png
[7]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_03.png
[8]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_04.png
[9]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_05.png
[10]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_06.png
[11]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_07.png
[12]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_08.png
[13]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_09.png
[14]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_10.png
[15]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_11.png
[16]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_12.png
[17]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_13.png
[18]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_06.png
[19]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_07.png


[20]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_100.png
[21]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_15.png


[200]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_silkroad_14.png
[203]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-silkroad-life-suite-tutorial/tutorial_general_205.png





