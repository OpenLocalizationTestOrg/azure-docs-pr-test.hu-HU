---
title: "Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és Splunk vállalati és Splunk felhő között."
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
ms.openlocfilehash: b78e9b7161207a74880e912241d5e965b353d1c5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-splunk-enterprise-and-splunk-cloud"></a><span data-ttu-id="72ea8-103">Oktatóanyag: Azure Active Directory-integráció a Splunk vállalati és Splunk felhő</span><span class="sxs-lookup"><span data-stu-id="72ea8-103">Tutorial: Azure Active Directory integration with Splunk Enterprise and Splunk Cloud</span></span>

<span data-ttu-id="72ea8-104">Ebben az oktatóanyagban elsajátíthatja Splunk vállalati és Splunk felhő integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="72ea8-104">In this tutorial, you learn how to integrate Splunk Enterprise and Splunk Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="72ea8-105">Splunk vállalati és Splunk felhő integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="72ea8-105">Integrating Splunk Enterprise and Splunk Cloud with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="72ea8-106">Szabályozhatja, aki hozzáfér Splunk vállalati és Splunk felhőalapú Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="72ea8-106">You can control in Azure AD who has access to Splunk Enterprise and Splunk Cloud</span></span>
- <span data-ttu-id="72ea8-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a Splunk vállalati és Splunk felhő egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="72ea8-107">You can enable your users to automatically get signed-on to Splunk Enterprise and Splunk Cloud single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="72ea8-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="72ea8-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="72ea8-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="72ea8-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="72ea8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="72ea8-110">Prerequisites</span></span>

<span data-ttu-id="72ea8-111">Konfigurálja az Azure AD-integrációs Splunk vállalati és Splunk felhő, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="72ea8-111">To configure Azure AD integration with Splunk Enterprise and Splunk Cloud, you need the following items:</span></span>

- <span data-ttu-id="72ea8-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="72ea8-112">An Azure AD subscription</span></span>
- <span data-ttu-id="72ea8-113">Egy Splunk vállalati vagy Splunk felhő SSO engedélyezve előfizetés</span><span class="sxs-lookup"><span data-stu-id="72ea8-113">A Splunk Enterprise or Splunk Cloud SSO enabled subscription</span></span>


>[!NOTE]
><span data-ttu-id="72ea8-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="72ea8-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="72ea8-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="72ea8-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="72ea8-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="72ea8-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="72ea8-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="72ea8-117">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>


## <a name="scenario-description"></a><span data-ttu-id="72ea8-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="72ea8-118">Scenario description</span></span>
<span data-ttu-id="72ea8-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="72ea8-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="72ea8-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="72ea8-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="72ea8-121">Splunk vállalati és Splunk felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="72ea8-121">Adding Splunk Enterprise and Splunk Cloud from the gallery</span></span>
2. <span data-ttu-id="72ea8-122">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72ea8-122">Configuring and testing Azure AD SSO</span></span>


## <a name="add-splunk-enterprise-and-splunk-cloud-from-the-gallery"></a><span data-ttu-id="72ea8-123">Splunk vállalati és Splunk felhő hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="72ea8-123">Add Splunk Enterprise and Splunk Cloud from the gallery</span></span>
<span data-ttu-id="72ea8-124">Az Azure AD integrálása a Splunk vállalati és Splunk felhő konfigurálásához kell hozzáadnia Splunk vállalati és a felhő Splunk a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="72ea8-124">To configure the integration of Splunk Enterprise and Splunk Cloud into Azure AD, you need to add Splunk Enterprise and Splunk Cloud from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="72ea8-125">**A gyűjteményből Splunk vállalati és Splunk felhő hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="72ea8-125">**To add Splunk Enterprise and Splunk Cloud from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="72ea8-126">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-126">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Active Directory][1]

2. <span data-ttu-id="72ea8-128">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="72ea8-128">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="72ea8-129">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="72ea8-129">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Alkalmazások][2]

4. <span data-ttu-id="72ea8-131">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="72ea8-131">Click **Add** at the bottom of the page.</span></span>

    ![Alkalmazások][3]

5. <span data-ttu-id="72ea8-133">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-133">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>

    ![Alkalmazások][4]

6. <span data-ttu-id="72ea8-135">Írja be a keresőmezőbe, **Splunk vállalati vagy Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-135">In the search box, type **Splunk Enterprise or Splunk Cloud**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_01.png)

7. <span data-ttu-id="72ea8-137">Az eredmények ablaktáblájában válassza **Splunk vállalati és Splunk felhő**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="72ea8-137">In the results pane, select **Splunk Enterprise and Splunk Cloud**, and then click **Complete** to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_02.png)

##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="72ea8-139">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72ea8-139">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="72ea8-140">Ebben a szakaszban az Azure AD egyszeri bejelentkezést a vállalati Splunk tesztelése és konfigurálása, és Splunk felhő "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="72ea8-140">In this section, you configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="72ea8-141">Az egyszeri bejelentkezés működéséhez az Azure AD tudnia kell, a partner felhasználó Splunk vállalati és Splunk felhő Újdonságok egy felhasználó számára az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="72ea8-141">For single sign-on to work, Azure AD needs to know what the counterpart user in Splunk Enterprise and Splunk Cloud is to a user in Azure AD.</span></span> <span data-ttu-id="72ea8-142">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a Splunk Enterprise és Splunk felhő közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="72ea8-142">In other words, a link relationship between an Azure AD user and the related user in Splunk Enterprise and Splunk Cloud needs to be established.</span></span>

<span data-ttu-id="72ea8-143">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** Splunk vállalati és Splunk felhő.</span><span class="sxs-lookup"><span data-stu-id="72ea8-143">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in Splunk Enterprise and Splunk Cloud.</span></span>

<span data-ttu-id="72ea8-144">Az Azure AD az egyszeri bejelentkezés Splunk vállalati és Splunk felhő tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="72ea8-144">To configure and test Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="72ea8-145">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="72ea8-145">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="72ea8-146">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="72ea8-146">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="72ea8-147">**[Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)**  - kell rendelkeznie a partner Britta Simon Splunk vállalati és Splunk felhő, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="72ea8-147">**[Creating a Splunk Enterprise and Splunk Cloud test user](#creating-a-splunk-enterprise-and-splunk-cloud-test-user)** - to have a counterpart of Britta Simon in Splunk Enterprise and Splunk Cloud that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="72ea8-148">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="72ea8-148">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="72ea8-149">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="72ea8-149">**[Testing single sign-on](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="72ea8-150">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="72ea8-150">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="72ea8-151">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést a klasszikus portálon, és egyszeri bejelentkezés konfigurálása az Splunk vállalati és Splunk felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="72ea8-151">In this section, you enable Azure AD SSO in the classic portal and configure SSO in your Splunk Enterprise and Splunk Cloud application.</span></span>


<span data-ttu-id="72ea8-152">**Konfigurálja az Azure AD az egyszeri bejelentkezés Splunk vállalati és Splunk felhő, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="72ea8-152">**To configure Azure AD single sign-on with Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="72ea8-153">A klasszikus portálon a a **Splunk vállalati és Splunk felhő** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezéshez** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="72ea8-153">In the classic portal, on the **Splunk Enterprise and Splunk Cloud** application integration page, click **Configure single sign-on** to open the **Configure Single Sign-On**  dialog.</span></span>
     
    ![Egyszeri bejelentkezés konfigurálása][6] 

2. <span data-ttu-id="72ea8-155">Az a **hová felhasználók bejelentkezhetnek a Splunk vállalati és Splunk felhő** lapon jelölje be **az Azure AD az egyszeri bejelentkezés**, és kattintson a **tovább**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-155">On the **How would you like users to sign on to Splunk Enterprise and Splunk Cloud** page, select **Azure AD Single Sign-On**, and then click **Next**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_03.png) 

3. <span data-ttu-id="72ea8-157">Az a **Alkalmazásbeállítások konfigurálása** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="72ea8-157">On the **Configure App Settings** dialog page, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_04.png) 
  1. <span data-ttu-id="72ea8-159">Az a **URL-cím bejelentkezési** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés az Splunk vállalati és Splunk felhő alkalmazáshoz, a következő minta használatával:`https://<splunkserverUrl>/en-US/app/launcher/home`</span><span class="sxs-lookup"><span data-stu-id="72ea8-159">In the **Sign On URL** textbox, type the URL used by your users to sign-on to your Splunk Enterprise and Splunk Cloud application using the following pattern: `https://<splunkserverUrl>/en-US/app/launcher/home`</span></span>
  2. <span data-ttu-id="72ea8-160">Az a **azonosító** szövegmezőhöz a Splunk kiszolgáló URL-címét.</span><span class="sxs-lookup"><span data-stu-id="72ea8-160">In the **Identifier** textbox, type the URL of your Splunk Server.</span></span>
  3. <span data-ttu-id="72ea8-161">Az a **válasz URL-CÍMEN** szövegmező, írja be a következő mintával az URL-cím:`https://<splunkserver>/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="72ea8-161">In the **Reply URL** textbox, type the URL with the following pattern: `https://<splunkserver>/saml/acs`</span></span>
  4. <span data-ttu-id="72ea8-162">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72ea8-162">Click **Next**.</span></span>
 
4. <span data-ttu-id="72ea8-163">Az a **Splunk vállalati és Splunk felhő egyszeri bejelentkezés konfigurálásáról** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="72ea8-163">On the **Configure single sign-on at Splunk Enterprise and Splunk Cloud** page, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_05.png)
  1. <span data-ttu-id="72ea8-165">Kattintson a **metaadatok letöltése**, majd mentse a fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="72ea8-165">Click **Download metadata**, and then save the file on your computer.</span></span>
  2. <span data-ttu-id="72ea8-166">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72ea8-166">Click **Next**.</span></span>

5. <span data-ttu-id="72ea8-167">Ahhoz, hogy az alkalmazáshoz konfigurált SSO, forduljon a Splunk vállalati és Splunk felhő támogatási csoport, és adja meg azokat a következő:</span><span class="sxs-lookup"><span data-stu-id="72ea8-167">To get SSO configured for your application, contact Splunk Enterprise and Splunk Cloud support team and provide them with the following:</span></span>

    * <span data-ttu-id="72ea8-168">A letöltött **federaton metaadatok**</span><span class="sxs-lookup"><span data-stu-id="72ea8-168">The downloaded **federaton metadata**</span></span>
6. <span data-ttu-id="72ea8-169">A klasszikus portálon, válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-169">In the classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span>
    
    ![Az Azure AD-egyszeri bejelentkezés][10]

7. <span data-ttu-id="72ea8-171">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-171">On the **Single sign-on confirmation** page, click **Complete**.</span></span>  
 
    ![Az Azure AD-egyszeri bejelentkezés][11]

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="72ea8-173">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="72ea8-173">Create an Azure AD test user</span></span>
<span data-ttu-id="72ea8-174">Ebben a szakaszban Britta Simon neve a klasszikus portálon tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="72ea8-174">In this section, you create a test user in the classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="72ea8-176">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="72ea8-176">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="72ea8-177">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-177">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="72ea8-179">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="72ea8-179">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="72ea8-180">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-180">To display the list of users, in the menu on the top, click **Users**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="72ea8-182">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-182">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="72ea8-184">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="72ea8-184">On the **Tell us about this user** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_05.png) 
  1. <span data-ttu-id="72ea8-186">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="72ea8-186">As Type Of User, select New user in your organization.</span></span>
  2. <span data-ttu-id="72ea8-187">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-187">In the User Name **textbox**, type **BrittaSimon**.</span></span>
  3. <span data-ttu-id="72ea8-188">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72ea8-188">Click **Next**.</span></span>

6.  <span data-ttu-id="72ea8-189">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="72ea8-189">On the **User Profile** dialog page, perform the following steps:</span></span>
  
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_06.png) 
  1. <span data-ttu-id="72ea8-191">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-191">In the **First Name** textbox, type **Britta**.</span></span>  
  2. <span data-ttu-id="72ea8-192">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-192">In the **Last Name** textbox, type, **Simon**.</span></span>
  3. <span data-ttu-id="72ea8-193">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-193">In the **Display Name** textbox, type **Britta Simon**.</span></span>
  4. <span data-ttu-id="72ea8-194">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-194">In the **Role** list, select **User**.</span></span>
  5. <span data-ttu-id="72ea8-195">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="72ea8-195">Click **Next**.</span></span>

7. <span data-ttu-id="72ea8-196">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-196">On the **Get temporary password** dialog page, click **create**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="72ea8-198">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="72ea8-198">On the **Get temporary password** dialog page, perform the following steps:</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/create_aaduser_08.png) 
  1. <span data-ttu-id="72ea8-200">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-200">Write down the value of the **New Password**.</span></span>
  2. <span data-ttu-id="72ea8-201">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="72ea8-201">Click **Complete**.</span></span>   

### <a name="create-a-splunk-enterprise-and-splunk-cloud-test-user"></a><span data-ttu-id="72ea8-202">Splunk vállalati és Splunk felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="72ea8-202">Create a Splunk Enterprise and Splunk Cloud test user</span></span>

<span data-ttu-id="72ea8-203">Ebben a szakaszban Britta Simon Splunk vállalati és Splunk felhő nevű felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="72ea8-203">In this section, you create a user called Britta Simon in Splunk Enterprise and Splunk Cloud.</span></span> <span data-ttu-id="72ea8-204">Splunk vállalati és Splunk felhő támogatási csoport hozzáadása a felhasználók a vállalati Splunk és Splunk felhő platform működik.</span><span class="sxs-lookup"><span data-stu-id="72ea8-204">Please work with Splunk Enterprise and Splunk Cloud support team to add the users in the Splunk Enterprise and Splunk Cloud platform.</span></span>


### <a name="assign-the-azure-ad-test-user"></a><span data-ttu-id="72ea8-205">Rendelje hozzá az Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="72ea8-205">Assign the Azure AD test user</span></span>

<span data-ttu-id="72ea8-206">Ebben a szakaszban Azure SSOy saját hozzáférés biztosítása az Splunk vállalati és a felhő Splunk használandó Britta Simon engedélyezi.</span><span class="sxs-lookup"><span data-stu-id="72ea8-206">In this section, you enable Britta Simon to use Azure SSOy granting her access to Splunk Enterprise and Splunk Cloud.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="72ea8-208">**Britta Simon Splunk vállalati és Splunk felhő hozzárendelése, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="72ea8-208">**To assign Britta Simon to Splunk Enterprise and Splunk Cloud, perform the following steps:**</span></span>

1. <span data-ttu-id="72ea8-209">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="72ea8-209">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="72ea8-211">Az alkalmazások listában válassza ki a **Splunk vállalati és Splunk felhő**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-211">In the applications list, select **Splunk Enterprise and Splunk Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-splunk-enterprise-and-splunk-cloud-tutorial/tutorial_splunk_50.png) 

3. <span data-ttu-id="72ea8-213">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-213">In the menu on the top, click **Users**.</span></span>

    ![Felhasználó hozzárendelése][203]

4. <span data-ttu-id="72ea8-215">A felhasználók listában válassza ki a **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-215">In the Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="72ea8-216">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="72ea8-216">In the toolbar on the bottom, click **Assign**.</span></span>

    ![Felhasználó hozzárendelése][205]

### <a name="test-single-sign-on"></a><span data-ttu-id="72ea8-218">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="72ea8-218">Test single sign-on</span></span>

<span data-ttu-id="72ea8-219">Ebben a szakaszban az Azure AD SSOonfiguration a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="72ea8-219">In this section, you test your Azure AD SSOonfiguration using the Access Panel.</span></span>

<span data-ttu-id="72ea8-220">A Splunk vállalati kattint, és a hozzáférési Panel Splunk felhő csempére, amikor meg kell beolvasása automatikusan bejelentkezett az Splunk vállalati és Splunk felhő alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="72ea8-220">When you click the Splunk Enterprise and Splunk Cloud tile in the Access Panel, you should get automatically signed-on to your Splunk Enterprise and Splunk Cloud application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="72ea8-221">További források</span><span class="sxs-lookup"><span data-stu-id="72ea8-221">Additional resources</span></span>

* [<span data-ttu-id="72ea8-222">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="72ea8-222">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="72ea8-223">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="72ea8-223">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


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
