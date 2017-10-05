---
title: "Oktatóanyag: Azure Active Directoryval integrált ServiceChannel |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja az egyszeri bejelentkezés Azure Active Directory és ServiceChannel között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: c3546eab-96b5-489b-a309-b895eb428053
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/3/2017
ms.author: jeedes
ms.openlocfilehash: 7e1dad18ff0ae9a9102b789b2cb32e7b96ed3d38
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="b5a01-103">Oktatóanyag: Azure Active Directoryval integrált ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="b5a01-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="b5a01-104">Ebben az oktatóanyagban elsajátíthatja ServiceChannel integrálása az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="b5a01-104">In this tutorial, you learn how to integrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="b5a01-105">ServiceChannel integrálása az Azure AD lehetővé teszi a következő előnyöket biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b5a01-105">Integrating ServiceChannel with Azure AD provides you with the following benefits:</span></span>

- <span data-ttu-id="b5a01-106">Megadhatja a ServiceChannel hozzáféréssel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b5a01-106">You can control in Azure AD who has access to ServiceChannel</span></span>
- <span data-ttu-id="b5a01-107">Engedélyezheti a felhasználóknak, hogy automatikusan beolvasása bejelentkezett ServiceChannel (egyszeri bejelentkezés) számára a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="b5a01-107">You can enable your users to automatically get signed-on to ServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="b5a01-108">Kezelheti a fiókokat, egy központi helyen – az Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="b5a01-108">You can manage your accounts in one central location - the Azure Management portal</span></span>

<span data-ttu-id="b5a01-109">Ha meg szeretné ismerni az Azure AD SaaS integrálásáról további adatait, tekintse meg [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="b5a01-109">If you want to know more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b5a01-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b5a01-110">Prerequisites</span></span>

<span data-ttu-id="b5a01-111">Az Azure AD rendszerrel történő integráció konfigurálása ServiceChannel, szüksége van a következőkre:</span><span class="sxs-lookup"><span data-stu-id="b5a01-111">To configure Azure AD integration with ServiceChannel, you need the following items:</span></span>

- <span data-ttu-id="b5a01-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="b5a01-112">An Azure AD subscription</span></span>
- <span data-ttu-id="b5a01-113">Egy ServiceChannel egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="b5a01-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="b5a01-114">Ez az oktatóanyag lépéseit teszteléséhez nem ajánlott használata termelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="b5a01-114">To test the steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="b5a01-115">Ebben az oktatóanyagban a lépéseket teszteléséhez kövesse ezeket a javaslatokat:</span><span class="sxs-lookup"><span data-stu-id="b5a01-115">To test the steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="b5a01-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="b5a01-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="b5a01-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b5a01-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="b5a01-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="b5a01-118">Scenario description</span></span>
<span data-ttu-id="b5a01-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="b5a01-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="b5a01-120">Ebben az oktatóanyagban leírt forgatókönyv két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="b5a01-120">The scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="b5a01-121">ServiceChannel hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b5a01-121">Adding ServiceChannel from the gallery</span></span>
2. <span data-ttu-id="b5a01-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b5a01-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-the-gallery"></a><span data-ttu-id="b5a01-123">ServiceChannel hozzáadása a gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="b5a01-123">Adding ServiceChannel from the gallery</span></span>
<span data-ttu-id="b5a01-124">Az Azure AD integrálása a ServiceChannel konfigurálásához kell hozzáadnia ServiceChannel a gyűjteményből a felügyelt SaaS-alkalmazások listájára.</span><span class="sxs-lookup"><span data-stu-id="b5a01-124">To configure the integration of ServiceChannel into Azure AD, you need to add ServiceChannel from the gallery to your list of managed SaaS apps.</span></span>

<span data-ttu-id="b5a01-125">**ServiceChannel hozzáadása a gyűjteményből, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b5a01-125">**To add ServiceChannel from the gallery, perform the following steps:**</span></span>

1. <span data-ttu-id="b5a01-126">Az a  **[Azure felügyeleti portálon](https://portal.azure.com)**, kattintson a bal oldali navigációs panelen **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b5a01-126">In the **[Azure Management Portal](https://portal.azure.com)**, on the left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="b5a01-128">Navigáljon a **vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-128">Navigate to **Enterprise applications**.</span></span> <span data-ttu-id="b5a01-129">Ezután lépjen **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-129">Then go to **All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="b5a01-131">Kattintson a **Hozzáadás** gombra a párbeszédpanel tetején.</span><span class="sxs-lookup"><span data-stu-id="b5a01-131">Click **Add** button on the top of the dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="b5a01-133">Írja be a keresőmezőbe, **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-133">In the search box, type **ServiceChannel**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="b5a01-135">Az eredmények panelen válassza ki a **ServiceChannel**, és kattintson a **Hozzáadás** gombra kattintva vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b5a01-135">In the results panel, select **ServiceChannel**, and then click **Add** button to add the application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="b5a01-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="b5a01-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="b5a01-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="b5a01-139">Az egyszeri bejelentkezés működéséhez az Azure AD meg kell tudja, hogy mi a párjukhoz felhasználó ServiceChannel a felhasználó Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="b5a01-139">For single sign-on to work, Azure AD needs to know what the counterpart user in ServiceChannel is to a user in Azure AD.</span></span> <span data-ttu-id="b5a01-140">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználó a ServiceChannel közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="b5a01-140">In other words, a link relationship between an Azure AD user and the related user in ServiceChannel needs to be established.</span></span>

<span data-ttu-id="b5a01-141">Ez a hivatkozás kapcsolat létesíti értéket rendeli az **felhasználónév** értékeként Azure AD-ben a **felhasználónév** a ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-141">This link relationship is established by assigning the value of the **user name** in Azure AD as the value of the **Username** in ServiceChannel.</span></span>

<span data-ttu-id="b5a01-142">Az Azure AD egyszeri bejelentkezést a ServiceChannel tesztelése és konfigurálása, hogy végezze el a következő építőelemeket kell:</span><span class="sxs-lookup"><span data-stu-id="b5a01-142">To configure and test Azure AD single sign-on with ServiceChannel, you need to complete the following building blocks:</span></span>

1. <span data-ttu-id="b5a01-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  – lehetővé teszi a felhasználók a szolgáltatás használatához.</span><span class="sxs-lookup"><span data-stu-id="b5a01-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - to enable your users to use this feature.</span></span>
2. <span data-ttu-id="b5a01-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b5a01-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="b5a01-145">**[ServiceChannel tesztfelhasználó létrehozása](#creating-a-servicechannel-test-user)**  – az Azure AD egyszeri bejelentkezést a Britta Simon teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="b5a01-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - to test Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="b5a01-146">**[Az Azure AD-teszt felhasználó hozzárendelése](#assigning-the-azure-ad-test-user)**  - Britta Simon használata az Azure AD az egyszeri bejelentkezés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b5a01-146">**[Assigning the Azure AD test user](#assigning-the-azure-ad-test-user)** - to enable Britta Simon to use Azure AD single sign-on.</span></span>
5. <span data-ttu-id="b5a01-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  – győződjön meg arról, hogy működik-e a konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="b5a01-147">**[Testing Single Sign-On](#testing-single-sign-on)** - to verify whether the configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="b5a01-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b5a01-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="b5a01-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezés engedélyezése az Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az ServiceChannel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="b5a01-149">In this section, you enable Azure AD single sign-on in the Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="b5a01-150">**Az Azure AD az egyszeri bejelentkezés konfigurálása ServiceChannel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b5a01-150">**To configure Azure AD single sign-on with ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="b5a01-151">Az Azure felügyeleti portálján a a **ServiceChannel** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-151">In the Azure Management portal, on the **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="b5a01-153">A a **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** a engedélyezése az egyszeri bejelentkezéshez.</span><span class="sxs-lookup"><span data-stu-id="b5a01-153">On the **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** to enable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="b5a01-155">Az a **ServiceChannel tartomány és az URL-címek** területen tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b5a01-155">On the **ServiceChannel Domain and URLs** section, perform the following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="b5a01-157">a.</span><span class="sxs-lookup"><span data-stu-id="b5a01-157">a.</span></span> <span data-ttu-id="b5a01-158">Az a **azonosító** szövegmező, írja be az értéket, mint:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="b5a01-158">In the **Identifier** textbox, type the value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="b5a01-159">b.</span><span class="sxs-lookup"><span data-stu-id="b5a01-159">b.</span></span> <span data-ttu-id="b5a01-160">Az a **válasz URL-CÍMEN** szövegmező, adja meg a következő minta használatával URL-címe:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="b5a01-160">In the **Reply URL** textbox, type a URL using the following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="b5a01-161">Ne feledje, hogy ezek nincsenek a valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="b5a01-161">Please note that these are not the real values.</span></span> <span data-ttu-id="b5a01-162">Akkor kell frissíteni ezeket az értékeket a tényleges azonosítóját és válasz URL-CÍMEN.</span><span class="sxs-lookup"><span data-stu-id="b5a01-162">You have to update these values with the actual Identifier and Reply URL.</span></span> <span data-ttu-id="b5a01-163">Itt javasoljuk, hogy az azonosító a karakterlánc egyedi értéket használja.</span><span class="sxs-lookup"><span data-stu-id="b5a01-163">Here we suggest you to use the unique value of string in the Identifier.</span></span> <span data-ttu-id="b5a01-164">Ügyfél [ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us) beolvasni ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="b5a01-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) to get these values.</span></span>

4. <span data-ttu-id="b5a01-165">ServiceChannel alkalmazás a SAML helyességi feltételek egy meghatározott formátumban, amelyek megkövetelik olyan egyéni attribútum-leképezésekhez hozzáadása a SAML-jogkivonat attribútumok konfigurációs vár.</span><span class="sxs-lookup"><span data-stu-id="b5a01-165">Your ServiceChannel application expects the SAML assertions in a specific format, which requires you to add custom attribute mappings to your SAML token attributes configuration.</span></span> <span data-ttu-id="b5a01-166">Az alábbi képernyőfelvételen látható egy példa a.</span><span class="sxs-lookup"><span data-stu-id="b5a01-166">The following screenshot shows an example for this.</span></span> <span data-ttu-id="b5a01-167">**NameIdentifier (felhasználói azonosító)** csak kötelező jogcímet, és az alapértelmezett érték **user.userprincipalname** ServiceChannel ez le kell képezni a vár, de **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-167">**NameIdentifier(User Identifier)** is the only mandatory claim and the default value is **user.userprincipalname** but ServiceChannel expects this to be mapped with **user.mail**.</span></span> <span data-ttu-id="b5a01-168">Ha azt tervezi, hogy engedélyezze a csak az idő a felhasználók átadása, majd adja hozzá a következő jogcímeket alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="b5a01-168">If you are planning to enable Just In Time user provisioning, then you should add the following claims as shown below.</span></span> <span data-ttu-id="b5a01-169">**Szerepkör** jogcím meg kell feleltetni a **user.assignedroles** , amely tartalmazza a felhasználói szerepkört.</span><span class="sxs-lookup"><span data-stu-id="b5a01-169">**Role** claim needs to be mapped to **user.assignedroles** which contains the role of the user.</span></span>  

    <span data-ttu-id="b5a01-170">Olvassa el a ServiceChannel útmutató [Itt](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) a jogcímek további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="b5a01-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="b5a01-172">Kattintson az [Itt](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tudnia kell, hogyan konfigurálhatja **szerepkör** Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="b5a01-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) to know how to configure **Role** in Azure AD</span></span>

5. <span data-ttu-id="b5a01-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="b5a01-173">In **User Attributes** section, click **View and edit all other user attributes** and set the attributes.</span></span>

    | <span data-ttu-id="b5a01-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="b5a01-174">Attribute Name</span></span> | <span data-ttu-id="b5a01-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="b5a01-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="b5a01-176">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="b5a01-176">Role</span></span>| <span data-ttu-id="b5a01-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="b5a01-177">user.assignedroles</span></span> |

    <span data-ttu-id="b5a01-178">a.</span><span class="sxs-lookup"><span data-stu-id="b5a01-178">a.</span></span> <span data-ttu-id="b5a01-179">Kattintson a **Hozzáadás attribútum** megnyitásához a **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-179">Click **Add attribute** to open the **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="b5a01-182">b.</span><span class="sxs-lookup"><span data-stu-id="b5a01-182">b.</span></span> <span data-ttu-id="b5a01-183">Az a **neve** szövegmező, írja be az adott sorhoz feltüntetett attribútumot nevét.</span><span class="sxs-lookup"><span data-stu-id="b5a01-183">In the **Name** textbox, type the attribute name shown for that row.</span></span>
    
    <span data-ttu-id="b5a01-184">c.</span><span class="sxs-lookup"><span data-stu-id="b5a01-184">c.</span></span> <span data-ttu-id="b5a01-185">Az a **érték** kilistázásához írja be a sorhoz látható attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="b5a01-185">From the **Value** list, type the attribute value shown for that row.</span></span>
    
    <span data-ttu-id="b5a01-186">d.</span><span class="sxs-lookup"><span data-stu-id="b5a01-186">d.</span></span> <span data-ttu-id="b5a01-187">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="b5a01-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="b5a01-188">A a **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a tanúsítványfájlt, a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b5a01-188">On the **SAML Signing Certificate** section, click **Certificate (Base64)** and then save the certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="b5a01-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="b5a01-190">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="b5a01-192">A a **ServiceChannel konfigurációs** kattintson **konfigurálása ServiceChannel** megnyitásához **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="b5a01-192">On the **ServiceChannel Configuration** section, click **Configure ServiceChannel** to open **Configure sign-on** window.</span></span> <span data-ttu-id="b5a01-193">Vegye figyelembe a **SAML Enitity azonosító** a a **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="b5a01-193">Please note the **SAML Enitity ID** from the **Quick Reference** section.</span></span>

9. <span data-ttu-id="b5a01-194">Egyszeri bejelentkezés konfigurálása **ServiceChannel** oldalon kell küldeniük a letöltött **tanúsítvány (Base64)** és **SAML Entitásazonosító** való [ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="b5a01-194">To configure single sign-on on **ServiceChannel** side, you need to send the downloaded **certificate (Base64)** and **SAML Entity ID** to [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="b5a01-195">Akkor lesz beállítására kell biztosítani a SAML SSO-kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="b5a01-195">They will set this up in order to have the SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="b5a01-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5a01-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="b5a01-197">Ez a szakasz célja a tesztfelhasználó létrehozása az Azure felügyeleti portálján Britta Simon nevezik.</span><span class="sxs-lookup"><span data-stu-id="b5a01-197">The objective of this section is to create a test user in the Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="b5a01-199">**Tesztfelhasználó létrehozása az Azure AD-ban, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b5a01-199">**To create a test user in Azure AD, perform the following steps:**</span></span>

1. <span data-ttu-id="b5a01-200">Az a **Azure Management portal**, a bal oldali navigációs ablaktábláján kattintson **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="b5a01-200">In the **Azure Management portal**, on the left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="b5a01-202">Ugrás a **felhasználók és csoportok** kattintson **minden felhasználó** azon felhasználók listájának megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b5a01-202">Go to **Users and groups** and click **All users** to display the list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="b5a01-204">Kattintson a párbeszédpanel tetején **Hozzáadás** megnyitásához a **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-204">At the top of the dialog click **Add** to open the **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="b5a01-206">Az a **felhasználói** párbeszédpanel lapon, a következő lépésekkel:</span><span class="sxs-lookup"><span data-stu-id="b5a01-206">On the **User** dialog page, perform the following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="b5a01-208">a.</span><span class="sxs-lookup"><span data-stu-id="b5a01-208">a.</span></span> <span data-ttu-id="b5a01-209">Az a **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-209">In the **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="b5a01-210">b.</span><span class="sxs-lookup"><span data-stu-id="b5a01-210">b.</span></span> <span data-ttu-id="b5a01-211">Az a **felhasználónév** szövegmezőhöz típusa a **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="b5a01-211">In the **User name** textbox, type the **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="b5a01-212">c.</span><span class="sxs-lookup"><span data-stu-id="b5a01-212">c.</span></span> <span data-ttu-id="b5a01-213">Válassza ki **megjelenítése jelszó** írja le a értékének a **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-213">Select **Show Password** and write down the value of the **Password**.</span></span>

    <span data-ttu-id="b5a01-214">d.</span><span class="sxs-lookup"><span data-stu-id="b5a01-214">d.</span></span> <span data-ttu-id="b5a01-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="b5a01-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="b5a01-216">ServiceChannel tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="b5a01-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="b5a01-217">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére az alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="b5a01-217">Application supports Just in time user provisioning and after authentication users will be created in the application automatically.</span></span> <span data-ttu-id="b5a01-218">Teljes felhasználói történő üzembe helyezéséhez, lépjen kapcsolatba [ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="b5a01-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-the-azure-ad-test-user"></a><span data-ttu-id="b5a01-219">Az Azure AD-teszt felhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="b5a01-219">Assigning the Azure AD test user</span></span>

<span data-ttu-id="b5a01-220">Ebben a szakaszban engedélyezze Britta Simon által biztosított a hozzáférés ServiceChannel Azure egyszeri bejelentkezéshez használandó.</span><span class="sxs-lookup"><span data-stu-id="b5a01-220">In this section, you enable Britta Simon to use Azure single sign-on by granting her access to ServiceChannel.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="b5a01-222">**ServiceChannel Britta Simon rendel, hajtsa végre az alábbi lépéseket:**</span><span class="sxs-lookup"><span data-stu-id="b5a01-222">**To assign Britta Simon to ServiceChannel, perform the following steps:**</span></span>

1. <span data-ttu-id="b5a01-223">Az Azure felügyeleti portálra, nyissa meg az alkalmazások nézet, majd nyissa meg a könyvtár nézetet, és navigáljon **vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-223">In the Azure Management portal, open the applications view, and then navigate to the directory view and go to **Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="b5a01-225">Az alkalmazások listában válassza ki a **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-225">In the applications list, select **ServiceChannel**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="b5a01-227">A bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="b5a01-227">In the menu on the left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="b5a01-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="b5a01-229">Click **Add** button.</span></span> <span data-ttu-id="b5a01-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="b5a01-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** a felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="b5a01-232">On **Users and groups** dialog, select **Britta Simon** in the Users list.</span></span>

6. <span data-ttu-id="b5a01-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="b5a01-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="b5a01-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="b5a01-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="b5a01-235">Testing single sign-on</span></span>

<span data-ttu-id="b5a01-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai a hozzáférési panelen tesztelése.</span><span class="sxs-lookup"><span data-stu-id="b5a01-236">In this section, you test your Azure AD single sign-on configuration using the Access Panel.</span></span>

<span data-ttu-id="b5a01-237">Ha a hozzáférési panelen ServiceChannel csempére kattint, akkor kell beolvasása automatikusan bejelentkezett az ServiceChannel alkalmazására.</span><span class="sxs-lookup"><span data-stu-id="b5a01-237">When you click the ServiceChannel tile in the Access Panel, you should get automatically signed-on to your ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5a01-238">További források</span><span class="sxs-lookup"><span data-stu-id="b5a01-238">Additional resources</span></span>

* [<span data-ttu-id="b5a01-239">Az Azure Active Directoryval SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="b5a01-239">List of Tutorials on How to Integrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="b5a01-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="b5a01-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-servicechannel-tutorial/tutorial_general_203.png