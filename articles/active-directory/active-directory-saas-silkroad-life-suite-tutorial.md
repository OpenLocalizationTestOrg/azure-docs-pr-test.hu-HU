---
title: "Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama Suite |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SilkRoad élettartama Suite között."
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
ms.openlocfilehash: ecf4e31ecea00d003fc47ea4cebb781ca58957f7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a><span data-ttu-id="eccf6-103">Oktatóanyag: Azure Active Directory-integráció SilkRoad élettartama csomaggal</span><span class="sxs-lookup"><span data-stu-id="eccf6-103">Tutorial: Azure Active Directory integration with SilkRoad Life Suite</span></span>
<span data-ttu-id="eccf6-104">Ez az oktatóanyag célja SilkRoad élettartama Suite integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="eccf6-104">The objective of this tutorial is to show you how to integrate SilkRoad Life Suite with Azure Active Directory (Azure AD).</span></span> 

<span data-ttu-id="eccf6-105">SilkRoad élettartama Suite integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="eccf6-105">Integrating SilkRoad Life Suite with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="eccf6-106">Megadhatja a SilkRoad élettartama Suite hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="eccf6-106">You can control in Azure AD who has access to SilkRoad Life Suite</span></span> 
* <span data-ttu-id="eccf6-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a SilkRoad élettartama Suite egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="eccf6-107">You can enable your users to automatically get signed-on to SilkRoad Life Suite single sign-on (SSO) with their Azure AD accounts</span></span>

<span data-ttu-id="eccf6-108">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="eccf6-108">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="eccf6-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="eccf6-109">Prerequisites</span></span>
<span data-ttu-id="eccf6-110">Az Azure AD-integráció konfigurálása SilkRoad élettartama csomaggal, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="eccf6-110">To configure Azure AD integration with SilkRoad Life Suite, you need the following items:</span></span>

* <span data-ttu-id="eccf6-111">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="eccf6-111">An Azure AD subscription</span></span>
* <span data-ttu-id="eccf6-112">Egy SilkRoad élettartama Suite SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="eccf6-112">A SilkRoad Life Suite SSO enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="eccf6-113">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-113">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="eccf6-114">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="eccf6-114">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="eccf6-115">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="eccf6-115">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="eccf6-116">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="eccf6-116">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="eccf6-117">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="eccf6-117">Scenario Description</span></span>
<span data-ttu-id="eccf6-118">Ez az oktatóanyag célja ahhoz, hogy az Azure AD SSO teszteléséhez tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-118">The objective of this tutorial is to enable you to test Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="eccf6-119">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="eccf6-119">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="eccf6-120">A gyűjteményből SilkRoad élettartama csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eccf6-120">Adding SilkRoad Life Suite from the gallery</span></span> 
2. <span data-ttu-id="eccf6-121">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eccf6-121">Configuring and testing Azure AD SSO</span></span>

## <a name="add-silkroad-life-suite-from-the-gallery"></a><span data-ttu-id="eccf6-122">A gyűjteményből SilkRoad élettartama csomag hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eccf6-122">Add SilkRoad Life Suite from the gallery</span></span>
<span data-ttu-id="eccf6-123">Az Azure AD integrálása a SilkRoad élettartama Suite konfigurálásához kell hozzáadnia SilkRoad élettartama Suite a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="eccf6-123">To configure the integration of SilkRoad Life Suite into Azure AD, you need to add SilkRoad Life Suite from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="eccf6-124">**A gyűjteményből SilkRoad élettartama Suite hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eccf6-124">**To add SilkRoad Life Suite from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="eccf6-125">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-125">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="eccf6-127">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="eccf6-127">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="eccf6-128">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-128">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="eccf6-130">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="eccf6-130">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="eccf6-132">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-132">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="eccf6-134">Írja be a keresőmezőbe, **SilkRoad élettartama Suite**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-134">In the search box, type **SilkRoad Life Suite**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="eccf6-136">Az eredmények ablaktáblájában válassza **SilkRoad élettartama Suite**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="eccf6-136">In the results pane, select **SilkRoad Life Suite**, and then click **Complete** to add the application.</span></span>
   
    ![Alkalmazások][50]

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="eccf6-138">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eccf6-138">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="eccf6-139">Ez a szakasz célja bemutatják a Azure AD SSO SilkRoad élettartama Suite "Britta Simon" nevű tesztfelhasználó alapján tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="eccf6-139">The objective of this section is to show you how to configure and test Azure AD SSO with SilkRoad Life Suite based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="eccf6-140">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a partner felhasználó SilkRoad élettartama csomagban található egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="eccf6-140">For SSO to work, Azure AD needs to know what the counterpart user in SilkRoad Life Suite to an user in Azure AD is.</span></span> <span data-ttu-id="eccf6-141">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó SilkRoad élettartama csomagban található hivatkozás kapcsolatának kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="eccf6-141">In other words, a link relationship between an Azure AD user and the related user in SilkRoad Life Suite needs to be established.</span></span>

<span data-ttu-id="eccf6-142">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** SilkRoad élettartama csomagban található.</span><span class="sxs-lookup"><span data-stu-id="eccf6-142">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SilkRoad Life Suite.</span></span>

<span data-ttu-id="eccf6-143">Az Azure AD az egyszeri bejelentkezés SilkRoad élettartama Suite tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="eccf6-143">To configure and test Azure AD single sign-on with SilkRoad Life Suite, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="eccf6-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="eccf6-144">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="eccf6-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="eccf6-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="eccf6-146">**[SilkRoad élettartama Suite tesztfelhasználó létrehozása](#creating-a-silkroad-life-suite-test-user)**  - való egy megfelelője a Britta Simon SilkRoad élettartama csomag, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="eccf6-146">**[Creating a SilkRoad Life Suite test user](#creating-a-silkroad-life-suite-test-user)** - to have a counterpart of Britta Simon in SilkRoad Life Suite that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="eccf6-147">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="eccf6-147">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="eccf6-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="eccf6-148">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="eccf6-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eccf6-149">Configure Azure AD single sign-on</span></span>
<span data-ttu-id="eccf6-150">Ez a szakasz célja engedélyezze az Azure AD egyszeri Bejelentkezést a klasszikus Azure portálon és egyszeri bejelentkezés konfigurálása az SilkRoad élettartama Suite alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="eccf6-150">The objective of this section is to enable Azure AD SSO in the Azure classic portal and to configure SSO in your SilkRoad Life Suite application.</span></span>

<span data-ttu-id="eccf6-151">**Konfigurálja az Azure AD az egyszeri bejelentkezés SilkRoad élettartama csomaggal, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eccf6-151">**To configure Azure AD single sign-on with SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="eccf6-152">Bejelentkezés a SilkRoad vállalati webhely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="eccf6-152">Sign-on to your SilkRoad company site as administrator.</span></span> 

  >[!NOTE] 
  > <span data-ttu-id="eccf6-153">A Microsoft Azure AD-összevonás konfigurálása az SilkRoad élettartama Suite hitelesítési alkalmazáshoz való hozzáférési, forduljon SilkRoad támogatási vagy az Ön SilkRoad szolgáltatások képviselőjével.</span><span class="sxs-lookup"><span data-stu-id="eccf6-153">To obtain access to the SilkRoad Life Suite Authentication application for configuring federation with Microsoft Azure AD, please contact SilkRoad Support or your SilkRoad Services representative.</span></span>
  > 

2. <span data-ttu-id="eccf6-154">Ugrás a **szolgáltató**, és kattintson a **összevonási részletek**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-154">Go to **Service Provider**, and then click **Federation Details**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][10] 

3. <span data-ttu-id="eccf6-156">Kattintson a **töltse le az összevonási metaadatok**, majd mentse a metaadatok fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="eccf6-156">Click **Download Federation Metadata**, and then save the metadata file on your computer.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][11] 

4. <span data-ttu-id="eccf6-158">A klasszikus Azure portálon a a **SilkRoad élettartama Suite** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="eccf6-158">In the Azure classic portal, on the **SilkRoad Life Suite** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 

5. <span data-ttu-id="eccf6-160">Az a **hová bejelentkezni SilkRoad élettartama Suite felhasználók** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-160">On the **How would you like users to sign on to SilkRoad Life Suite** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][7] 

6. <span data-ttu-id="eccf6-162">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="eccf6-162">On the **Configure App Settings** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][8]   
 1. <span data-ttu-id="eccf6-164">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés SilkRoad élettartama Suite webhelyekhez (pl.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span><span class="sxs-lookup"><span data-stu-id="eccf6-164">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your SilkRoad Life Suite site (e.g.: *https://defcompanytest-test-redcarpet.silkroad-eng.com/Authentication/*).</span></span>  
 2. <span data-ttu-id="eccf6-165">Nyissa meg a letöltött **Silkroad** metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="eccf6-165">Open the downloaded **Silkroad** metadata file.</span></span> 
 3. <span data-ttu-id="eccf6-166">Keresse meg a **AssertionConsumerService** címkét, és másolja a **hely** attribútum.</span><span class="sxs-lookup"><span data-stu-id="eccf6-166">Locate the **AssertionConsumerService** tag, and then copy the **Location** attribute.</span></span>         
   
    ![Az Azure AD-egyszeri bejelentkezés][21] 
 4. <span data-ttu-id="eccf6-168">Illessze be az érték a **válasz URL-CÍMEN** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="eccf6-168">Paste the value into the **Reply URL** textbox.</span></span>  
 5. <span data-ttu-id="eccf6-169">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-169">Click **Next**.</span></span>

6. <span data-ttu-id="eccf6-170">Az a **konfigurálhatja az egyszeri bejelentkezés SilkRoad élettartama Suite** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="eccf6-170">On the **Configure single sign-on at SilkRoad Life Suite** page, perform the following steps:</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]  
 1. <span data-ttu-id="eccf6-172">Kattintson a letöltés tanúsítványt, és mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="eccf6-172">Click Download certificate, and then save the file on your computer.</span></span>  
 2. <span data-ttu-id="eccf6-173">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-173">Click **Next**.</span></span>

7. <span data-ttu-id="eccf6-174">Az a **SilkRoad** alkalmazás, kattintson a **hitelesítési források**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-174">In your **SilkRoad** application, click **Authentication Sources**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][12] 

8. <span data-ttu-id="eccf6-176">Kattintson a **hitelesítési forrás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-176">Click **Add Authentication Source**.</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][13] 

9. <span data-ttu-id="eccf6-178">Az a **hitelesítési forrás hozzáadása** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="eccf6-178">In the **Add Authentication Source** section, perform the following steps:</span></span> 
   
    ![Az Azure AD-egyszeri bejelentkezés][14]  
 1. <span data-ttu-id="eccf6-180">A **beállítás 2 - metaadatfájl**, kattintson a **Tallózás** feltölteni a fájlt a letöltött metaadat.</span><span class="sxs-lookup"><span data-stu-id="eccf6-180">Under **Option 2 - Metadata File**, click **Browse** to upload the downloaded metadata file.</span></span>  
 2. <span data-ttu-id="eccf6-181">Kattintson a **fájladatok használatával hozzon létre identitásszolgáltató**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-181">Click **Create Identity Provider using File Data**.</span></span>

10. <span data-ttu-id="eccf6-182">Az a **hitelesítési források** kattintson **szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-182">In the **Authentication Sources** section, click **Edit**.</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][15] 

11. <span data-ttu-id="eccf6-184">Az a **szerkesztése hitelesítési forrás** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="eccf6-184">On the **Edit Authentication Source** dialog, perform the following steps:</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][16] 
 1. <span data-ttu-id="eccf6-186">Mint **engedélyezve**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-186">As **Enabled**, select **Yes**.</span></span>   
 2. <span data-ttu-id="eccf6-187">Az a **IdP leírás** szövegmező, adja meg a konfiguráció leírása (pl.: *Azure AD SSO*).</span><span class="sxs-lookup"><span data-stu-id="eccf6-187">In the **IdP Description** textbox, type a description for your configuration (e.g.: *Azure AD SSO*).</span></span>  
 3. <span data-ttu-id="eccf6-188">Az a **IdP neve** szövegmező, írja be a konfigurációs adott nevét (pl.: *Azure SP*).</span><span class="sxs-lookup"><span data-stu-id="eccf6-188">In the **IdP Name** textbox, type a name that is specific to your configuration (e.g.: *Azure SP*).</span></span>  
 4. <span data-ttu-id="eccf6-189">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-189">Click **Save**.</span></span>

12. <span data-ttu-id="eccf6-190">Tiltsa le a többi hitelesítési forrást.</span><span class="sxs-lookup"><span data-stu-id="eccf6-190">Disable all other authentication sources.</span></span> 
    
     ![Az Azure AD-egyszeri bejelentkezés][17]

13. <span data-ttu-id="eccf6-192">A klasszikus Azure portálon a a **az egyszeri bejelentkezés megerősítő** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-192">In the Azure classic portal, on the **Single sign-on confirmation** page, click **Next**.</span></span>  
    
     ![Az Azure AD-egyszeri bejelentkezés][18]

14. <span data-ttu-id="eccf6-194">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-194">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
    
     ![Az Azure AD-egyszeri bejelentkezés][19]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="eccf6-196">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eccf6-196">Create an Azure AD test user</span></span>
<span data-ttu-id="eccf6-197">Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="eccf6-197">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="eccf6-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eccf6-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="eccf6-200">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-200">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_09.png)  

2. <span data-ttu-id="eccf6-202">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="eccf6-202">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="eccf6-203">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-203">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="eccf6-205">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-205">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="eccf6-207">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="eccf6-207">On the **Tell us about this user** dialog page, perform the following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_05.png)  
 1. <span data-ttu-id="eccf6-209">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="eccf6-209">As Type Of User, select New user in your organization.</span></span>  
 2. <span data-ttu-id="eccf6-210">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-210">In the User Name **textbox**, type **BrittaSimon**.</span></span> 
 3. <span data-ttu-id="eccf6-211">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-211">Click **Next**.</span></span>

6. <span data-ttu-id="eccf6-212">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="eccf6-212">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_06.png)  
 1. <span data-ttu-id="eccf6-214">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-214">In the **First Name** textbox, type **Britta**.</span></span>    
 2. <span data-ttu-id="eccf6-215">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-215">In the **Last Name** textbox, type, **Simon**.</span></span> 
 3. <span data-ttu-id="eccf6-216">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-216">In the **Display Name** textbox, type **Britta Simon**.</span></span> 
 4. <span data-ttu-id="eccf6-217">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-217">In the **Role** list, select **User**.</span></span>
 5. <span data-ttu-id="eccf6-218">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-218">Click **Next**.</span></span>

7. <span data-ttu-id="eccf6-219">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-219">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="eccf6-221">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="eccf6-221">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-silkroad-life-suite-tutorial/create_aaduser_08.png)  
 1. <span data-ttu-id="eccf6-223">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-223">Write down the value of the **New Password**.</span></span> 
 2. <span data-ttu-id="eccf6-224">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="eccf6-224">Click **Complete**.</span></span>   

### <a name="create-a-silkroad-life-suite-test-user"></a><span data-ttu-id="eccf6-225">SilkRoad élettartama Suite tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="eccf6-225">Create a SilkRoad Life Suite test user</span></span>
<span data-ttu-id="eccf6-226">Ez a szakasz célja Britta Simon SilkRoad élettartama Suite nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="eccf6-226">The objective of this section is to create a user called Britta Simon in SilkRoad Life Suite.</span></span> <span data-ttu-id="eccf6-227">Britta tartozó egyszeri bejelentkezési Azonosítóval kell rendelkeznie (más néven egy *AuthParam*), amely megfelel a Britta **emailaddress** Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-227">Britta's must have an SSO ID (sometimes referred to as an *AuthParam*) that matches Britta's **emailaddress** in Azure AD.</span></span>

<span data-ttu-id="eccf6-228">**A felhasználó Britta Simon SilkRoad élettartama Suite nevű létrehozásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eccf6-228">**To create a user called Britta Simon in SilkRoad Life Suite, perform the following steps:**</span></span>

- <span data-ttu-id="eccf6-229">Kérje meg a SilkRoad élettartama Suite támogatási csoport, amely rendelkezik felhasználó létrehozásához **egyszeri bejelentkezési azonosító** attribútum értéke megegyezik a **emailaddress** a Britta Simon Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-229">Ask your SilkRoad Life Suite support team to create a user that has as **SSO ID** attribute the same value as the **emailaddress** of Britta Simon in Azure AD.</span></span>

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="eccf6-230">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="eccf6-230">Assign the Azure AD test user</span></span>
<span data-ttu-id="eccf6-231">Ez a szakasz célja Britta Simon Azure SSO saját hozzáférést biztosít SilkRoad élettartama Suite által használandó engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="eccf6-231">The objective of this section is to enable Britta Simon to use Azure SSO by granting her access to SilkRoad Life Suite.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="eccf6-233">**Britta Simon hozzárendelése SilkRoad élettartama Suite, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="eccf6-233">**To assign Britta Simon to SilkRoad Life Suite, perform the following steps:**</span></span>

1. <span data-ttu-id="eccf6-234">A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="eccf6-234">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="eccf6-236">Az alkalmazások listában válassza ki a **SilkRoad élettartama Suite**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-236">In the applications list, select **SilkRoad Life Suite**.</span></span>
   
    ![Felhasználó hozzárendelése][202] 

3. <span data-ttu-id="eccf6-238">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-238">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 

4. <span data-ttu-id="eccf6-240">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-240">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="eccf6-241">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="eccf6-241">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="eccf6-243">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="eccf6-243">Test single sign-on</span></span>
<span data-ttu-id="eccf6-244">Ez a szakasz célja a hozzáférési panelen az Azure AD SSO-konfigurációjának tesztelése.</span><span class="sxs-lookup"><span data-stu-id="eccf6-244">The objective of this section is to test your Azure AD SSO configuration using the Access Panel.</span></span>  

<span data-ttu-id="eccf6-245">Ha a hozzáférési panelen SilkRoad élettartama Suite csempére kattint, meg kell beolvasása automatikusan bejelentkezett az SilkRoad élettartama Suite alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="eccf6-245">When you click the SilkRoad Life Suite tile in the Access Panel, you should get automatically signed-on to your SilkRoad Life Suite application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eccf6-246">További források</span><span class="sxs-lookup"><span data-stu-id="eccf6-246">Additional Resources</span></span>
* [<span data-ttu-id="eccf6-247">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="eccf6-247">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="eccf6-248">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="eccf6-248">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

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





