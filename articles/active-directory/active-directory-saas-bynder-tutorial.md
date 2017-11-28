---
title: "Oktatóanyag: Azure Active Directoryval integrált Bynder |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Bynder között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 4fb0ab26-b3b9-420a-8072-a0be80ea021e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/17/2017
ms.author: jeedes
ms.openlocfilehash: 6786d7eb6a11405278ef7267f25279f9e39b3bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-bynder"></a><span data-ttu-id="5a3da-103">Oktatóanyag: Azure Active Directoryval integrált Bynder</span><span class="sxs-lookup"><span data-stu-id="5a3da-103">Tutorial: Azure Active Directory integration with Bynder</span></span>
<span data-ttu-id="5a3da-104">Ez az oktatóanyag célja Bynder integrálása az Azure Active Directory (Azure AD) mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="5a3da-104">The objective of this tutorial is to show you how to integrate Bynder with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="5a3da-105">Bynder integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5a3da-105">Integrating Bynder with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="5a3da-106">Megadhatja a Bynder hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="5a3da-106">You can control in Azure AD who has access to Bynder</span></span>
* <span data-ttu-id="5a3da-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Bynder egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="5a3da-107">You can enable your users to automatically get signed-on to Bynder single sign-on (SSO) with their Azure AD accounts</span></span>
* <span data-ttu-id="5a3da-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="5a3da-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="5a3da-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="5a3da-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a3da-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5a3da-110">Prerequisites</span></span>
<span data-ttu-id="5a3da-111">Konfigurálása az Azure AD-integrációs Bynder, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="5a3da-111">To configure Azure AD integration with Bynder, you need the following items:</span></span>

* <span data-ttu-id="5a3da-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="5a3da-112">An Azure AD subscription</span></span>
* <span data-ttu-id="5a3da-113">Egy Bynder egyszeri bejelentkezést (SSO) engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="5a3da-113">A Bynder single-sign on (SSO) enabled subscription</span></span>

>[!NOTE]
><span data-ttu-id="5a3da-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="5a3da-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span> 
> 

<span data-ttu-id="5a3da-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="5a3da-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="5a3da-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5a3da-116">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="5a3da-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="5a3da-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="5a3da-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="5a3da-118">Scenario description</span></span>
<span data-ttu-id="5a3da-119">Ez az oktatóanyag célja ahhoz, hogy a Microsoft Azure AD SSO teszteléséhez tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="5a3da-119">The objective of this tutorial is to enable you to test Microsoft Azure AD SSO in a test environment.</span></span>

<span data-ttu-id="5a3da-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="5a3da-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="5a3da-121">A gyűjteményből Bynder hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5a3da-121">Adding Bynder from the gallery</span></span>
2. <span data-ttu-id="5a3da-122">A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5a3da-122">Configuring and testing Microsoft Azure AD SSO</span></span>

## <a name="add-bynder-from-the-gallery"></a><span data-ttu-id="5a3da-123">Adja hozzá a Bynder a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="5a3da-123">Add Bynder from the gallery</span></span>
<span data-ttu-id="5a3da-124">Az Azure AD integrálása a Bynder konfigurálásához kell hozzáadnia Bynder a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="5a3da-124">To configure the integration of Bynder into Azure AD, you need to add Bynder from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="5a3da-125">**A gyűjteményből Bynder hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a3da-125">**To add Bynder from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="5a3da-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-126">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="5a3da-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="5a3da-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="5a3da-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="5a3da-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="5a3da-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="5a3da-131">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="5a3da-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="5a3da-135">Írja be a keresőmezőbe, **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-135">In the search box, type **Bynder**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_01.png)
7. <span data-ttu-id="5a3da-137">Az eredmények panelen válassza ki a **Bynder**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="5a3da-137">In the results panel, select **Bynder**, and then click **Complete** to add the application.</span></span>
   
    ![Az alkalmazás kiválasztása a katalógusban](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_001.png)

## <a name="configure-and-test-microsoft-azure-ad-sso"></a><span data-ttu-id="5a3da-139">A Microsoft Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5a3da-139">Configure and test Microsoft Azure AD SSO</span></span>
<span data-ttu-id="5a3da-140">Ez a szakasz célja bemutatják a Microsoft Azure AD "Britta Simon" nevű tesztfelhasználó alapján Bynder egyszeri bejelentkezés tesztelése és konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="5a3da-140">The objective of this section is to show you how to configure and test Microsoft Azure AD SSO with Bynder based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="5a3da-141">Az egyszeri bejelentkezés működjön az Azure AD tudnia kell, a partner felhasználó Bynder egy olyan felhasználó számára az Azure ad-ben van.</span><span class="sxs-lookup"><span data-stu-id="5a3da-141">For SSO to work, Azure AD needs to know what the counterpart user in Bynder to an user in Azure AD is.</span></span> <span data-ttu-id="5a3da-142">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a Bynder közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5a3da-142">In other words, a link relationship between an Azure AD user and the related user in Bynder needs to be established.</span></span>

<span data-ttu-id="5a3da-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Bynder a.</span><span class="sxs-lookup"><span data-stu-id="5a3da-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Bynder.</span></span>

<span data-ttu-id="5a3da-144">A Microsoft Azure AD Bynder egyszeri bejelentkezés tesztelése és konfigurálása, végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="5a3da-144">To configure and test Microsoft Azure AD SSO with Bynder, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="5a3da-145">**[Microsoft Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="5a3da-145">**[Configuring Microsoft Azure AD single sign-on](#configuring-azure-ad-single-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="5a3da-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – a Microsoft Azure AD egyszeri bejelentkezést a Britta Simon tesztelése.</span><span class="sxs-lookup"><span data-stu-id="5a3da-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Microsoft Azure AD Single Sign-On with Britta Simon.</span></span>
3. <span data-ttu-id="5a3da-147">**[Bynder tesztfelhasználó létrehozása](#creating-a-bynder-test-user)**  - való Britta Simon valami Bynder, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="5a3da-147">**[Creating a Bynder test user](#creating-a-bynder-test-user)** - to have a counterpart of Britta Simon in Bynder that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="5a3da-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használandó Microsoft Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5a3da-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Microsoft Azure AD Single Sign-On.</span></span>
5. <span data-ttu-id="5a3da-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="5a3da-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-microsoft-azure-ad-sso"></a><span data-ttu-id="5a3da-150">A Microsoft Azure AD egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="5a3da-150">Configuring Microsoft Azure AD SSO</span></span>
<span data-ttu-id="5a3da-151">Ebben a szakaszban a Microsoft Azure AD SSO engedélyezése a klasszikus portálon, és egyszeri bejelentkezés konfigurálása Bynder alkalmazásában.</span><span class="sxs-lookup"><span data-stu-id="5a3da-151">In this section, you enable Microsoft Azure AD SSO in the classic portal and configure SSO in your Bynder application.</span></span>

<span data-ttu-id="5a3da-152">**A Microsoft Azure AD egyszeri bejelentkezés konfigurálása Bynder, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a3da-152">**To configure Microsoft Azure AD SSO with Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="5a3da-153">A klasszikus portálon a a **Bynder** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="5a3da-153">In the classic portal, on the **Bynder** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása][6] 
2. <span data-ttu-id="5a3da-155">Az a **hová bejelentkezni Bynder felhasználók** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-155">On the **How would you like users to sign on to Bynder** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_03.png)
3. <span data-ttu-id="5a3da-157">A a **Alkalmazásbeállítások konfigurálása** párbeszédpanel oldal, ha szeretne beállítani az alkalmazás **IDP kezdeményezett mód**, hajtsa végre az alábbi lépéseket, és kattintson a **következő**:</span><span class="sxs-lookup"><span data-stu-id="5a3da-157">On the **Configure App Settings** dialog page, If you wish to configure the application in **IDP initiated mode**, perform the following steps and click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_04.png)
  1. <span data-ttu-id="5a3da-159">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.getbynder.com/sso/SAML/authenticate/`</span><span class="sxs-lookup"><span data-stu-id="5a3da-159">In the **Reply URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/sso/SAML/authenticate/`</span></span>
  2. <span data-ttu-id="5a3da-160">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-160">Click **Next**.</span></span>
4. <span data-ttu-id="5a3da-161">Ha szeretne beállítani az alkalmazás **Szolgáltató kezdeményezett mód** a a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapra, majd kattintson a a **"Megjelenítése speciális beállítások (választható)"** és Írja be a **URL-cím bejelentkezési** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-161">If you wish to configure the application in **SP initiated mode** on the **Configure App Settings** dialog page, then click on the **“Show advanced settings (optional)”** and then enter the **Sign On URL** and click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_10.png)
  1. <span data-ttu-id="5a3da-163">Az a **URL-cím bejelentkezési** szövegmező, adja meg a következő minta használatával URL-címe:`https://<company name>.getbynder.com/login/`</span><span class="sxs-lookup"><span data-stu-id="5a3da-163">In the **Sign On URL** textbox, type a URL using the following pattern: `https://<company name>.getbynder.com/login/`</span></span>
  2. <span data-ttu-id="5a3da-164">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-164">Click **Next**.</span></span>
  
   >[!NOTE]
   ><span data-ttu-id="5a3da-165">A bejelentkezési URL-cím ebben az oktatóanyagban értéke csak egy placeholfer.</span><span class="sxs-lookup"><span data-stu-id="5a3da-165">The value for the Sign On URL in this tutorial is just a placeholfer.</span></span> <span data-ttu-id="5a3da-166">Ahhoz, hogy a tényleges vlaue a környezetnek, forduljon a Bynder.</span><span class="sxs-lookup"><span data-stu-id="5a3da-166">To get the actual vlaue for your environment, contact Bynder.</span></span>
   >

5. <span data-ttu-id="5a3da-167">Az a **konfigurálhatja az egyszeri bejelentkezés Bynder** lapon hajtsa végre az alábbi lépéseket, majd kattintson **következő**:</span><span class="sxs-lookup"><span data-stu-id="5a3da-167">On the **Configure single sign-on at Bynder** page, perform the following steps and click **Next**:</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_05.png)  
  1. <span data-ttu-id="5a3da-169">Kattintson a **metaadatok letöltése**, majd mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="5a3da-169">Click **Download metadata**, and then save the file on your computer.</span></span>
  2. <span data-ttu-id="5a3da-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-170">Click **Next**.</span></span>
6. <span data-ttu-id="5a3da-171">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, lépjen kapcsolatba a Bynder támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="5a3da-171">To get SSO configured for your application, contact your Bynder support team.</span></span> <span data-ttu-id="5a3da-172">A letöltött metaadatfájl csatolja, és azt megosztása Bynder team beállítania egyszeri Bejelentkezést az oldalon.</span><span class="sxs-lookup"><span data-stu-id="5a3da-172">Attach the downloaded metadata file and share it with Bynder team to set up SSO on their side.</span></span>
7. <span data-ttu-id="5a3da-173">A klasszikus portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-173">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
   
    ![Az Azure AD-egyszeri bejelentkezés][10]
8. <span data-ttu-id="5a3da-175">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-175">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
   
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="5a3da-177">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5a3da-177">Create an Azure AD test user</span></span>
<span data-ttu-id="5a3da-178">Ez a szakasz célja a tesztfelhasználó létrehozása Britta Simon neve a klasszikus portálon.</span><span class="sxs-lookup"><span data-stu-id="5a3da-178">The objective of this section is to create a test user in the classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="5a3da-180">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a3da-180">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="5a3da-181">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-181">In the **Azure classic Portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_09.png)
2. <span data-ttu-id="5a3da-183">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="5a3da-183">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="5a3da-184">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-184">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_03.png)
4. <span data-ttu-id="5a3da-186">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-186">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_04.png)
5. <span data-ttu-id="5a3da-188">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5a3da-188">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_05.png)
  1. <span data-ttu-id="5a3da-190">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="5a3da-190">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="5a3da-191">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-191">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="5a3da-192">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-192">Click **Next**.</span></span>
6. <span data-ttu-id="5a3da-193">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5a3da-193">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_06.png)
  1. <span data-ttu-id="5a3da-195">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-195">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="5a3da-196">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-196">In the **Last Name** textbox, type, **Simon**.</span></span> 
  3. <span data-ttu-id="5a3da-197">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-197">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="5a3da-198">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-198">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="5a3da-199">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-199">Click **Next**.</span></span>
7. <span data-ttu-id="5a3da-200">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-200">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_07.png)
8. <span data-ttu-id="5a3da-202">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="5a3da-202">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-bynder-tutorial/create_aaduser_08.png)
   1. <span data-ttu-id="5a3da-204">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-204">Write down the value of the **New Password**.</span></span>
   2. <span data-ttu-id="5a3da-205">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="5a3da-205">Click **Complete**.</span></span>   

### <a name="create-a-bynder-test-user"></a><span data-ttu-id="5a3da-206">Bynder tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a3da-206">Create a Bynder test user</span></span>
<span data-ttu-id="5a3da-207">Ez a szakasz célja Bynder Britta Simon nevű felhasználót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="5a3da-207">The objective of this section is to create a user called Britta Simon in Bynder.</span></span> <span data-ttu-id="5a3da-208">Bynder támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="5a3da-208">Bynder supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="5a3da-209">Nincs ebben a szakaszban az Ön művelet elem.</span><span class="sxs-lookup"><span data-stu-id="5a3da-209">There is no action item for you in this section.</span></span> <span data-ttu-id="5a3da-210">Új felhasználó jön létre az Bynder elérésére, ha még nem létezik tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="5a3da-210">A new user will be created during an attempt to access Bynder if it doesn't exist yet.</span></span>

>[!NOTE]
><span data-ttu-id="5a3da-211">Hozzon létre egy felhasználó manuálisan kell, ha szüksége a Bynder támogatási csoportjához.</span><span class="sxs-lookup"><span data-stu-id="5a3da-211">If you need to create an user manually, you need to contact the Bynder support team.</span></span> 
> 

### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="5a3da-212">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="5a3da-212">Assign the Azure AD test user</span></span>
<span data-ttu-id="5a3da-213">Ez a szakasz célja Britta Simon által biztosított a hozzáférés Bynder Azure SSO használandó engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="5a3da-213">The objective of this section is to enabling Britta Simon to use Azure SSO by granting her access to Bynder.</span></span>

   ![Felhasználó hozzárendelése][200]

<span data-ttu-id="5a3da-215">**Britta Simon hozzárendelése Bynder, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="5a3da-215">**To assign Britta Simon to Bynder, perform the following steps:**</span></span>

1. <span data-ttu-id="5a3da-216">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="5a3da-216">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201]
2. <span data-ttu-id="5a3da-218">Az alkalmazások listában válassza ki a **Bynder**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-218">In the applications list, select **Bynder**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-bynder-tutorial/tutorial_bynder_50.png)
3. <span data-ttu-id="5a3da-220">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-220">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203]
4. <span data-ttu-id="5a3da-222">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-222">In the Users list, select **Britta Simon**.</span></span>
5. <span data-ttu-id="5a3da-223">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="5a3da-223">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="5a3da-225">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="5a3da-225">Test single sign-on</span></span>
<span data-ttu-id="5a3da-226">Ez a szakasz célja a hozzáférési panelen a Microsoft Azure AD SSO-konfiguráció teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="5a3da-226">The objective of this section is to test your Microsoft Azure AD SSO configuration using the Access Panel.</span></span>

<span data-ttu-id="5a3da-227">Ha a hozzáférési panelen Bynder csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az Bynder alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="5a3da-227">When you click the Bynder tile in the Access Panel, you should get automatically signed-on to your Bynder application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5a3da-228">További források</span><span class="sxs-lookup"><span data-stu-id="5a3da-228">Additional resources</span></span>
* [<span data-ttu-id="5a3da-229">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5a3da-229">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="5a3da-230">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="5a3da-230">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-bynder-tutorial/tutorial_general_205.png
