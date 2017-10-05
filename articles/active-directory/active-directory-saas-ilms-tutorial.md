---
title: "Oktatóanyag: Azure Active Directoryval integrált iLMS |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és iLMS között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: d6e11639-6cea-48c9-b008-246cf686e726
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: jeedes
ms.openlocfilehash: 22c72020200138e78835ed7dd2661f18b824c785
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-ilms"></a><span data-ttu-id="a608b-103">Oktatóanyag: Azure Active Directoryval integrált iLMS</span><span class="sxs-lookup"><span data-stu-id="a608b-103">Tutorial: Azure Active Directory integration with iLMS</span></span>

<span data-ttu-id="a608b-104">Ebben az oktatóanyagban elsajátíthatja iLMS integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a608b-104">In this tutorial, you learn how to integrate iLMS with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a608b-105">ILMS integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="a608b-105">Integrating iLMS with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="a608b-106">Megadhatja a iLMS hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a608b-106">You can control in Azure AD who has access to iLMS</span></span>
- <span data-ttu-id="a608b-107">Engedélyezheti a felhasználóknak, hogy automatikusan lekérni aláírt a iLMS (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a608b-107">You can enable your users to automatically get signed-on to iLMS (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a608b-108">Kezelheti a fiókokat, egy központi helyen – az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a608b-108">You can manage your accounts in one central location - the Azure portal</span></span>

<span data-ttu-id="a608b-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a608b-109">If you want to know more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a608b-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a608b-110">Prerequisites</span></span>

<span data-ttu-id="a608b-111">Konfigurálása az Azure AD-integrációs iLMS, a következőkre van szükség:</span><span class="sxs-lookup"><span data-stu-id="a608b-111">To configure Azure AD integration with iLMS, you need the following items:</span></span>

- <span data-ttu-id="a608b-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a608b-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a608b-113">Egy iLMS egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="a608b-113">An iLMS single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="a608b-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="a608b-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="a608b-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="a608b-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a608b-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a608b-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="a608b-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a608b-117">If you don't have an Azure AD trial environment, you can get a one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a608b-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a608b-118">Scenario description</span></span>
<span data-ttu-id="a608b-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a608b-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a608b-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a608b-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a608b-121">A gyűjteményből iLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a608b-121">Adding iLMS from the gallery</span></span>
2. <span data-ttu-id="a608b-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a608b-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-ilms-from-the-gallery"></a><span data-ttu-id="a608b-123">A gyűjteményből iLMS hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a608b-123">Adding iLMS from the gallery</span></span>
<span data-ttu-id="a608b-124">Az Azure AD integrálása a iLMS konfigurálásához kell hozzáadnia iLMS a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="a608b-124">To configure the integration of iLMS into Azure AD, you need to add iLMS from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="a608b-125">**A gyűjteményből iLMS hozzáadásához hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a608b-125">**To add iLMS from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="a608b-126">Az a  **[Azure-portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a608b-126">In the **[Azure portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="a608b-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a608b-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="a608b-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a608b-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="a608b-131">Új alkalmazás hozzáadásához kattintson **új alkalmazás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="a608b-131">To add new application, click **New application** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="a608b-133">Írja be a keresőmezőbe, **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="a608b-133">In the search box, type **iLMS**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_search.png)

5. <span data-ttu-id="a608b-135">Az eredmények panelen válassza ki a **iLMS**, majd kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a608b-135">In the results panel, select **iLMS**, then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a608b-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a608b-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a608b-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján iLMS.</span><span class="sxs-lookup"><span data-stu-id="a608b-138">In this section, you configure and test Azure AD single sign-on with iLMS based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="a608b-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó iLMS a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a608b-139">For single sign-on to work, Azure AD needs to know what the counterpart user in iLMS is to a user in Azure AD.</span></span> <span data-ttu-id="a608b-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a iLMS közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="a608b-140">In other words, a link relationship between an Azure AD user and the related user in iLMS needs to be established.</span></span>

<span data-ttu-id="a608b-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** iLMS a.</span><span class="sxs-lookup"><span data-stu-id="a608b-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in iLMS.</span></span>

<span data-ttu-id="a608b-142">Az Azure AD egyszeri bejelentkezést a iLMS tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="a608b-142">To configure and test Azure AD single sign-on with iLMS, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="a608b-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="a608b-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="a608b-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="a608b-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a608b-145">**[Egy iLMS tesztfelhasználó létrehozása](#creating-an-ilms-test-user)**  - kell rendelkeznie egy Britta Simon megfelelője a iLMS, amely csatolva van rá, hogy az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a608b-145">**[Creating an iLMS test user](#creating-an-ilms-test-user)** - to have a counterpart of Britta Simon in iLMS that is linked to the Azure AD representation of her.</span></span>
4. <span data-ttu-id="a608b-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a608b-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a608b-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="a608b-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a608b-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a608b-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a608b-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure portálon, és konfigurálása egyszeri bejelentkezéshez az iLMS alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a608b-149">In this section, you enable Azure AD single sign-on in the Azure portal and configure single sign-on in your iLMS application.</span></span>

<span data-ttu-id="a608b-150">**Konfigurálása az Azure AD az egyszeri bejelentkezés iLMS, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a608b-150">**To configure Azure AD single sign-on with iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="a608b-151">Az Azure portálon a a **iLMS** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a608b-151">In the Azure portal, on the **iLMS** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a608b-153">Az a **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a608b-153">On the **Single sign-on** dialog, select **Mode** as **SAML-based Sign-on** to enable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_samlbase.png)

3. <span data-ttu-id="a608b-155">Az a **iLMS tartomány és az URL-címek** területen tegye a következőket, ha szeretne beállítani az alkalmazás **IDP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a608b-155">On the **iLMS Domain and URLs** section, perform the following steps if you wish to configure the application in **IDP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url.png)

    <span data-ttu-id="a608b-157">a.</span><span class="sxs-lookup"><span data-stu-id="a608b-157">a.</span></span> <span data-ttu-id="a608b-158">Az a **azonosítója** szövegmező, illessze be a **azonosítója** értéket másol a **szolgáltató** iLMS felügyeleti portál SAML-beállítások szakasza.</span><span class="sxs-lookup"><span data-stu-id="a608b-158">In the **Identifier** textbox, paste the **Identifier** value you copy from **Service Provider** section of SAML settings in iLMS admin portal.</span></span>

    <span data-ttu-id="a608b-159">b.</span><span class="sxs-lookup"><span data-stu-id="a608b-159">b.</span></span> <span data-ttu-id="a608b-160">Az a **válasz URL-CÍMEN** szövegmező, illessze be a **végpont URL-** értéket másol a **szolgáltató** szakasz iLMS felügyeleti portál, hogy a következő mintát SAML-beállítások`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="a608b-160">In the **Reply URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal having the following pattern `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>

    >[!Note]
    ><span data-ttu-id="a608b-161">Ez 123456 példa érték azonosító.</span><span class="sxs-lookup"><span data-stu-id="a608b-161">This '123456' is an example value of identifier.</span></span>

4. <span data-ttu-id="a608b-162">Ellenőrizze **megjelenítése speciális URL-beállításainak**, ha szeretne beállítani az alkalmazás **SP** kezdeményezett mód:</span><span class="sxs-lookup"><span data-stu-id="a608b-162">Check **Show advanced URL settings**, if you wish to configure the application in **SP** initiated mode:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_url1.png)

    <span data-ttu-id="a608b-164">Az a **bejelentkezési URL-cím** szövegmező, illessze be a **végpont URL-** értéket másol a **szolgáltató** szakasz iLMS felügyeleti portálon SAML-beállítások`https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span><span class="sxs-lookup"><span data-stu-id="a608b-164">In the **Sign-on URL** textbox, paste the **Endpoint (URL)** value you copy from **Service Provider** section of SAML settings in iLMS admin portal as `https://www.inspiredlms.com/Login/<instanceName>/consumer.aspx`</span></span>     

5. <span data-ttu-id="a608b-165">Igény szerinti kiépítés engedélyezéséhez iLMS alkalmazás vár a SAML helyességi feltételek egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="a608b-165">To enable JIT provisioning, iLMS application expects the SAML assertions in a specific format.</span></span> <span data-ttu-id="a608b-166">A következő jogcímek alkalmazás konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="a608b-166">Configure the following claims for this application.</span></span> <span data-ttu-id="a608b-167">Ezek az attribútumok értékének kezelheti a **felhasználói attribútumok** szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="a608b-167">You can manage the values of these attributes from the **User Attributes** section on application integration page.</span></span> <span data-ttu-id="a608b-168">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="a608b-168">The following screenshot shows an example for this.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/4.png)
    
    <span data-ttu-id="a608b-170">Hozzon létre **részleg, régió** és **osztás** attribútumok közül, adja hozzá ezek az attribútumok nevét iLMS.</span><span class="sxs-lookup"><span data-stu-id="a608b-170">Create **Department, Region** and **Division** attributes and add the name of these attributes in iLMS.</span></span> <span data-ttu-id="a608b-171">A fent látható összes attribútum megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="a608b-171">All these attributes shown above are required.</span></span>  

    > [!NOTE] 
    > <span data-ttu-id="a608b-172">Engedélyezni kell **Un-recognized felhasználói fiók létrehozása** a iLMS ezek az attribútumok hozzárendelését.</span><span class="sxs-lookup"><span data-stu-id="a608b-172">You have to enable **Create Un-recognized User Account** in iLMS to map these attributes.</span></span> <span data-ttu-id="a608b-173">Kövesse az utasításokat [Itt](http://support.inspiredelearning.com/customer/portal/articles/2204526) attribútumok konfigurálása képet kapjon.</span><span class="sxs-lookup"><span data-stu-id="a608b-173">Follow the instructions [here](http://support.inspiredelearning.com/customer/portal/articles/2204526) to get an idea on the attributes configuration.</span></span>

6. <span data-ttu-id="a608b-174">A a **felhasználói attribútumok** a szakasz a **egyszeri bejelentkezés** párbeszédpanelen konfigurálja a SAML-jogkivonat attribútum, a fenti ábrán látható módon, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a608b-174">In the **User Attributes** section on the **Single sign-on** dialog, configure SAML token attribute as shown in the image above and perform the following steps:</span></span>
    
    | <span data-ttu-id="a608b-175">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="a608b-175">Attribute Name</span></span> | <span data-ttu-id="a608b-176">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="a608b-176">Attribute Value</span></span> |
    | ---------------| --------------- |    
    | <span data-ttu-id="a608b-177">osztály</span><span class="sxs-lookup"><span data-stu-id="a608b-177">division</span></span> | <span data-ttu-id="a608b-178">felhasználó.részleg</span><span class="sxs-lookup"><span data-stu-id="a608b-178">user.department</span></span> |
    | <span data-ttu-id="a608b-179">Régió</span><span class="sxs-lookup"><span data-stu-id="a608b-179">region</span></span> | <span data-ttu-id="a608b-180">User.state</span><span class="sxs-lookup"><span data-stu-id="a608b-180">user.state</span></span> |
    | <span data-ttu-id="a608b-181">Szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="a608b-181">department</span></span> | <span data-ttu-id="a608b-182">User.jobtitle</span><span class="sxs-lookup"><span data-stu-id="a608b-182">user.jobtitle</span></span> |

    <span data-ttu-id="a608b-183">a.</span><span class="sxs-lookup"><span data-stu-id="a608b-183">a.</span></span> <span data-ttu-id="a608b-184">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a608b-184">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_05.png)
    
    <span data-ttu-id="a608b-187">b.</span><span class="sxs-lookup"><span data-stu-id="a608b-187">b.</span></span> <span data-ttu-id="a608b-188">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="a608b-188">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="a608b-189">c.</span><span class="sxs-lookup"><span data-stu-id="a608b-189">c.</span></span> <span data-ttu-id="a608b-190">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="a608b-190">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="a608b-191">d.</span><span class="sxs-lookup"><span data-stu-id="a608b-191">d.</span></span> <span data-ttu-id="a608b-192">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="a608b-192">Click **Ok**</span></span>

7. <span data-ttu-id="a608b-193">Az a **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse az XML-fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a608b-193">On the **SAML Signing Certificate** section, click **Metadata XML** and then save the XML file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_certificate.png) 

8. <span data-ttu-id="a608b-195">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a608b-195">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-iLMS-tutorial/tutorial_general_400.png)

9. <span data-ttu-id="a608b-197">Egy másik webes böngészőablakban, jelentkezzen be a **iLMS felügyeleti portál** rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a608b-197">In a different web browser window, log in to your **iLMS admin portal** as an administrator.</span></span>

10. <span data-ttu-id="a608b-198">Kattintson a **SSO:SAML** alatt **beállítások** lapon nyissa meg a SAML-beállítások, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a608b-198">Click **SSO:SAML** under **Settings** tab to open SAML settings and perform the following steps:</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/1.png) 

    <span data-ttu-id="a608b-200">a.</span><span class="sxs-lookup"><span data-stu-id="a608b-200">a.</span></span> <span data-ttu-id="a608b-201">Bontsa ki a **szolgáltató** szakaszt, és másolja a **azonosító** és **végpont URL-** érték.</span><span class="sxs-lookup"><span data-stu-id="a608b-201">Expand the **Service Provider** section and copy the **Identifier** and **Endpoint (URL)** value.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/2.png) 

    <span data-ttu-id="a608b-203">b.</span><span class="sxs-lookup"><span data-stu-id="a608b-203">b.</span></span> <span data-ttu-id="a608b-204">A **identitásszolgáltató** kattintson **metaadatok importálása**.</span><span class="sxs-lookup"><span data-stu-id="a608b-204">Under **Identity Provider** section, click **Import Metadata**.</span></span>
    
    <span data-ttu-id="a608b-205">c.</span><span class="sxs-lookup"><span data-stu-id="a608b-205">c.</span></span> <span data-ttu-id="a608b-206">Válassza ki a **metaadatok** az Azure-portálról letöltött fájl **SAML-aláíró tanúsítványa** szakasz.</span><span class="sxs-lookup"><span data-stu-id="a608b-206">Select the **Metadata** file downloaded from Azure Portal from **SAML Signing Certificate** section.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig1.png) 

    <span data-ttu-id="a608b-208">d.</span><span class="sxs-lookup"><span data-stu-id="a608b-208">d.</span></span> <span data-ttu-id="a608b-209">Ha később engedélyezni kívánja történő iLMS fiókokat hozhat létre a un JIT-ismeri fel a felhasználók, kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a608b-209">If you want to enable JIT provisioning to create iLMS accounts for un-recognize users, follow below steps:</span></span>
        
       - <span data-ttu-id="a608b-210">Ellenőrizze **nem felismerhető felhasználói fiók létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a608b-210">Check **Create Un-recognized User Account**.</span></span>
       
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_ssoconfig2.png)

       -  <span data-ttu-id="a608b-212">Az attribútumok hozzárendelését iLMS attribútumokat az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="a608b-212">Map the attributes in Azure AD with the attributes in iLMS.</span></span> <span data-ttu-id="a608b-213">Az attribútum oszlopának adja meg az attribútumok nevét vagy az alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="a608b-213">In the attribute column, specify the attributes name or the default value.</span></span>

    <span data-ttu-id="a608b-214">e.</span><span class="sxs-lookup"><span data-stu-id="a608b-214">e.</span></span> <span data-ttu-id="a608b-215">Ugrás a **üzleti szabályok** lapra, és hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a608b-215">Go to **Business Rules** tab and perform the following steps:</span></span> 
        
       ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/5.png)

       - <span data-ttu-id="a608b-217">Ellenőrizze **Un-recognized régiók létrehozása, az osztályok és a szervezeti egységek** régiók, osztályok és szervezeti egységek, amely már létezik az egyszeri bejelentkezés időpontjában létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="a608b-217">Check **Create Un-recognized Regions, Divisions and Departments** to create Regions, Divisions, and Departments that do not already exist at the time of Single Sign-on.</span></span>
        
       - <span data-ttu-id="a608b-218">Ellenőrizze **frissítés felhasználói profil során bejelentkezés** adhatja meg, hogy a profil frissül minden egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a608b-218">Check **Update User Profile During Sign-in** to specify whether the user’s profile is updated with each Single Sign-on.</span></span> 
        
       - <span data-ttu-id="a608b-219">Ha a **"Frissítés üres értékek a nem kötelező mezők a felhasználói profil"** beállítás be van jelölve, a nem kötelező profil mező üres lesz a bejelentkezés után is iLMS profil üres értékek mezőket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a608b-219">If the **“Update Blank Values for Non Mandatory Fields in User Profile”** option is checked, optional profile fields that are blank upon sign in will also cause the user’s iLMS profile to contain blank values for those fields.</span></span>
        
       - <span data-ttu-id="a608b-220">Ellenőrizze **hiba értesítő E-mail küldése** és adja meg a felhasználó e-mail címét, ahol szeretne kapni a hiba értesítő e-mailt.</span><span class="sxs-lookup"><span data-stu-id="a608b-220">Check **Send Error Notification Email** and enter the email of the user where you want to receive the error notification email.</span></span>

11. <span data-ttu-id="a608b-221">Kattintson a **mentése** gombra a beállítások mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a608b-221">Click **Save** button to save the settings.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="a608b-223">Ezek az utasítások belül tömör verziója most el tudja olvasni a [Azure-portálon](https://portal.azure.com), míg az alkalmazás beállításakor!</span><span class="sxs-lookup"><span data-stu-id="a608b-223">You can now read a concise version of these instructions inside the [Azure portal](https://portal.azure.com), while you are setting up the app!</span></span>  <span data-ttu-id="a608b-224">Ez az alkalmazás a hozzáadása után a **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a **egyszeri bejelentkezés** lapra, és a beágyazott dokumentációja keresztül a **konfigurációs** szakasz alján.</span><span class="sxs-lookup"><span data-stu-id="a608b-224">After adding this app from the **Active Directory > Enterprise Applications** section, simply click the **Single Sign-On** tab and access the embedded documentation through the **Configuration** section at the bottom.</span></span> <span data-ttu-id="a608b-225">További Itt a embedded dokumentációjából szolgáltatásról: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a608b-225">You can read more about the embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
    
### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a608b-226">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a608b-226">Creating an Azure AD test user</span></span>
<span data-ttu-id="a608b-227">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure portálon Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="a608b-227">The objective of this section is to create a test user in the Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a608b-229">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a608b-229">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="a608b-230">Az a **Azure-portálon**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a608b-230">In the **Azure portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a608b-232">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="a608b-232">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a608b-234">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a608b-234">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a608b-236">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="a608b-236">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-ilms-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a608b-238">a.</span><span class="sxs-lookup"><span data-stu-id="a608b-238">a.</span></span> <span data-ttu-id="a608b-239">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a608b-239">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a608b-240">b.</span><span class="sxs-lookup"><span data-stu-id="a608b-240">b.</span></span> <span data-ttu-id="a608b-241">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a608b-241">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a608b-242">c.</span><span class="sxs-lookup"><span data-stu-id="a608b-242">c.</span></span> <span data-ttu-id="a608b-243">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a608b-243">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="a608b-244">d.</span><span class="sxs-lookup"><span data-stu-id="a608b-244">d.</span></span> <span data-ttu-id="a608b-245">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a608b-245">Click **Create**.</span></span>
 
### <a name="creating-an-ilms-test-user"></a><span data-ttu-id="a608b-246">Egy iLMS tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a608b-246">Creating an iLMS test user</span></span>

<span data-ttu-id="a608b-247">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="a608b-247">Application supports Just in time user provisioning and after authentication users are created in the application automatically.</span></span> <span data-ttu-id="a608b-248">Igény szerinti fog működni, ha rákattint a **Un-recognized felhasználói fiók létrehozása** jelölőnégyzet során iLMS felügyeleti portál a SAML-alapú konfigurációs beállítást.</span><span class="sxs-lookup"><span data-stu-id="a608b-248">JIT will work, if you have clicked the **Create Un-recognized User Account** checkbox during SAML configuration setting at iLMS admin portal.</span></span>

<span data-ttu-id="a608b-249">Ha manuálisan hozzon létre egy felhasználó van szüksége, majd hajtsa végre a következő lépések:</span><span class="sxs-lookup"><span data-stu-id="a608b-249">If you need to create an user manually, then follow below steps :</span></span>

1. <span data-ttu-id="a608b-250">Jelentkezzen be rendszergazdaként a iLMS vállalati webhely.</span><span class="sxs-lookup"><span data-stu-id="a608b-250">Log in to your iLMS company site as an administrator.</span></span>

2. <span data-ttu-id="a608b-251">Kattintson a **"Felhasználó regisztrálása"** alatt **felhasználók** elemére kattintva nyissa meg **regisztrálása felhasználói** lap.</span><span class="sxs-lookup"><span data-stu-id="a608b-251">Click **“Register User”** under **Users** tab to open **Register User** page.</span></span> 
   
   ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/3.png)

3. <span data-ttu-id="a608b-253">Az a **"Felhasználó regisztrálása"** lapon, a következő lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="a608b-253">On the **“Register User”** page, perform the following steps.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-ilms-tutorial/create_testuser_add.png)

    <span data-ttu-id="a608b-255">a.</span><span class="sxs-lookup"><span data-stu-id="a608b-255">a.</span></span> <span data-ttu-id="a608b-256">Az a **Keresztnév** szövegmezőhöz Britta az első típusnév.</span><span class="sxs-lookup"><span data-stu-id="a608b-256">In the **First Name** textbox, type the first name Britta.</span></span>
   
    <span data-ttu-id="a608b-257">b.</span><span class="sxs-lookup"><span data-stu-id="a608b-257">b.</span></span> <span data-ttu-id="a608b-258">Az a **Vezetéknév** szövegmező, írja be a vezetéknevet Simon.</span><span class="sxs-lookup"><span data-stu-id="a608b-258">In the **Last Name** textbox, type the last name Simon.</span></span>

    <span data-ttu-id="a608b-259">c.</span><span class="sxs-lookup"><span data-stu-id="a608b-259">c.</span></span> <span data-ttu-id="a608b-260">Az a **E-mail azonosító** szövegmezőhöz Britta Simon fiók e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="a608b-260">In the **Email ID** textbox, type the email address of Britta Simon account.</span></span>

    <span data-ttu-id="a608b-261">d.</span><span class="sxs-lookup"><span data-stu-id="a608b-261">d.</span></span> <span data-ttu-id="a608b-262">Az a **régió** legördülő menüben válassza ki a régiót értékét.</span><span class="sxs-lookup"><span data-stu-id="a608b-262">In the **Region** dropdown, select the value for region.</span></span>

    <span data-ttu-id="a608b-263">e.</span><span class="sxs-lookup"><span data-stu-id="a608b-263">e.</span></span> <span data-ttu-id="a608b-264">Az a **osztás** legördülő menüben válassza ki a részleg értékét.</span><span class="sxs-lookup"><span data-stu-id="a608b-264">In the **Division** dropdown, select the value for division.</span></span>

    <span data-ttu-id="a608b-265">f.</span><span class="sxs-lookup"><span data-stu-id="a608b-265">f.</span></span> <span data-ttu-id="a608b-266">Az a **részleg** legördülő menüben válassza ki a részleg értékét.</span><span class="sxs-lookup"><span data-stu-id="a608b-266">In the **Department** dropdown, select the value for department.</span></span>

    <span data-ttu-id="a608b-267">g.</span><span class="sxs-lookup"><span data-stu-id="a608b-267">g.</span></span> <span data-ttu-id="a608b-268">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a608b-268">Click **Save**.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a608b-269">Elküldheti regisztrációs mail felhasználói kiválasztásával **regisztrációs üzenet küldése** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="a608b-269">You can send registration mail to user by selecting **Send Registration Mail** checkbox.</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="a608b-270">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a608b-270">Assigning the Azure AD test user</span></span>

<span data-ttu-id="a608b-271">Ebben a szakaszban engedélyezze Britta Simon Azure egyszeri bejelentkezéshez használandó saját iLMS való hozzáférés biztosítása.</span><span class="sxs-lookup"><span data-stu-id="a608b-271">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to iLMS.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="a608b-273">**Britta Simon hozzárendelése iLMS, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="a608b-273">**To assign Britta Simon to iLMS, perform the following steps:**</span></span>

1. <span data-ttu-id="a608b-274">Az Azure-portálon, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a608b-274">In the Azure portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a608b-276">Az alkalmazások listában válassza ki a **iLMS**.</span><span class="sxs-lookup"><span data-stu-id="a608b-276">In the applications list, select **iLMS**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-ilms-tutorial/tutorial_ilms_app.png) 

3. <span data-ttu-id="a608b-278">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a608b-278">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="a608b-280">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a608b-280">Click **Add** button.</span></span> <span data-ttu-id="a608b-281">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a608b-281">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="a608b-283">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a608b-283">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="a608b-284">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a608b-284">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a608b-285">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a608b-285">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a608b-286">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a608b-286">Testing single sign-on</span></span>

<span data-ttu-id="a608b-287">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="a608b-287">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="a608b-288">Ha a hozzáférési panelen iLMS csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az iLMS alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="a608b-288">When you click the iLMS tile in the Access Panel, you should get automatically signed-on to your iLMS application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a608b-289">További források</span><span class="sxs-lookup"><span data-stu-id="a608b-289">Additional resources</span></span>

* [<span data-ttu-id="a608b-290">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="a608b-290">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a608b-291">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a608b-291">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-ilms-tutorial/tutorial_general_203.png

