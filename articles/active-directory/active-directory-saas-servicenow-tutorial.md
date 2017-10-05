---
title: "Oktatóanyag: Azure Active Directoryval integrált ServiceNow |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és a ServiceNow és a ServiceNow Express között."
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: 1d8eb99e-8ce5-4ba4-8b54-5c3d9ae573f6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: a91fab90a94b655b93c8ae9064ea4836b80d7678
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a><span data-ttu-id="80c90-103">Oktatóanyag: Azure Active Directoryval integrált ServiceNow</span><span class="sxs-lookup"><span data-stu-id="80c90-103">Tutorial: Azure Active Directory integration with ServiceNow</span></span>
<span data-ttu-id="80c90-104">Ebben az oktatóanyagban elsajátíthatja a ServiceNow és a ServiceNow Express integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="80c90-104">In this tutorial, you learn how to integrate ServiceNow and ServiceNow Express with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="80c90-105">A ServiceNow és a ServiceNow Express integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="80c90-105">Integrating ServiceNow and ServiceNow Express with Azure AD provides you with the following benefits:</span></span>

* <span data-ttu-id="80c90-106">Szabályozhatja, aki hozzáfér a ServiceNow és a ServiceNow Express Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="80c90-106">You can control in Azure AD who has access to ServiceNow and ServiceNow Express</span></span>
* <span data-ttu-id="80c90-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása aláírt a ServiceNow és a ServiceNow Express (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="80c90-107">You can enable your users to automatically get signed-on to ServiceNow and ServiceNow Express (Single Sign-On) with their Azure AD accounts</span></span>
* <span data-ttu-id="80c90-108">Kezelheti a fiókokat, egy központi helyen - a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="80c90-108">You can manage your accounts in one central location - the Azure classic portal</span></span>

<span data-ttu-id="80c90-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="80c90-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="80c90-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="80c90-110">Prerequisites</span></span>
<span data-ttu-id="80c90-111">Az Azure AD-integráció konfigurálása a ServiceNow és a ServiceNow Express, az alábbi elemek szükségesek:</span><span class="sxs-lookup"><span data-stu-id="80c90-111">To configure Azure AD integration with ServiceNow and ServiceNow Express, you need the following items:</span></span>

* <span data-ttu-id="80c90-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="80c90-112">An Azure AD subscription</span></span>
* <span data-ttu-id="80c90-113">A ServiceNow, egy példány vagy bérlő ServiceNow, Calgary verzió vagy újabb</span><span class="sxs-lookup"><span data-stu-id="80c90-113">For ServiceNow, an instance or tenant of ServiceNow, Calgary version or higher</span></span>
* <span data-ttu-id="80c90-114">A ServiceNow expressz, ServiceNow kifejezett, Helsinki verzió példányának vagy újabb</span><span class="sxs-lookup"><span data-stu-id="80c90-114">For ServiceNow Express, an instance of ServiceNow Express, Helsinki version or higher</span></span>
* <span data-ttu-id="80c90-115">A ServiceNow bérlő kell rendelkeznie a [több szolgáltató egyszeri bejelentkezést a beépülő modul](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="80c90-115">The ServiceNow tenant must have the [Multiple Provider Single Sign On Plugin](http://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0) enabled.</span></span> <span data-ttu-id="80c90-116">Ezt úgy teheti [szolgáltatási kérelem elküldése](https://hi.service-now.com).</span><span class="sxs-lookup"><span data-stu-id="80c90-116">This can be done by [submitting a service request](https://hi.service-now.com).</span></span> 

> [!NOTE]
> <span data-ttu-id="80c90-117">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="80c90-117">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>
> 
> 

<span data-ttu-id="80c90-118">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="80c90-118">To test the steps in this tutorial, you should follow these recommendations:</span></span>

* <span data-ttu-id="80c90-119">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="80c90-119">You should not use your production environment, unless this is necessary.</span></span>
* <span data-ttu-id="80c90-120">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="80c90-120">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="80c90-121">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="80c90-121">Scenario description</span></span>
<span data-ttu-id="80c90-122">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="80c90-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="80c90-123">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="80c90-123">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="80c90-124">A ServiceNow hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="80c90-124">Adding ServiceNow from the gallery</span></span>
2. <span data-ttu-id="80c90-125">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés ServiceNow vagy ServiceNow Express</span><span class="sxs-lookup"><span data-stu-id="80c90-125">Configuring and testing Azure AD single sign-on for ServiceNow or ServiceNow Express</span></span>

## <a name="adding-servicenow-from-the-gallery"></a><span data-ttu-id="80c90-126">A ServiceNow hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="80c90-126">Adding ServiceNow from the gallery</span></span>
<span data-ttu-id="80c90-127">Az Azure AD integrálása a ServiceNow vagy ServiceNow Express konfigurálásához kell hozzáadnia a ServiceNow a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="80c90-127">To configure the integration of ServiceNow or ServiceNow Express into Azure AD, you need to add ServiceNow from the gallery to your list of managed SaaS apps.</span></span> 

<span data-ttu-id="80c90-128">**A ServiceNow hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="80c90-128">**To add ServiceNow from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="80c90-129">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="80c90-129">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Active Directory][1]
2. <span data-ttu-id="80c90-131">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="80c90-131">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
3. <span data-ttu-id="80c90-132">A könyvtár nézetben a alkalmazások nézet megnyitásához kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="80c90-132">To open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Alkalmazások][2]
4. <span data-ttu-id="80c90-134">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="80c90-134">Click **Add** at the bottom of the page.</span></span>
   
    ![Alkalmazások][3]
5. <span data-ttu-id="80c90-136">Az a **mi történjen a teendő** párbeszédpanel, kattintson a **hozzáadhat egy alkalmazást a katalógusból**.</span><span class="sxs-lookup"><span data-stu-id="80c90-136">On the **What do you want to do** dialog, click **Add an application from the gallery**.</span></span>
   
    ![Alkalmazások][4]
6. <span data-ttu-id="80c90-138">Írja be a keresőmezőbe, **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="80c90-138">In the search box, type **ServiceNow**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_01.png)
7. <span data-ttu-id="80c90-140">Az eredmények ablaktáblájában válassza **ServiceNow**, és kattintson a **Complete** hozzáadása az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="80c90-140">In the results pane, select **ServiceNow**, and then click **Complete** to add the application.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_02.png)

## <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="80c90-142">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="80c90-142">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="80c90-143">Ebben a szakaszban konfigurálása és tesztelése az Azure AD az egyszeri bejelentkezés ServiceNow vagy ServiceNow Express "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="80c90-143">In this section, you configure and test Azure AD single sign-on with ServiceNow or ServiceNow Express based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="80c90-144">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a ServiceNow tartozó felhasználót a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="80c90-144">For single sign-on to work, Azure AD needs to know what the counterpart user in ServiceNow is to a user in Azure AD.</span></span> <span data-ttu-id="80c90-145">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó a ServiceNow közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="80c90-145">In other words, a link relationship between an Azure AD user and the related user in ServiceNow needs to be established.</span></span>
<span data-ttu-id="80c90-146">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="80c90-146">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ServiceNow.</span></span> <span data-ttu-id="80c90-147">Az Azure AD egyszeri bejelentkezést a ServiceNow tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="80c90-147">To configure and test Azure AD single sign-on with ServiceNow, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="80c90-148">**[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="80c90-148">**[Configuring Azure AD Single Sign-On for ServiceNow](#configuring-azure-ad-single-sign-on-for-servicenow)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="80c90-149">**[Az Azure AD az egyszeri bejelentkezés beállítása a ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="80c90-149">**[Configuring Azure AD Single Sign-On for ServiceNow Express](#configuring-azure-ad-single-sign-on-for-servicenow-express)** - to enable your users to use this feature.</span></span>
3. <span data-ttu-id="80c90-150">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="80c90-150">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="80c90-151">**[A ServiceNow tesztfelhasználó létrehozása](#creating-a-servicenow-test-user)**  - való Britta Simon egy megfelelője a ServiceNow, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="80c90-151">**[Creating a ServiceNow test user](#creating-a-servicenow-test-user)** - to have a counterpart of Britta Simon in ServiceNow that is linked to the Azure AD representation of her.</span></span>
5. <span data-ttu-id="80c90-152">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="80c90-152">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
6. <span data-ttu-id="80c90-153">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="80c90-153">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

> [!NOTE]
> <span data-ttu-id="80c90-154">Ha szeretne konfigurálni a ServiceNow hagyja el a 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="80c90-154">If you want to configure ServiceNow omit step 2.</span></span> <span data-ttu-id="80c90-155">Hasonlóképpen ha azt szeretné, hogy a ServiceNow Express konfigurálásához 1. lépés kihagyása.</span><span class="sxs-lookup"><span data-stu-id="80c90-155">Likewise, if you want to configure ServiceNow Express omit step 1.</span></span>
> 
> 

### <a name="configuring-azure-ad-single-sign-on-for-servicenow"></a><span data-ttu-id="80c90-156">A ServiceNow az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="80c90-156">Configuring Azure AD Single Sign-On for ServiceNow</span></span>
1. <span data-ttu-id="80c90-157">A klasszikus Azure AD portálon a a **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c90-157">In the Azure AD classic portal, on the **ServiceNow** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="80c90-158">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-158">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="80c90-159">Az a **hová felhasználók számára történő bejelentkezést ServiceNow** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="80c90-159">On the **How would you like users to sign on to ServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="80c90-160">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-160">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="80c90-161">Az a **Alkalmazásbeállítások konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="80c90-161">On the **Configure App Settings** page, perform the following steps:</span></span>
   
    <span data-ttu-id="80c90-162">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-162">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="80c90-163">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-163">a.</span></span> <span data-ttu-id="80c90-164">az a **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés a ServiceNow alkalmazáshoz a következő mintát: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="80c90-164">in the **ServiceNow Sign On URL** textbox, type your URL used by your users to sign-on to your ServiceNow application following the pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="80c90-165">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-165">b.</span></span> <span data-ttu-id="80c90-166">Az a **azonosító** szövegmező, írja be az URL-cím segítségével a felhasználók bejelentkezés a ServiceNow alkalmazáshoz a következő mintát: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="80c90-166">In the **Identifier** textbox,type your URL used by your users to sign-on to your ServiceNow application following the pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="80c90-167">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-167">c.</span></span> <span data-ttu-id="80c90-168">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="80c90-168">Click **Next**</span></span>

4. <span data-ttu-id="80c90-169">Ahhoz, hogy az Azure AD automatikus konfigurálása a ServiceNow SAML-alapú hitelesítéshez, adja meg a ServiceNow példány nevét, a rendszergazda felhasználónevét és a rendszergazdai jelszó a **automatikus konfigurálása egyszeri bejelentkezéshez** kialakításához, és kattintson a *konfigurálása*.</span><span class="sxs-lookup"><span data-stu-id="80c90-169">To have Azure AD automatically configure ServiceNow for SAML-based authentication, enter your ServiceNow instance name, admin username, and admin password in the **Auto configure single sign-on** form and click *Configure*.</span></span> <span data-ttu-id="80c90-170">Vegye figyelembe, hogy a rendszergazdai felhasználónevet kell rendelkeznie a **security_admin** működéséhez a ServiceNow hozzárendelve szerepkör.</span><span class="sxs-lookup"><span data-stu-id="80c90-170">Note that the admin username provided must have the **security_admin** role assigned in ServiceNow for this to work.</span></span> <span data-ttu-id="80c90-171">Ellenkező esetben a SAML-Identitásszolgáltatóként az Azure AD használandó ServiceNow kézzel konfigurálásához kattintson **manuálisan állítsa be az alkalmazását az egyszeri bejelentkezés**, majd kattintson a **következő** és kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="80c90-171">Otherwise, to manually configure ServiceNow to use Azure AD as a SAML identity provider, click **Manually configure the application for single sign-on**, then click **Next** and complete the following steps.</span></span>
   
    <span data-ttu-id="80c90-172">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-172">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="80c90-173">A a **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen a tanúsítvány fájlt.</span><span class="sxs-lookup"><span data-stu-id="80c90-173">On the **Configure single sign-on at ServiceNow** page, click **Download certificate**, save the certificate file locally on your computer.</span></span>
   
    <span data-ttu-id="80c90-174">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-174">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="80c90-175">Bejelentkezés a ServiceNow alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="80c90-175">Sign-on to your ServiceNow application as an administrator.</span></span>

7. <span data-ttu-id="80c90-176">Aktiválja a *integrációs - több szolgáltató egyszeri bejelentkezés telepítő* beépülő modul a következő lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="80c90-176">Activate the *Integration - Multiple Provider Single Sign-On Installer* plugin by following the next steps:</span></span>
   
    <span data-ttu-id="80c90-177">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-177">a.</span></span> <span data-ttu-id="80c90-178">A bal oldali navigációs ablaktábláján válassza **rendszer Definition** szakaszt, és kattintson a **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="80c90-178">In the navigation pane on the left side, go to **System Definition** section and then click **Plugins**.</span></span>
   
    <span data-ttu-id="80c90-179">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "beépülő modul aktiválása")</span><span class="sxs-lookup"><span data-stu-id="80c90-179">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_03.png "Activate plugin")</span></span>
   
    <span data-ttu-id="80c90-180">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-180">b.</span></span> <span data-ttu-id="80c90-181">Keresse meg *integrációs - több szolgáltató egyszeri bejelentkezés telepítő*.</span><span class="sxs-lookup"><span data-stu-id="80c90-181">Search for *Integration - Multiple Provider Single Sign-On Installer*.</span></span>
   
    <span data-ttu-id="80c90-182">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "beépülő modul aktiválása")</span><span class="sxs-lookup"><span data-stu-id="80c90-182">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_04.png "Activate plugin")</span></span>
   
    <span data-ttu-id="80c90-183">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-183">c.</span></span> <span data-ttu-id="80c90-184">Válassza ki a beépülő modul.</span><span class="sxs-lookup"><span data-stu-id="80c90-184">Select the plugin.</span></span> <span data-ttu-id="80c90-185">Rigth kattintson, és válassza ki **aktiválás/frissítése**.</span><span class="sxs-lookup"><span data-stu-id="80c90-185">Rigth click and select **Activate/Upgrade**.</span></span>
   
    <span data-ttu-id="80c90-186">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-186">d.</span></span> <span data-ttu-id="80c90-187">Kattintson a **aktiválás** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-187">Click the **Activate** button.</span></span>

8. <span data-ttu-id="80c90-188">A bal oldali navigációs ablaktábláján kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="80c90-188">In the navigation pane on the left side, click **Properties**.</span></span>  
   
    <span data-ttu-id="80c90-189">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-189">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_06.png "Configure app URL")</span></span>

9. <span data-ttu-id="80c90-190">Az a **több SSO tulajdonságokat** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-190">On the **Multiple Provider SSO Properties** dialog, perform the following steps:</span></span>
   
    <span data-ttu-id="80c90-191">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-191">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694981.png "Configure app URL")</span></span>
   
    <span data-ttu-id="80c90-192">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-192">a.</span></span> <span data-ttu-id="80c90-193">Mint **több szolgáltató SSO engedélyezése**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="80c90-193">As **Enable multiple provider SSO**, select **Yes**.</span></span>
   
    <span data-ttu-id="80c90-194">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-194">b.</span></span> <span data-ttu-id="80c90-195">Mint **hibakeresési naplózás kapott a több szolgáltató SSO-integráció engedélyezése**, jelölje be **Igen**.</span><span class="sxs-lookup"><span data-stu-id="80c90-195">As **Enable debug logging got the multiple provider SSO integration**, select **Yes**.</span></span>
   
    <span data-ttu-id="80c90-196">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-196">c.</span></span> <span data-ttu-id="80c90-197">A **a mezőt, a felhasználó táblázat...**  szövegmezőhöz típus **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="80c90-197">In **The field on the user table that...** textbox, type **user_name**.</span></span>
   
    <span data-ttu-id="80c90-198">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-198">d.</span></span> <span data-ttu-id="80c90-199">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-199">Click **Save**.</span></span>

10. <span data-ttu-id="80c90-200">A bal oldali navigációs ablaktábláján kattintson **x509 tanúsítványok**.</span><span class="sxs-lookup"><span data-stu-id="80c90-200">In the navigation pane on the left side, click **x509 Certificates**.</span></span>
    
     <span data-ttu-id="80c90-201">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-201">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_05.png "Configure single sign-on")</span></span>

11. <span data-ttu-id="80c90-202">Az a **X.509-tanúsítványokat** párbeszédpanel, kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="80c90-202">On the **X.509 Certificates** dialog, click **New**.</span></span>
    
     <span data-ttu-id="80c90-203">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-203">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694974.png "Configure single sign-on")</span></span>

12. <span data-ttu-id="80c90-204">Az a **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-204">On the **X.509 Certificates** dialog, perform the following steps:</span></span>
    
     <span data-ttu-id="80c90-205">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-205">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
     <span data-ttu-id="80c90-206">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-206">a.</span></span> <span data-ttu-id="80c90-207">Kattintson az **Új** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="80c90-207">Click **New**.</span></span>
    
     <span data-ttu-id="80c90-208">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-208">b.</span></span> <span data-ttu-id="80c90-209">Az a **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="80c90-209">In the **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
     <span data-ttu-id="80c90-210">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-210">c.</span></span> <span data-ttu-id="80c90-211">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="80c90-211">Select **Active**.</span></span>
    
     <span data-ttu-id="80c90-212">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-212">d.</span></span> <span data-ttu-id="80c90-213">Mint **formátum**, jelölje be **PEM**.</span><span class="sxs-lookup"><span data-stu-id="80c90-213">As **Format**, select **PEM**.</span></span>
    
     <span data-ttu-id="80c90-214">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-214">e.</span></span> <span data-ttu-id="80c90-215">Mint **típus**, jelölje be **megbízható tároló Cert**.</span><span class="sxs-lookup"><span data-stu-id="80c90-215">As **Type**, select **Trust Store Cert**.</span></span>
    
     <span data-ttu-id="80c90-216">f.</span><span class="sxs-lookup"><span data-stu-id="80c90-216">f.</span></span> <span data-ttu-id="80c90-217">A Base64 kódolású tanúsítvány megnyitása a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **PEM tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-217">Open your Base64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **PEM Certificate** textbox.</span></span>
    
     <span data-ttu-id="80c90-218">g.</span><span class="sxs-lookup"><span data-stu-id="80c90-218">g.</span></span> <span data-ttu-id="80c90-219">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="80c90-219">Click **Update**.</span></span>

13. <span data-ttu-id="80c90-220">A bal oldali navigációs ablaktábláján kattintson **identitás-szolgáltatóktól**.</span><span class="sxs-lookup"><span data-stu-id="80c90-220">In the navigation pane on the left side, click **Identity Providers**.</span></span>
    
     <span data-ttu-id="80c90-221">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-221">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_07.png "Configure single sign-on")</span></span>

14. <span data-ttu-id="80c90-222">Az a **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **új**:</span><span class="sxs-lookup"><span data-stu-id="80c90-222">On the **Identity Providers** dialog, click **New**:</span></span>
    
     <span data-ttu-id="80c90-223">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-223">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694977.png "Configure single sign-on")</span></span>

15. <span data-ttu-id="80c90-224">Az a **identitás-szolgáltatóktól** párbeszédpanel, kattintson a **egy SAML2 1. frissítés?**:</span><span class="sxs-lookup"><span data-stu-id="80c90-224">On the **Identity Providers** dialog, click **SAML2 Update1?**:</span></span>
    
     <span data-ttu-id="80c90-225">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-225">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694978.png "Configure single sign-on")</span></span>

16. <span data-ttu-id="80c90-226">Egy SAML2 1. frissítés tulajdonságai párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-226">On the SAML2 Update1 Properties dialog, perform the following steps:</span></span>
    
     <span data-ttu-id="80c90-227">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-227">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694982.png "Configure single sign-on")</span></span>

    <span data-ttu-id="80c90-228">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-228">a.</span></span> <span data-ttu-id="80c90-229">az a **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="80c90-229">in the **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="80c90-230">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-230">b.</span></span> <span data-ttu-id="80c90-231">Az a **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, melyik mezőt a ServiceNow környezetben a felhasználók egyedi azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="80c90-231">In the **User Field** textbox, type **email** or **user_name**, depending on which field is used to uniquely identify users in your ServiceNow deployment.</span></span> 

    > [!NOTE] 
    > <span data-ttu-id="80c90-232">Az Azure AD configue hozható létre az Azure AD felhasználói azonosító (egyszerű felhasználónév) vagy az e-mail cím a SAML-jogkivonat egyedi azonosítóként a is a **ServiceNow > attribútumok > egyszeri bejelentkezés** a klasszikus Azure portálra, és a kívánt mezőt leképezési a **nameidentifier** attribútum.</span><span class="sxs-lookup"><span data-stu-id="80c90-232">You can configue Azure AD to emit either the Azure AD user ID (user principal name) or the email address as the unique identifier in the SAML token by going to the **ServiceNow > Attributes > Single Sign-On** section of the Azure classic portal and mapping the desired field to the **nameidentifier** attribute.</span></span> <span data-ttu-id="80c90-233">A kiválasztott attribútumnak az Azure AD (pl. egyszerű felhasználónév) tárolt érték meg kell egyeznie a megadott mező (pl. felhasználónév) ServiceNow tárolt érték</span><span class="sxs-lookup"><span data-stu-id="80c90-233">The value stored for the selected attribute in Azure AD (e.g. user principal name) must match the value stored in ServiceNow for the entered field (e.g. user_name)</span></span>

    <span data-ttu-id="80c90-234">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-234">c.</span></span> <span data-ttu-id="80c90-235">A klasszikus Azure AD portálon, másolja a **identitás Szolgáltatóazonosító** értékét, és illessze be azt a **identitási szolgáltató URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-235">In the Azure AD classic portal, copy the **Identity Provider ID** value, and then paste it into the **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="80c90-236">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-236">d.</span></span> <span data-ttu-id="80c90-237">A klasszikus Azure AD portálon, másolja a **hitelesítési kérelem URL-cím** értékét, és illessze be azt a **identitásszolgáltató AuthnRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-237">In the Azure AD classic portal, copy the **Authentication Request URL** value, and then paste it into the **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="80c90-238">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-238">e.</span></span> <span data-ttu-id="80c90-239">A klasszikus Azure AD portálon, másolja a **egyetlen Sign-Out URL-címe** értékét, és illessze be azt a **identitásszolgáltató SingleLogoutRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-239">In the Azure AD classic portal, copy the **Single Sign-Out Service URL** value, and then paste it into the **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="80c90-240">f.</span><span class="sxs-lookup"><span data-stu-id="80c90-240">f.</span></span> <span data-ttu-id="80c90-241">Az a **ServiceNow kezdőlap** szövegmezőhöz a ServiceNow példány kezdőlap URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-241">In the **ServiceNow Homepage** textbox, type the URL of your ServiceNow instance homepage.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="80c90-242">A ServiceNow példány kezdőlapon összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.:`https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="80c90-242">The ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.:`https://fabrikam.service-now.com/navpage.do`).</span></span>

    <span data-ttu-id="80c90-243">g.</span><span class="sxs-lookup"><span data-stu-id="80c90-243">g.</span></span> <span data-ttu-id="80c90-244">Az a **Entitásazonosító / kibocsátó** szövegmezőhöz a ServiceNow bérlői URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-244">In the **Entity ID / Issuer** textbox, type the URL of your ServiceNow tenant.</span></span>

    <span data-ttu-id="80c90-245">h.</span><span class="sxs-lookup"><span data-stu-id="80c90-245">h.</span></span> <span data-ttu-id="80c90-246">Az a **célközönség URL-cím** szövegmezőhöz a ServiceNow bérlői URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-246">In the **Audience URL** textbox, type the URL of your ServiceNow tenant.</span></span> 

    <span data-ttu-id="80c90-247">i.</span><span class="sxs-lookup"><span data-stu-id="80c90-247">i.</span></span> <span data-ttu-id="80c90-248">Az a **protokoll kötése az IDP SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.</span><span class="sxs-lookup"><span data-stu-id="80c90-248">In the **Protocol Binding for the IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>

    <span data-ttu-id="80c90-249">j.</span><span class="sxs-lookup"><span data-stu-id="80c90-249">j.</span></span> <span data-ttu-id="80c90-250">Írja be a NameID házirend szövegmező **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.</span><span class="sxs-lookup"><span data-stu-id="80c90-250">In the NameID Policy textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>

    <span data-ttu-id="80c90-251">k.</span><span class="sxs-lookup"><span data-stu-id="80c90-251">k.</span></span> <span data-ttu-id="80c90-252">Kapcsolja ki **hozzon létre egy AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="80c90-252">Deselect **Create an AuthnContextClass**.</span></span>

    <span data-ttu-id="80c90-253">l.</span><span class="sxs-lookup"><span data-stu-id="80c90-253">l.</span></span> <span data-ttu-id="80c90-254">Az a **AuthnContextClassRef metódus**, típus `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span><span class="sxs-lookup"><span data-stu-id="80c90-254">In the **AuthnContextClassRef Method**, type `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.</span></span> <span data-ttu-id="80c90-255">Ez csak akkor szükséges, ha egyetlen szervezet felhőbeli.</span><span class="sxs-lookup"><span data-stu-id="80c90-255">This is only needed if you are cloud only organization.</span></span> <span data-ttu-id="80c90-256">Használata a helyszíni AD FS vagy MFA hitelesítés akkor ne konfigurálja ezt az értéket.</span><span class="sxs-lookup"><span data-stu-id="80c90-256">If you are using on-premises ADFS or MFA for authentication then you should not configure this value.</span></span> 

    <span data-ttu-id="80c90-257">m.</span><span class="sxs-lookup"><span data-stu-id="80c90-257">m.</span></span> <span data-ttu-id="80c90-258">A **óra döntés** szövegmezőhöz típus **60**.</span><span class="sxs-lookup"><span data-stu-id="80c90-258">In **Clock Skew** textbox, type **60**.</span></span>

    <span data-ttu-id="80c90-259">n.</span><span class="sxs-lookup"><span data-stu-id="80c90-259">n.</span></span> <span data-ttu-id="80c90-260">Mint **egyszeri bejelentkezést a parancsfájl**, jelölje be **MultiSSO_SAML2_Update1**.</span><span class="sxs-lookup"><span data-stu-id="80c90-260">As **Single Sign On Script**, select **MultiSSO_SAML2_Update1**.</span></span>

    <span data-ttu-id="80c90-261">o.</span><span class="sxs-lookup"><span data-stu-id="80c90-261">o.</span></span> <span data-ttu-id="80c90-262">Mint **x509 tanúsítvány**, válassza ki az előző lépésben létrehozott tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="80c90-262">As **x509 Certificate**, select the certificate you have created in the previous step.</span></span>

    <span data-ttu-id="80c90-263">p.</span><span class="sxs-lookup"><span data-stu-id="80c90-263">p.</span></span> <span data-ttu-id="80c90-264">Kattintson a **nyújt**.</span><span class="sxs-lookup"><span data-stu-id="80c90-264">Click **Submit**.</span></span> 

1. <span data-ttu-id="80c90-265">A klasszikus Azure AD portálon válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="80c90-265">On the Azure AD classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="80c90-266">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-266">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="80c90-267">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="80c90-267">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="80c90-268">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-268">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

### <a name="configuring-azure-ad-single-sign-on-for-servicenow-express"></a><span data-ttu-id="80c90-269">A ServiceNow expressz az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="80c90-269">Configuring Azure AD Single Sign-On for ServiceNow Express</span></span>
1. <span data-ttu-id="80c90-270">A klasszikus Azure AD portálon a a **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálása egyszeri bejelentkezéshez** megnyitásához a **konfigurálása egyszeri bejelentkezés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="80c90-270">In the Azure AD classic portal, on the **ServiceNow** application integration page, click **Configure single sign-on** to open the **Configure Single Sign On** dialog.</span></span>
   
    <span data-ttu-id="80c90-271">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749323.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-271">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749323.png "Configure single sign-on")</span></span>

2. <span data-ttu-id="80c90-272">Az a **hová felhasználók számára történő bejelentkezést ServiceNow** lapon jelölje be **Microsoft Azure AD az egyszeri bejelentkezés**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="80c90-272">On the **How would you like users to sign on to ServiceNow** page, select **Microsoft Azure AD Single Sign-On**, and then click **Next**.</span></span>
   
    <span data-ttu-id="80c90-273">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749324.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-273">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749324.png "Configure single sign-on")</span></span>

3. <span data-ttu-id="80c90-274">Az a **Alkalmazásbeállítások konfigurálása** lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="80c90-274">On the **Configure App Settings** page, perform the following steps:</span></span>
   
    <span data-ttu-id="80c90-275">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC769497.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-275">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC769497.png "Configure app URL")</span></span>
   
    <span data-ttu-id="80c90-276">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-276">a.</span></span> <span data-ttu-id="80c90-277">az a **ServiceNow bejelentkezési URL-cím** szövegmező, írja be az URL-címet használják-e a felhasználók bejelentkezés a ServiceNow alkalmazáshoz a következő mintát: `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="80c90-277">in the **ServiceNow Sign On URL** textbox, type your URL used by your users to sign-on to your ServiceNow application following the pattern: `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="80c90-278">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-278">b.</span></span> <span data-ttu-id="80c90-279">Az a **kiállítójának URL-címe** szövegmező, írja be az URL-cím segítségével a felhasználók bejelentkezés a ServiceNow alkalmazásra mintát a következő `https://<instance-name>.service-now.com`.</span><span class="sxs-lookup"><span data-stu-id="80c90-279">In the **Issuer URL** textbox,type your URL used by your users to sign-on to your ServiceNow application following the pattern `https://<instance-name>.service-now.com`.</span></span>
   
    <span data-ttu-id="80c90-280">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-280">c.</span></span> <span data-ttu-id="80c90-281">Kattintson a **Tovább** gombra</span><span class="sxs-lookup"><span data-stu-id="80c90-281">Click **Next**</span></span>

4. <span data-ttu-id="80c90-282">Kattintson a **manuálisan állítsa be az alkalmazását az egyszeri bejelentkezés**, majd kattintson a **következő** és kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="80c90-282">Click **Manually configure the application for single sign-on**, then click **Next** and complete the following steps.</span></span>
   
    <span data-ttu-id="80c90-283">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-283">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/IC7694971.png "Configure app URL")</span></span>

5. <span data-ttu-id="80c90-284">A a **konfigurálhatja az egyszeri bejelentkezés ServiceNow** kattintson **tanúsítvánnyal letöltés**, mentse helyileg a számítógépen a tanúsítvány fájlt, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="80c90-284">On the **Configure single sign-on at ServiceNow** page, click **Download certificate**, save the certificate file locally on your computer, and then click **Next**.</span></span>
   
    <span data-ttu-id="80c90-285">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC749325.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-285">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC749325.png "Configure single sign-on")</span></span>

6. <span data-ttu-id="80c90-286">Bejelentkezés a ServiceNow Express alkalmazást rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="80c90-286">Sign-on to your ServiceNow Express application as an administrator.</span></span>

7. <span data-ttu-id="80c90-287">A bal oldali navigációs ablaktábláján kattintson **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="80c90-287">In the navigation pane on the left side, click **Single Sign-On**.</span></span>  
   
    <span data-ttu-id="80c90-288">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-288">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694980ex.png "Configure app URL")</span></span>

8. <span data-ttu-id="80c90-289">Az a **egyszeri bejelentkezés** párbeszédpanel, kattintson a jobb felső konfigurációs ikonjára, és állítsa be a következő tulajdonságokat:</span><span class="sxs-lookup"><span data-stu-id="80c90-289">On the **Single Sign-On** dialog, click the configuration icon on the upper right and set the following properties:</span></span>
   
    <span data-ttu-id="80c90-290">![Alkalmazás URL-CÍMEK konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "alkalmazás URL-CÍMEK konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-290">![Configure app URL](./media/active-directory-saas-servicenow-tutorial/ic7694981ex.png "Configure app URL")</span></span>
   
    <span data-ttu-id="80c90-291">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-291">a.</span></span> <span data-ttu-id="80c90-292">Váltás **több szolgáltató SSO engedélyezése** jobbra.</span><span class="sxs-lookup"><span data-stu-id="80c90-292">Toggle **Enable multiple provider SSO** to the right.</span></span>
   
    <span data-ttu-id="80c90-293">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-293">b.</span></span> <span data-ttu-id="80c90-294">Váltás **engedélyezése hibakeresési naplózás több szolgáltatójának SSO integrációs** jobbra.</span><span class="sxs-lookup"><span data-stu-id="80c90-294">Toggle **Enable debug logging for the multiple provider SSO integration** to the right.</span></span>
   
    <span data-ttu-id="80c90-295">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-295">c.</span></span> <span data-ttu-id="80c90-296">A **a mezőt, a felhasználó táblázat...**  szövegmezőhöz típus **felhasználónév**.</span><span class="sxs-lookup"><span data-stu-id="80c90-296">In **The field on the user table that...** textbox, type **user_name**.</span></span>
9. <span data-ttu-id="80c90-297">Az a **egyszeri bejelentkezés** párbeszédpanel, kattintson a **új tanúsítvány hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="80c90-297">On the **Single Sign-On** dialog, click **Add New Certificate**.</span></span>
   
    <span data-ttu-id="80c90-298">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-298">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694973ex.png "Configure single sign-on")</span></span>
10. <span data-ttu-id="80c90-299">Az a **X.509-tanúsítványokat** párbeszédpanelen hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-299">On the **X.509 Certificates** dialog, perform the following steps:</span></span>
    
    <span data-ttu-id="80c90-300">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-300">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694975.png "Configure single sign-on")</span></span>
    
    <span data-ttu-id="80c90-301">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-301">a.</span></span> <span data-ttu-id="80c90-302">Az a **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **TestSAML2.0**).</span><span class="sxs-lookup"><span data-stu-id="80c90-302">In the **Name** textbox, type a name for your configuration (e.g.: **TestSAML2.0**).</span></span>
    
    <span data-ttu-id="80c90-303">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-303">b.</span></span> <span data-ttu-id="80c90-304">Válassza ki **aktív**.</span><span class="sxs-lookup"><span data-stu-id="80c90-304">Select **Active**.</span></span>
    
    <span data-ttu-id="80c90-305">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-305">c.</span></span> <span data-ttu-id="80c90-306">Mint **formátum**, jelölje be **PEM**.</span><span class="sxs-lookup"><span data-stu-id="80c90-306">As **Format**, select **PEM**.</span></span>
    
    <span data-ttu-id="80c90-307">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-307">d.</span></span> <span data-ttu-id="80c90-308">Mint **típus**, jelölje be **megbízható tároló Cert**.</span><span class="sxs-lookup"><span data-stu-id="80c90-308">As **Type**, select **Trust Store Cert**.</span></span>
    
    <span data-ttu-id="80c90-309">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-309">e.</span></span> <span data-ttu-id="80c90-310">Hozzon létre egy Base64-kódolású fájlt a letöltött tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="80c90-310">Create a Base64 encoded file from your downloaded certificate.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="80c90-311">További részletekért lásd: [bináris tanúsítvány szöveg fájlba való konvertálása](http://youtu.be/PlgrzUZ-Y1o).</span><span class="sxs-lookup"><span data-stu-id="80c90-311">For more details, see [How to convert a binary certificate into a text file](http://youtu.be/PlgrzUZ-Y1o).</span></span>
    > 
    > 
    
    <span data-ttu-id="80c90-312">f.</span><span class="sxs-lookup"><span data-stu-id="80c90-312">f.</span></span> <span data-ttu-id="80c90-313">A Base64 kódolású tanúsítvány megnyitása a Jegyzettömbben, annak tartalmának másolása a vágólapra és illessze be azt a **PEM tanúsítvány** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-313">Open your Base64 encoded certificate in notepad, copy the content of it into your clipboard, and then paste it to the **PEM Certificate** textbox.</span></span>
    
    <span data-ttu-id="80c90-314">g.</span><span class="sxs-lookup"><span data-stu-id="80c90-314">g.</span></span> <span data-ttu-id="80c90-315">Kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="80c90-315">Click **Update**.</span></span>
11. <span data-ttu-id="80c90-316">Az a **egyszeri bejelentkezés** párbeszédpanel, kattintson a **hozzáadása új IdP**.</span><span class="sxs-lookup"><span data-stu-id="80c90-316">On the **Single Sign-On** dialog, click **Add New IdP**.</span></span>
    
    <span data-ttu-id="80c90-317">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-317">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694976ex.png "Configure single sign-on")</span></span>
12. <span data-ttu-id="80c90-318">Az a **hozzáadása új identitásszolgáltató** párbeszédpanelen, a **identitásszolgáltató konfigurálása**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-318">On the **Add New Identity Provider** dialog, under **Configure Identity Provider**, perform the following steps:</span></span>
    
    <span data-ttu-id="80c90-319">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-319">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694982ex.png "Configure single sign-on")</span></span>

    <span data-ttu-id="80c90-320">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-320">a.</span></span> <span data-ttu-id="80c90-321">az a **neve** szövegmező, adja meg a konfiguráció nevét (pl.: **SAML 2.0**).</span><span class="sxs-lookup"><span data-stu-id="80c90-321">In the **Name** textbox, type a name for your configuration (e.g.: **SAML 2.0**).</span></span>

    <span data-ttu-id="80c90-322">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-322">b.</span></span> <span data-ttu-id="80c90-323">A klasszikus Azure AD portálon, másolja a **identitás Szolgáltatóazonosító** értékét, és illessze be azt a **identitási szolgáltató URL-cím** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-323">In the Azure AD classic portal, copy the **Identity Provider ID** value, and then paste it into the **Identity Provider URL** textbox.</span></span>

    <span data-ttu-id="80c90-324">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-324">c.</span></span> <span data-ttu-id="80c90-325">A klasszikus Azure AD portálon, másolja a **hitelesítési kérelem URL-cím** értékét, és illessze be azt a **identitásszolgáltató AuthnRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-325">In the Azure AD classic portal, copy the **Authentication Request URL** value, and then paste it into the **Identity Provider's AuthnRequest** textbox.</span></span>

    <span data-ttu-id="80c90-326">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-326">d.</span></span> <span data-ttu-id="80c90-327">A klasszikus Azure AD portálon, másolja a **egyetlen Sign-Out URL-címe** értékét, és illessze be azt a **identitásszolgáltató SingleLogoutRequest** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="80c90-327">In the Azure AD classic portal, copy the **Single Sign-Out Service URL** value, and then paste it into the **Identity Provider's SingleLogoutRequest** textbox.</span></span>

    <span data-ttu-id="80c90-328">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-328">e.</span></span> <span data-ttu-id="80c90-329">Mint **szolgáltató Identitástanúsítvány**, válassza ki az előző lépésben létrehozott tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="80c90-329">As **Identity Provider Certificate**, select the certificate you have created in the previous step.</span></span>


1. <span data-ttu-id="80c90-330">Kattintson a **speciális beállítások**, majd a **további identitás szolgáltató tulajdonságai**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-330">Click **Advanced Settings**, and under **Additional Identity Provider Properties**, perform the following steps:</span></span>
   
    <span data-ttu-id="80c90-331">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-331">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694983ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="80c90-332">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-332">a.</span></span> <span data-ttu-id="80c90-333">Az a **protokoll kötése az IDP SingleLogoutRequest** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:2.0:bindings:HTTP-átirányítási**.</span><span class="sxs-lookup"><span data-stu-id="80c90-333">In the **Protocol Binding for the IDP's SingleLogoutRequest** textbox, type **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.</span></span>
   
    <span data-ttu-id="80c90-334">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-334">b.</span></span> <span data-ttu-id="80c90-335">Az a **NameID házirend** szövegmezőhöz típus **urn: oasis: nevek: tc: SAML:1.1:nameid-formátum: nem meghatározott**.</span><span class="sxs-lookup"><span data-stu-id="80c90-335">In the **NameID Policy** textbox, type **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.</span></span>    
   
    <span data-ttu-id="80c90-336">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-336">c.</span></span> <span data-ttu-id="80c90-337">Az a **AuthnContextClassRef metódus**, típus **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span><span class="sxs-lookup"><span data-stu-id="80c90-337">In the **AuthnContextClassRef Method**, type **http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password**.</span></span>
   
    <span data-ttu-id="80c90-338">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-338">d.</span></span> <span data-ttu-id="80c90-339">Kapcsolja ki **hozzon létre egy AuthnContextClass**.</span><span class="sxs-lookup"><span data-stu-id="80c90-339">Deselect **Create an AuthnContextClass**.</span></span>

2. <span data-ttu-id="80c90-340">A **további szolgáltató tulajdonságai**, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-340">Under **Additional Service Provider Properties**, perform the following steps:</span></span>
   
    <span data-ttu-id="80c90-341">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-341">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/ic7694984ex.png "Configure single sign-on")</span></span>
   
    <span data-ttu-id="80c90-342">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-342">a.</span></span> <span data-ttu-id="80c90-343">Az a **ServiceNow kezdőlap** szövegmezőhöz a ServiceNow példány kezdőlap URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-343">In the **ServiceNow Homepage** textbox, type the URL of your ServiceNow instance homepage.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="80c90-344">A ServiceNow példány kezdőlapon összefűzése a **ServieNow bérlői URL-cím** és **/navpage.do** (pl.: `https://fabrikam.service-now.com/navpage.do`).</span><span class="sxs-lookup"><span data-stu-id="80c90-344">The ServiceNow instance homepage is a concatenation of your **ServieNow tenant URL** and **/navpage.do** (e.g.: `https://fabrikam.service-now.com/navpage.do`).</span></span>
    > 
    > 
   
    <span data-ttu-id="80c90-345">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-345">b.</span></span> <span data-ttu-id="80c90-346">Az a **Entitásazonosító / kibocsátó** szövegmezőhöz a ServiceNow bérlői URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-346">In the **Entity ID / Issuer** textbox, type the URL of your ServiceNow tenant.</span></span>
   
    <span data-ttu-id="80c90-347">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-347">c.</span></span> <span data-ttu-id="80c90-348">Az a **célközönség URI** szövegmezőhöz a ServiceNow bérlői URL-címét.</span><span class="sxs-lookup"><span data-stu-id="80c90-348">In the **Audience URI** textbox, type the URL of your ServiceNow tenant.</span></span> 
   
    <span data-ttu-id="80c90-349">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-349">d.</span></span> <span data-ttu-id="80c90-350">A **óra döntés** szövegmezőhöz típus **60**.</span><span class="sxs-lookup"><span data-stu-id="80c90-350">In **Clock Skew** textbox, type **60**.</span></span>
   
    <span data-ttu-id="80c90-351">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-351">e.</span></span> <span data-ttu-id="80c90-352">Az a **felhasználói mező** szövegmezőhöz típus **e-mail** vagy **felhasználónév**, attól függően, melyik mezőt a ServiceNow környezetben a felhasználók egyedi azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="80c90-352">In the **User Field** textbox, type **email** or **user_name**, depending on which field is used to uniquely identify users in your ServiceNow deployment.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="80c90-353">Az Azure AD configue hozható létre az Azure AD felhasználói azonosító (egyszerű felhasználónév) vagy az e-mail cím a SAML-jogkivonat egyedi azonosítóként a is a **ServiceNow > attribútumok > egyszeri bejelentkezés** a klasszikus Azure portálra, és a kívánt mezőt leképezési a **nameidentifier** attribútum.</span><span class="sxs-lookup"><span data-stu-id="80c90-353">You can configue Azure AD to emit either the Azure AD user ID (user principal name) or the email address as the unique identifier in the SAML token by going to the **ServiceNow > Attributes > Single Sign-On** section of the Azure classic portal and mapping the desired field to the **nameidentifier** attribute.</span></span> <span data-ttu-id="80c90-354">A kiválasztott attribútumnak az Azure AD (pl. egyszerű felhasználónév) tárolt érték meg kell egyeznie a megadott mező (pl. felhasználónév) ServiceNow tárolt érték</span><span class="sxs-lookup"><span data-stu-id="80c90-354">The value stored for the selected attribute in Azure AD (e.g. user principal name) must match the value stored in ServiceNow for the entered field (e.g. user_name)</span></span>
    > 
    > 
   
    <span data-ttu-id="80c90-355">f.</span><span class="sxs-lookup"><span data-stu-id="80c90-355">f.</span></span> <span data-ttu-id="80c90-356">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-356">Click **Save**.</span></span> 

3. <span data-ttu-id="80c90-357">A klasszikus Azure AD portálon válassza ki az egyszeri bejelentkezés konfigurációs megerősítő, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="80c90-357">On the Azure AD classic portal, select the single sign-on configuration confirmation, and then click **Next**.</span></span> 
   
    <span data-ttu-id="80c90-358">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-358">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694990.png "Configure single sign-on")</span></span>

4. <span data-ttu-id="80c90-359">Az a **az egyszeri bejelentkezés megerősítő** kattintson **Complete**.</span><span class="sxs-lookup"><span data-stu-id="80c90-359">On the **Single sign-on confirmation** page, click **Complete**.</span></span>
   
    <span data-ttu-id="80c90-360">![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "egyszeri bejelentkezés konfigurálása")</span><span class="sxs-lookup"><span data-stu-id="80c90-360">![Configure single sign-on](./media/active-directory-saas-servicenow-tutorial/IC7694991.png "Configure single sign-on")</span></span>

## <a name="configuring-user-provisioning"></a><span data-ttu-id="80c90-361">Felhasználók átadására</span><span class="sxs-lookup"><span data-stu-id="80c90-361">Configuring user provisioning</span></span>
<span data-ttu-id="80c90-362">Ez a szakasz célja engedélyezése a felhasználók átadása, az Active Directory felhasználói fiókokat a ServiceNow felvázoló.</span><span class="sxs-lookup"><span data-stu-id="80c90-362">The objective of this section is to outline how to enable user provisioning of Active Directory user accounts to ServiceNow.</span></span>

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a><span data-ttu-id="80c90-363">Adja meg a felhasználók átadása, hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80c90-363">To configure user provisioning, perform the following steps:</span></span>
1. <span data-ttu-id="80c90-364">A klasszikus Azure felügyeleti portálon a a **ServiceNow** alkalmazás integráció lapján, kattintson a **konfigurálja, a felhasználók átadása**.</span><span class="sxs-lookup"><span data-stu-id="80c90-364">In the Azure Management classic portal, on the **ServiceNow** application integration page, click **Configure user provisioning**.</span></span> 
   
    <span data-ttu-id="80c90-365">![A felhasználók átadása](./media/active-directory-saas-servicenow-tutorial/IC769498.png "a felhasználók átadása")</span><span class="sxs-lookup"><span data-stu-id="80c90-365">![User provisioning](./media/active-directory-saas-servicenow-tutorial/IC769498.png "User provisioning")</span></span>

2. <span data-ttu-id="80c90-366">Az a **ServiceNow hitelesítő adatait ahhoz, hogy a felhasználók automatikus átadása** lapján adja meg a következő konfigurációs beállításokat:</span><span class="sxs-lookup"><span data-stu-id="80c90-366">On the **Enter your ServiceNow credentials to enable automatic user provisioning** page, provide the following configuration settings:</span></span>
   
     <span data-ttu-id="80c90-367">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-367">a.</span></span> <span data-ttu-id="80c90-368">Az a **ServiceNow példánynév** szövegmező, írja be a ServiceNow-példány nevét.</span><span class="sxs-lookup"><span data-stu-id="80c90-368">In the **ServiceNow Instance Name** textbox, type the ServiceNow instance name.</span></span>
   
     <span data-ttu-id="80c90-369">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-369">b.</span></span> <span data-ttu-id="80c90-370">Az a **ServiceNow rendszergazda felhasználóneve** szövegmező, írja be a ServiceNow rendszergazda fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="80c90-370">In the **ServiceNow Admin User Name** textbox, type the name of the ServiceNow admin account.</span></span>
   
     <span data-ttu-id="80c90-371">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-371">c.</span></span> <span data-ttu-id="80c90-372">Az a **ServiceNow rendszergazdai jelszó** szövegmező, írja be a fiókhoz tartozó jelszót.</span><span class="sxs-lookup"><span data-stu-id="80c90-372">In the **ServiceNow Admin Password** textbox, type the password for this account.</span></span>
   
     <span data-ttu-id="80c90-373">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-373">d.</span></span> <span data-ttu-id="80c90-374">Kattintson a **érvényesítése** a konfiguráció ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="80c90-374">Click **validate** to verify your configuration.</span></span>
   
     <span data-ttu-id="80c90-375">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-375">e.</span></span> <span data-ttu-id="80c90-376">Kattintson a **következő** gombra kattintva nyissa meg a **további lépések** lap.</span><span class="sxs-lookup"><span data-stu-id="80c90-376">Click the **Next** button to open the **Next steps** page.</span></span>
   
     <span data-ttu-id="80c90-377">f.</span><span class="sxs-lookup"><span data-stu-id="80c90-377">f.</span></span> <span data-ttu-id="80c90-378">Ha minden felhasználó számára az alkalmazás telepítéséhez, jelölje be "**a könyvtárban ezen alkalmazás összes felhasználói fiók automatikusan létesítsen**".</span><span class="sxs-lookup"><span data-stu-id="80c90-378">If you want to provision all users to this application, select “**Automatically provision all user accounts in the directory to this application**”.</span></span> 
   
    <span data-ttu-id="80c90-379">![További lépések](./media/active-directory-saas-servicenow-tutorial/IC698804.png "további lépések")</span><span class="sxs-lookup"><span data-stu-id="80c90-379">![Next Steps](./media/active-directory-saas-servicenow-tutorial/IC698804.png "Next Steps")</span></span>
   
     <span data-ttu-id="80c90-380">g.</span><span class="sxs-lookup"><span data-stu-id="80c90-380">g.</span></span> <span data-ttu-id="80c90-381">Az a **további lépések** kattintson **Complete** kattintva mentse a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="80c90-381">On the **Next steps** page, click **Complete** to save your configuration.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="80c90-382">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="80c90-382">Creating an Azure AD test user</span></span>
<span data-ttu-id="80c90-383">Ebben a szakaszban Britta Simon neve a klasszikus portálon tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="80c90-383">In this section, you create a test user in the classic portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][20]

<span data-ttu-id="80c90-385">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="80c90-385">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="80c90-386">Az a **a klasszikus Azure portálon**, a bal oldali navigációs ablaktábláján kattintson **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="80c90-386">In the **Azure classic portal**, on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_09.png) 

2. <span data-ttu-id="80c90-388">Az a **Directory** listára, válassza ki a könyvtárat, amelyhez a címtár-integrációs engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="80c90-388">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>

3. <span data-ttu-id="80c90-389">A felhasználók listájának megjelenítéséhez a felső menüben, kattintson a **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="80c90-389">To display the list of users, in the menu on the top, click **Users**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="80c90-391">Lehetőségre a **felhasználó hozzáadása** párbeszédpanel alján, az eszköztárban kattintson **felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="80c90-391">To open the **Add User** dialog, in the toolbar on the bottom, click **Add User**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_04.png) 

5. <span data-ttu-id="80c90-393">Az a **adja meg azt a felhasználó** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="80c90-393">On the **Tell us about this user** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_05.png) 
   
    <span data-ttu-id="80c90-395">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-395">a.</span></span> <span data-ttu-id="80c90-396">A felhasználó típusát válassza ki az új felhasználót a szervezetében.</span><span class="sxs-lookup"><span data-stu-id="80c90-396">As Type Of User, select New user in your organization.</span></span>
   
    <span data-ttu-id="80c90-397">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-397">b.</span></span> <span data-ttu-id="80c90-398">A felhasználó nevében **szövegmező**, típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="80c90-398">In the User Name **textbox**, type **BrittaSimon**.</span></span>
   
    <span data-ttu-id="80c90-399">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-399">c.</span></span> <span data-ttu-id="80c90-400">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-400">Click **Next**.</span></span>

6. <span data-ttu-id="80c90-401">Az a **felhasználói profil** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="80c90-401">On the **User Profile** dialog page, perform the following steps:</span></span>
   
   ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_06.png) 
   
   <span data-ttu-id="80c90-403">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-403">a.</span></span> <span data-ttu-id="80c90-404">Az a **Utónév** szövegmezőhöz típus **Britta**.</span><span class="sxs-lookup"><span data-stu-id="80c90-404">In the **First Name** textbox, type **Britta**.</span></span>  
   
   <span data-ttu-id="80c90-405">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-405">b.</span></span> <span data-ttu-id="80c90-406">Az a **Vezetéknév** szövegmezőhöz típusa, **Simon**.</span><span class="sxs-lookup"><span data-stu-id="80c90-406">In the **Last Name** textbox, type, **Simon**.</span></span>
   
   <span data-ttu-id="80c90-407">c.</span><span class="sxs-lookup"><span data-stu-id="80c90-407">c.</span></span> <span data-ttu-id="80c90-408">Az a **megjelenített név** szövegmezőhöz típus **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="80c90-408">In the **Display Name** textbox, type **Britta Simon**.</span></span>
   
   <span data-ttu-id="80c90-409">d.</span><span class="sxs-lookup"><span data-stu-id="80c90-409">d.</span></span> <span data-ttu-id="80c90-410">Az a **szerepkör** listáról válassza ki **felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="80c90-410">In the **Role** list, select **User**.</span></span>
   
   <span data-ttu-id="80c90-411">e.</span><span class="sxs-lookup"><span data-stu-id="80c90-411">e.</span></span> <span data-ttu-id="80c90-412">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-412">Click **Next**.</span></span>

7. <span data-ttu-id="80c90-413">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lap, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="80c90-413">On the **Get temporary password** dialog page, click **create**.</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_07.png) 

8. <span data-ttu-id="80c90-415">Az a **ideiglenes jelszó beszerzése** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="80c90-415">On the **Get temporary password** dialog page, perform the following steps:</span></span>
   
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicenow-tutorial/create_aaduser_08.png) 
   
    <span data-ttu-id="80c90-417">a.</span><span class="sxs-lookup"><span data-stu-id="80c90-417">a.</span></span> <span data-ttu-id="80c90-418">Jegyezze fel az értéket a **új jelszó**.</span><span class="sxs-lookup"><span data-stu-id="80c90-418">Write down the value of the **New Password**.</span></span>
   
    <span data-ttu-id="80c90-419">b.</span><span class="sxs-lookup"><span data-stu-id="80c90-419">b.</span></span> <span data-ttu-id="80c90-420">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="80c90-420">Click **Complete**.</span></span>   

### <a name="creating-a-servicenow-test-user"></a><span data-ttu-id="80c90-421">A ServiceNow tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="80c90-421">Creating a ServiceNow test user</span></span>
<span data-ttu-id="80c90-422">Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="80c90-422">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="80c90-423">Ebben a szakaszban a ServiceNow Britta Simon nevű felhasználó hoz létre.</span><span class="sxs-lookup"><span data-stu-id="80c90-423">In this section, you create a user called Britta Simon in ServiceNow.</span></span> <span data-ttu-id="80c90-424">Ha nem tudja a felhasználó hozzáadása a ServiceNow vagy ServiceNow Express fiókját, lépjen kapcsolatba a terméktámogatással a ServiceNow.</span><span class="sxs-lookup"><span data-stu-id="80c90-424">If you don't know how to add a user in your ServiceNow or ServiceNow Express account, contact ServiceNow support team.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="80c90-425">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="80c90-425">Assigning the Azure AD test user</span></span>
<span data-ttu-id="80c90-426">Ebben a szakaszban engedélyezze Britta Simon szerint saját hozzáférést biztosít a ServiceNow Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="80c90-426">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to ServiceNow.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="80c90-428">**A ServiceNow Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="80c90-428">**To assign Britta Simon to ServiceNow, perform the following steps:**</span></span>

1. <span data-ttu-id="80c90-429">A klasszikus portál megnyitásához az alkalmazások megtekintése a könyvtár nézetben kattintson **alkalmazások** a felső menüben.</span><span class="sxs-lookup"><span data-stu-id="80c90-429">On the classic portal, to open the applications view, in the directory view, click **Applications** in the top menu.</span></span>
   
    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="80c90-431">Az alkalmazások listában válassza ki a **ServiceNow**.</span><span class="sxs-lookup"><span data-stu-id="80c90-431">In the applications list, select **ServiceNow**.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicenow-tutorial/tutorial_servicenow_10.png) 

3. <span data-ttu-id="80c90-433">Kattintson a felső menüben **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="80c90-433">In the menu on the top, click **Users**.</span></span>
   
    ![Felhasználó hozzárendelése][203] 

4. <span data-ttu-id="80c90-435">Válassza a minden felhasználó lista **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="80c90-435">In the All Users list, select **Britta Simon**.</span></span>

5. <span data-ttu-id="80c90-436">Kattintson az alsó eszköztár **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="80c90-436">In the toolbar on the bottom, click **Assign**.</span></span>
   
    ![Felhasználó hozzárendelése][205]

### <a name="testing-single-sign-on"></a><span data-ttu-id="80c90-438">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="80c90-438">Testing single sign-on</span></span>
<span data-ttu-id="80c90-439">Ez a szakasz célja tesztelése az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen.</span><span class="sxs-lookup"><span data-stu-id="80c90-439">The objective of this section is to test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="80c90-440">Ha a hozzáférési panelen a ServiceNow csempére kattint, akkor kell beolvasása automatikusan bejelentkezett a ServiceNow alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="80c90-440">When you click the ServiceNow tile in the Access Panel, you should get automatically signed-on to your ServiceNow application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80c90-441">További források</span><span class="sxs-lookup"><span data-stu-id="80c90-441">Additional resources</span></span>
* [<span data-ttu-id="80c90-442">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="80c90-442">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="80c90-443">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="80c90-443">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_04.png


[5]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_06.png
[7]:  ./media/active-directory-saas-servicenow-tutorial/tutorial_general_050.png
[10]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_060.png
[11]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_070.png
[20]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-servicenow-tutorial/tutorial_general_205.png
