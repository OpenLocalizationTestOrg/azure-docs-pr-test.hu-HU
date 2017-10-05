---
title: "Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és SciQuest töltött igazgató között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 9fab641b-292e-4bef-91d1-8ccc4f3a0c1f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 84b707668dc45e92e6151f422f1c919f638533b1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sciquest-spend-director"></a><span data-ttu-id="9900f-103">Oktatóanyag: Azure Active Directoryval integrált SciQuest töltött igazgató</span><span class="sxs-lookup"><span data-stu-id="9900f-103">Tutorial: Azure Active Directory integration with SciQuest Spend Director</span></span>
<span data-ttu-id="9900f-104">Ez az oktatóanyag célja SciQuest töltött igazgató integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="9900f-104">The objective of this tutorial is to show you how to integrate SciQuest Spend Director with Azure Active Directory (Azure AD).</span></span>  
<span data-ttu-id="9900f-105">SciQuest töltött igazgató integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="9900f-105">Integrating SciQuest Spend Director with Azure AD provides you with the following benefits:</span></span> 

* <span data-ttu-id="9900f-106">Megadhatja a SciQuest töltött igazgató hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="9900f-106">You can control in Azure AD who has access to SciQuest Spend Director</span></span> 
* <span data-ttu-id="9900f-107">Az Azure AD-fiókok a engedélyezheti a felhasználóknak, hogy automatikusan lekérni bejelentkezett SciQuest töltött igazgató (egyszeri bejelentkezés)</span><span class="sxs-lookup"><span data-stu-id="9900f-107">You can enable your users to automatically get signed-on to SciQuest Spend Director (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="9900f-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="9900f-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="9900f-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="9900f-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9900f-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="9900f-110">Prerequisites</span></span>
<span data-ttu-id="9900f-111">Konfigurálása az Azure AD-integrációs SciQuest töltött igazgató, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="9900f-111">To configure Azure AD integration with SciQuest Spend Director, you need the following items:</span></span>

* <span data-ttu-id="9900f-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="9900f-112">An Azure AD subscription</span></span>
* <span data-ttu-id="9900f-113">Egy SciQuest töltött igazgató egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="9900f-113">A SciQuest Spend Director single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="9900f-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="9900f-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="9900f-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="9900f-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="9900f-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="9900f-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="9900f-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9900f-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span> 

## <a name="scenario-description"></a><span data-ttu-id="9900f-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="9900f-118">Scenario Description</span></span>
<span data-ttu-id="9900f-119">Ez az oktatóanyag célja ahhoz, hogy egy tesztkörnyezetben az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="9900f-119">The objective of this tutorial is to enable you to test Azure AD single sign-on in a test environment.</span></span>  
<span data-ttu-id="9900f-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="9900f-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="9900f-121">A gyűjteményből SciQuest töltött igazgató hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9900f-121">Adding SciQuest Spend Director from the gallery</span></span> 
2. <span data-ttu-id="9900f-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9900f-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sciquest-spend-director-from-the-gallery"></a><span data-ttu-id="9900f-123">A gyűjteményből SciQuest töltött igazgató hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9900f-123">Adding SciQuest Spend Director from the gallery</span></span>
<span data-ttu-id="9900f-124">Az Azure AD integrálása a SciQuest töltött igazgató konfigurálásához kell hozzáadnia SciQuest töltött igazgató a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="9900f-124">To configure the integration of SciQuest Spend Director into Azure AD, you need to add SciQuest Spend Director from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="9900f-125">**A gyűjteményből SciQuest töltött igazgató hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9900f-125">**To add SciQuest Spend Director from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="9900f-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9900f-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]

2. <span data-ttu-id="9900f-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="9900f-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="9900f-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="9900f-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]

4. <span data-ttu-id="9900f-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="9900f-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]

5. <span data-ttu-id="9900f-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="9900f-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]

6. <span data-ttu-id="9900f-135">Írja be a keresőmezőbe, **sciQuest töltött igazgató**.</span><span class="sxs-lookup"><span data-stu-id="9900f-135">In the search box, type **sciQuest spend director**.</span></span>
   
    ![Alkalmazások][5]

7. <span data-ttu-id="9900f-137">Az eredmények ablaktáblájában válassza **SciQuest töltött igazgató**, és kattintson a **Complete** az alkalmazás hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="9900f-137">In the results pane, select **SciQuest Spend Director**, and then click **Complete** to add the application.</span></span>
   
    ![Alkalmazások][6]

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="9900f-139">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="9900f-139">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="9900f-140">Ez a szakasz célja bemutatják a konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés SciQuest töltött igazgató "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="9900f-140">The objective of this section is to show you how to configure and test Azure AD single sign-on with SciQuest Spend Director based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="9900f-141">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó SciQuest töltött igazgató egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="9900f-141">For single sign-on to work, Azure AD needs to know what the counterpart user in SciQuest Spend Director to an user in Azure AD is.</span></span> <span data-ttu-id="9900f-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó SciQuest töltött Director közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9900f-142">In other words, a link relationship between an Azure AD user and the related user in SciQuest Spend Director needs to be established.</span></span>  
<span data-ttu-id="9900f-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** SciQuest töltött Director.</span><span class="sxs-lookup"><span data-stu-id="9900f-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in SciQuest Spend Director.</span></span>

<span data-ttu-id="9900f-144">Az Azure AD egyszeri bejelentkezést a SciQuest töltött igazgató tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="9900f-144">To configure and test Azure AD single sign-on with SciQuest Spend Director, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="9900f-145">**[Az Azure AD egy egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="9900f-145">**[Configuring Azure AD Single Single Sign-On](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="9900f-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="9900f-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="9900f-147">**[SciQuest töltött igazgató tesztfelhasználó létrehozása](#creating-a-halogen-software-test-user)**  - való egy megfelelője a Britta Simon SciQuest töltött igazgató, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="9900f-147">**[Creating a SciQuest Spend Director test user](#creating-a-halogen-software-test-user)** - to have a counterpart of Britta Simon in SciQuest Spend Director that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="9900f-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9900f-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="9900f-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="9900f-149">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-single-sign-on"></a><span data-ttu-id="9900f-150">Az Azure AD egy egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9900f-150">Configuring Azure AD Single Single Sign-On</span></span>
<span data-ttu-id="9900f-151">Ez a szakasz célja engedélyezése az Azure AD az egyszeri bejelentkezés a klasszikus Azure portálon, és konfigurálása egyszeri bejelentkezéshez az SciQuest töltött igazgató alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="9900f-151">The objective of this section is to enable Azure AD single sign-on in the Azure classic portal and to configure single sign-on in your SciQuest Spend Director application.</span></span>

<span data-ttu-id="9900f-152">**Az Azure AD az egyszeri bejelentkezés konfigurálása SciQuest töltött igazgató, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9900f-152">**To configure Azure AD single sign-on with SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="9900f-153">A klasszikus Azure portálon a a **SciQuest töltött igazgató** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9900f-153">In the Azure classic portal, on the **SciQuest Spend Director** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][8]

2. <span data-ttu-id="9900f-155">A a **hová bejelentkezni SciQuest töltött igazgató felhasználók** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="9900f-155">On the **How would you like users to sign on to SciQuest Spend Director** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][9]

3. <span data-ttu-id="9900f-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9900f-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span> 
   
    ![Alkalmazásbeállítások konfigurálása][10]
   
     <span data-ttu-id="9900f-159">a.</span><span class="sxs-lookup"><span data-stu-id="9900f-159">a.</span></span> <span data-ttu-id="9900f-160">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-cím segítségével a felhasználók jelentkezzen be a SciQuest töltött igazgató alkalmazást a következő mintát: *https://.* sciquest.com/.**</span><span class="sxs-lookup"><span data-stu-id="9900f-160">In the **Sign On URL** textbox, type your URL used by your users to sign on to your SciQuest Spend Director application using the following pattern: *https://.*sciquest.com/.**</span></span>
   
     <span data-ttu-id="9900f-161">b.</span><span class="sxs-lookup"><span data-stu-id="9900f-161">b.</span></span> <span data-ttu-id="9900f-162">Az a **válasz URL-CÍMEN** szövegmezőhöz beírt ugyanazt az értéket írja be a **URL-cím bejelentkezési** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="9900f-162">In the **Reply URL** textbox, type the same value you have typed into the **Sign On URL** textbox.</span></span> 
   
     <span data-ttu-id="9900f-163">c.</span><span class="sxs-lookup"><span data-stu-id="9900f-163">c.</span></span> <span data-ttu-id="9900f-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9900f-164">Click **Next**.</span></span>

4. <span data-ttu-id="9900f-165">A a **konfigurálhatja az egyszeri bejelentkezés SciQuest töltött igazgató** lapján kattintson **metaadatok letöltése**, és mentse helyileg a számítógépen a metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="9900f-165">On the **Configure single sign-on at SciQuest Spend Director** page, click **Download metadata**, and then save the metadata file locally on your computer.</span></span>
   
    ![Mi az az Azure AD Connect?][11]

5. <span data-ttu-id="9900f-167">Kapcsolattartási SciQuest támogatja ezt a hitelesítési módszert a fent letöltött metaadatok segítségével engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="9900f-167">Contact SciQuest support to enable this authentication method using the above downloaded metadata.</span></span>

6. <span data-ttu-id="9900f-168">A klasszikus Azure portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **Complete** bezárásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="9900f-168">On the Azure classic portal, select the single sign-on configuration confirmation, and then click **Complete** to close the **Configure Single Sign On** dialog.</span></span> 
   
    ![Mi az az Azure AD Connect?][15]

7. <span data-ttu-id="9900f-170">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="9900f-170">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="9900f-171">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9900f-171">Creating an Azure AD test user</span></span>
<span data-ttu-id="9900f-172">Ez a szakasz célja a tesztfelhasználó létrehozása a klasszikus Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="9900f-172">The objective of this section is to create a test user in the Azure classic portal called Britta Simon.</span></span>

<span data-ttu-id="9900f-173">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9900f-173">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="9900f-174">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9900f-174">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Mi az az Azure AD Connect?][100] 

2. <span data-ttu-id="9900f-176">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="9900f-176">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="9900f-177">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9900f-177">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][101] 

4. <span data-ttu-id="9900f-179">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="9900f-179">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span> 
   
    ![Mi az az Azure AD Connect?][102] 

5. <span data-ttu-id="9900f-181">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9900f-181">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Mi az az Azure AD Connect?][103] 
   
    <span data-ttu-id="9900f-183">a.</span><span class="sxs-lookup"><span data-stu-id="9900f-183">a.</span></span> <span data-ttu-id="9900f-184">Mint **felhasználó típusa**, jelölje be **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="9900f-184">As **Type Of User**, select **New user in your organization**.</span></span>
   
    <span data-ttu-id="9900f-185">b.</span><span class="sxs-lookup"><span data-stu-id="9900f-185">b.</span></span> <span data-ttu-id="9900f-186">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="9900f-186">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="9900f-187">c.</span><span class="sxs-lookup"><span data-stu-id="9900f-187">c.</span></span> <span data-ttu-id="9900f-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9900f-188">Click **Next**.</span></span>

6. <span data-ttu-id="9900f-189">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9900f-189">On the **User Profile** dialog page, perform the following steps:</span></span> 
   
    ![Mi az az Azure AD Connect?][104] 
   
    <span data-ttu-id="9900f-191">a.</span><span class="sxs-lookup"><span data-stu-id="9900f-191">a.</span></span> <span data-ttu-id="9900f-192">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="9900f-192">In the **First Name** textbox, type **Britta**.</span></span>  
   
    <span data-ttu-id="9900f-193">b.</span><span class="sxs-lookup"><span data-stu-id="9900f-193">b.</span></span> <span data-ttu-id="9900f-194">Az a **Vezetéknév** txtbox, típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="9900f-194">In the **Last Name** txtbox, type, **Simon**.</span></span>
   
    <span data-ttu-id="9900f-195">c.</span><span class="sxs-lookup"><span data-stu-id="9900f-195">c.</span></span> <span data-ttu-id="9900f-196">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9900f-196">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
    <span data-ttu-id="9900f-197">d.</span><span class="sxs-lookup"><span data-stu-id="9900f-197">d.</span></span> <span data-ttu-id="9900f-198">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="9900f-198">In the **Role** list, select **User**.</span></span>
   
    <span data-ttu-id="9900f-199">e.</span><span class="sxs-lookup"><span data-stu-id="9900f-199">e.</span></span> <span data-ttu-id="9900f-200">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="9900f-200">Click **Next**.</span></span>

7. <span data-ttu-id="9900f-201">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="9900f-201">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Mi az az Azure AD Connect?][105]  

8. <span data-ttu-id="9900f-203">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="9900f-203">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Mi az az Azure AD Connect?][106]   
   
    <span data-ttu-id="9900f-205">a.</span><span class="sxs-lookup"><span data-stu-id="9900f-205">a.</span></span> <span data-ttu-id="9900f-206">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="9900f-206">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="9900f-207">b.</span><span class="sxs-lookup"><span data-stu-id="9900f-207">b.</span></span> <span data-ttu-id="9900f-208">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="9900f-208">Click **Complete**.</span></span>   

### <a name="creating-a-sciquest-spend-director-test-user"></a><span data-ttu-id="9900f-209">SciQuest töltött igazgató tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="9900f-209">Creating a SciQuest Spend Director test user</span></span>
<span data-ttu-id="9900f-210">Ez a szakasz célja SciQuest töltött igazgató Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9900f-210">The objective of this section is to create a user called Britta Simon in SciQuest Spend Director.</span></span>

<span data-ttu-id="9900f-211">Szeretné a SciQuest töltött igazgató támogatási csoportjához, és adja meg a teszt fiókját is létre részleteit.</span><span class="sxs-lookup"><span data-stu-id="9900f-211">You need to contact your SciQuest Spend Director support team and provide them with the details about your test account to get it created.</span></span>

<span data-ttu-id="9900f-212">Másik lehetőségként is kihasználhatja just-in-time kiépítés, egyetlen bejelentkezés szolgáltatásként SciQuest töltött igazgató által támogatott.</span><span class="sxs-lookup"><span data-stu-id="9900f-212">Alternatively, you can also leverage just-in-time provisioning, a single sign-on feature that is supported by SciQuest Spend Director.</span></span>  
<span data-ttu-id="9900f-213">Ha közvetlenül az időponthoz kötött kiépítés engedélyezve van, felhasználók automatikusan létrejönnek SciQuest töltött igazgató egy egyszeri bejelentkezési kísérlet során, ha azok még nem léteznek.</span><span class="sxs-lookup"><span data-stu-id="9900f-213">If just-in-time provisioning is enabled, users are automatically created by SciQuest Spend Director during a single sign-on attempt if they don't exist.</span></span> <span data-ttu-id="9900f-214">Ez a funkció nem kell létrehoznia az egyszeri bejelentkezés tartozó felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9900f-214">This feature eliminates the need to manually create single sign-on counterpart users.</span></span>

<span data-ttu-id="9900f-215">Közvetlenül az időponthoz kötött kiépítés engedélyezve beszerzéséhez kapcsolatba kell lépnie még a SciQuest töltött igazgató támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="9900f-215">To get just-in-time provisioning enabled, you need to contact your your SciQuest Spend Director support team.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="9900f-216">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="9900f-216">Assigning the Azure AD test user</span></span>
<span data-ttu-id="9900f-217">Ez a szakasz célja Britta Simon nyújtó saját SciQuest töltött igazgató használandó Azure egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="9900f-217">The objective of this section is to enabling Britta Simon to use Azure single sign-on by granting her access to SciQuest Spend Director.</span></span>

![Mi az az Azure AD Connect?][200]

<span data-ttu-id="9900f-219">**Britta Simon hozzárendelése SciQuest töltött igazgató, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="9900f-219">**To assign Britta Simon to SciQuest Spend Director, perform the following steps:**</span></span>

1. <span data-ttu-id="9900f-220">A klasszikus Azure portálon, a könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="9900f-220">On the Azure classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Mi az az Azure AD Connect?][201]

2. <span data-ttu-id="9900f-222">Az alkalmazások listában válassza ki a **SciQuest töltött igazgató**.</span><span class="sxs-lookup"><span data-stu-id="9900f-222">In the applications list, select **SciQuest Spend Director**.</span></span>
   
    ![Mi az az Azure AD Connect?][202]

3. <span data-ttu-id="9900f-224">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9900f-224">In the menu on the top, click **Users**.</span></span>
   
    ![Mi az az Azure AD Connect?][203]

4. <span data-ttu-id="9900f-226">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="9900f-226">In the Users list, select **Britta Simon**.</span></span>
   
    ![Mi az az Azure AD Connect?][204]

5. <span data-ttu-id="9900f-228">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="9900f-228">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Mi az az Azure AD Connect?][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="9900f-230">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="9900f-230">Testing Single Sign-On</span></span>
<span data-ttu-id="9900f-231">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="9900f-231">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>  
<span data-ttu-id="9900f-232">Ha a hozzáférési panelen SciQuest töltött igazgató csempére kattint, meg kell beolvasása automatikusan bejelentkezett az SciQuest töltött igazgató alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="9900f-232">When you click the SciQuest Spend Director tile in the Access Panel, you should get automatically signed-on to your SciQuest Spend Director application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9900f-233">További források</span><span class="sxs-lookup"><span data-stu-id="9900f-233">Additional Resources</span></span>
* [<span data-ttu-id="9900f-234">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="9900f-234">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="9900f-235">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="9900f-235">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_01.png
[6]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_05.png
[8]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_06.png
[9]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_07.png
[10]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_08.png
[11]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_03.png
[15]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_04.png

[100]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_09.png 
[101]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_10.png 
[102]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_11.png 
[103]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_12.png 
[104]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_13.png 
[105]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_14.png 
[106]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_15.png 
[200]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_16.png 
[201]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_17.png 
[202]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_sciquest_spend_director_06.png
[203]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_18.png
[204]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_19.png
[205]: ./media/active-directory-saas-sciquest-spend-director-tutorial/tutorial_general_20.png

