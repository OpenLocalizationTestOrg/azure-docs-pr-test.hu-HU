---
title: "Oktatóanyag: Azure Active Directoryval integrált ServiceChannel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és ServiceChannel között."
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
ms.openlocfilehash: 956371a1e99dcba4137c271ecfe8a62b9ec64a99
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-servicechannel"></a><span data-ttu-id="340cf-103">Oktatóanyag: Azure Active Directoryval integrált ServiceChannel</span><span class="sxs-lookup"><span data-stu-id="340cf-103">Tutorial: Azure Active Directory integration with ServiceChannel</span></span>

<span data-ttu-id="340cf-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate ServiceChannel az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="340cf-104">In this tutorial, you learn how toointegrate ServiceChannel with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="340cf-105">ServiceChannel integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="340cf-105">Integrating ServiceChannel with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="340cf-106">Megadhatja a hozzáférés tooServiceChannel rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="340cf-106">You can control in Azure AD who has access tooServiceChannel</span></span>
- <span data-ttu-id="340cf-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooServiceChannel (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="340cf-107">You can enable your users tooautomatically get signed-on tooServiceChannel (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="340cf-108">Kezelheti a fiókokat, egy központi helyen - hello Azure felügyeleti portálon</span><span class="sxs-lookup"><span data-stu-id="340cf-108">You can manage your accounts in one central location - hello Azure Management portal</span></span>

<span data-ttu-id="340cf-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="340cf-109">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="340cf-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="340cf-110">Prerequisites</span></span>

<span data-ttu-id="340cf-111">az Azure AD integrálása ServiceChannel tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="340cf-111">tooconfigure Azure AD integration with ServiceChannel, you need hello following items:</span></span>

- <span data-ttu-id="340cf-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="340cf-112">An Azure AD subscription</span></span>
- <span data-ttu-id="340cf-113">Egy ServiceChannel egyszeri bejelentkezés engedélyezve van az előfizetésben</span><span class="sxs-lookup"><span data-stu-id="340cf-113">A ServiceChannel single-sign on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="340cf-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="340cf-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="340cf-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="340cf-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="340cf-116">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="340cf-116">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="340cf-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy egy hónapos próbaverzió kaphat [Itt](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="340cf-117">If you don't have an Azure AD trial environment, you can get an one-month trial [here](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="340cf-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="340cf-118">Scenario description</span></span>
<span data-ttu-id="340cf-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="340cf-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="340cf-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="340cf-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="340cf-121">ServiceChannel hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="340cf-121">Adding ServiceChannel from hello gallery</span></span>
2. <span data-ttu-id="340cf-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="340cf-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-servicechannel-from-hello-gallery"></a><span data-ttu-id="340cf-123">ServiceChannel hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="340cf-123">Adding ServiceChannel from hello gallery</span></span>
<span data-ttu-id="340cf-124">tooconfigure hello integrációja ServiceChannel az Azure AD-be, meg kell tooadd ServiceChannel hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="340cf-124">tooconfigure hello integration of ServiceChannel into Azure AD, you need tooadd ServiceChannel from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="340cf-125">**tooadd ServiceChannel hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="340cf-125">**tooadd ServiceChannel from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="340cf-126">A hello  **[Azure felügyeleti portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="340cf-126">In hello **[Azure Management Portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="340cf-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="340cf-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="340cf-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="340cf-129">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="340cf-131">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="340cf-131">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="340cf-133">Hello keresési mezőbe, írja be a **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="340cf-133">In hello search box, type **ServiceChannel**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_000.png)

5. <span data-ttu-id="340cf-135">A hello eredmények panelen válassza ki a **ServiceChannel**, és kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="340cf-135">In hello results panel, select **ServiceChannel**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_2.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="340cf-137">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="340cf-137">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="340cf-138">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD egyszeri bejelentkezéshez "Britta Simon" nevű tesztfelhasználó alapján ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="340cf-138">In this section, you configure and test Azure AD single sign-on with ServiceChannel based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="340cf-139">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó ServiceChannel tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="340cf-139">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in ServiceChannel is tooa user in Azure AD.</span></span> <span data-ttu-id="340cf-140">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello ServiceChannel közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="340cf-140">In other words, a link relationship between an Azure AD user and hello related user in ServiceChannel needs toobe established.</span></span>

<span data-ttu-id="340cf-141">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** a ServiceChannel.</span><span class="sxs-lookup"><span data-stu-id="340cf-141">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in ServiceChannel.</span></span>

<span data-ttu-id="340cf-142">tooconfigure és az Azure AD az egyszeri bejelentkezés ServiceChannel-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="340cf-142">tooconfigure and test Azure AD single sign-on with ServiceChannel, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="340cf-143">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="340cf-143">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="340cf-144">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="340cf-144">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="340cf-145">**[ServiceChannel tesztfelhasználó létrehozása](#creating-a-servicechannel-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="340cf-145">**[Creating a ServiceChannel test user](#creating-a-servicechannel-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
4. <span data-ttu-id="340cf-146">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="340cf-146">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="340cf-147">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="340cf-147">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="340cf-148">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="340cf-148">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="340cf-149">Ebben a szakaszban az Azure AD egyszeri bejelentkezéssel hello Azure felügyeleti portálon, és konfigurálása egyszeri bejelentkezéshez az ServiceChannel alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="340cf-149">In this section, you enable Azure AD single sign-on in hello Azure Management portal and configure single sign-on in your ServiceChannel application.</span></span>

<span data-ttu-id="340cf-150">**az Azure AD tooconfigure egyszeri bejelentkezést a ServiceChannel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="340cf-150">**tooconfigure Azure AD single sign-on with ServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="340cf-151">Hello Azure felügyeleti portálon, a hello **ServiceChannel** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="340cf-151">In hello Azure Management portal, on hello **ServiceChannel** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="340cf-153">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="340cf-153">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_01.png)

3. <span data-ttu-id="340cf-155">A hello **ServiceChannel tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="340cf-155">On hello **ServiceChannel Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_urls.png)

    <span data-ttu-id="340cf-157">a.</span><span class="sxs-lookup"><span data-stu-id="340cf-157">a.</span></span> <span data-ttu-id="340cf-158">A hello **azonosító** szövegmezőhöz hello érték típusa:`http://adfs.<domain>.com/adfs/service/trust`</span><span class="sxs-lookup"><span data-stu-id="340cf-158">In hello **Identifier** textbox, type hello value as: `http://adfs.<domain>.com/adfs/service/trust`</span></span>

    <span data-ttu-id="340cf-159">b.</span><span class="sxs-lookup"><span data-stu-id="340cf-159">b.</span></span> <span data-ttu-id="340cf-160">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<customer domain>.servicechannel.com/saml/acs`</span><span class="sxs-lookup"><span data-stu-id="340cf-160">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<customer domain>.servicechannel.com/saml/acs`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="340cf-161">Ne feledje, hogy ezek nincsenek hello valódi értékek.</span><span class="sxs-lookup"><span data-stu-id="340cf-161">Please note that these are not hello real values.</span></span> <span data-ttu-id="340cf-162">Tooupdate rendelkezik ezekkel az értékekkel rendelkező hello tényleges azonosítója és válasz URL-címet.</span><span class="sxs-lookup"><span data-stu-id="340cf-162">You have tooupdate these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="340cf-163">Itt javasoljuk, hogy Ön toouse hello egyedi karakterlánc értéke a hello azonosítója.</span><span class="sxs-lookup"><span data-stu-id="340cf-163">Here we suggest you toouse hello unique value of string in hello Identifier.</span></span> <span data-ttu-id="340cf-164">Ügyfél [ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="340cf-164">Contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us) tooget these values.</span></span>

4. <span data-ttu-id="340cf-165">A ServiceChannel alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban, és hogy tooadd egyéni attribútum hozzárendelések tooyour SAML token attribútumok konfigurációt igényel.</span><span class="sxs-lookup"><span data-stu-id="340cf-165">Your ServiceChannel application expects hello SAML assertions in a specific format, which requires you tooadd custom attribute mappings tooyour SAML token attributes configuration.</span></span> <span data-ttu-id="340cf-166">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="340cf-166">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="340cf-167">**NameIdentifier (felhasználói azonosító)** hello csak akkor kötelező, ha a jogcímet, és alapértelmezett értéke hello **user.userprincipalname** ServiceChannel vár a toobe leképezve, de **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="340cf-167">**NameIdentifier(User Identifier)** is hello only mandatory claim and hello default value is **user.userprincipalname** but ServiceChannel expects this toobe mapped with **user.mail**.</span></span> <span data-ttu-id="340cf-168">Ha azt tervezi, tooenable idő csak a felhasználók átadása, majd adja hozzá a következő jogcímeket alább látható módon hello.</span><span class="sxs-lookup"><span data-stu-id="340cf-168">If you are planning tooenable Just In Time user provisioning, then you should add hello following claims as shown below.</span></span> <span data-ttu-id="340cf-169">**Szerepkör** jogcím kell leképezése túl toobe**user.assignedroles** hello felhasználó hello szerepkör, amely tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="340cf-169">**Role** claim needs toobe mapped too**user.assignedroles** which contains hello role of hello user.</span></span>  

    <span data-ttu-id="340cf-170">Olvassa el a ServiceChannel útmutató [Itt](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) a jogcímek további útmutatást.</span><span class="sxs-lookup"><span data-stu-id="340cf-170">You can refer ServiceChannel guide [here](https://servicechannel.zendesk.com/hc/en-us/articles/217514326-Azure-AD-Configuration-Example) for more guidance on claims.</span></span>
    
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_attribute.png)

    > [!NOTE] 
    > <span data-ttu-id="340cf-172">Kattintson az [Itt](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow hogyan tooconfigure **szerepkör** Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="340cf-172">Please click [here](http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/) tooknow how tooconfigure **Role** in Azure AD</span></span>

5. <span data-ttu-id="340cf-173">A **felhasználói attribútumok** kattintson **nézet és egyéb felhasználói attribútumok szerkesztése** és hello attribútumainak beállítása.</span><span class="sxs-lookup"><span data-stu-id="340cf-173">In **User Attributes** section, click **View and edit all other user attributes** and set hello attributes.</span></span>

    | <span data-ttu-id="340cf-174">Attribútum neve</span><span class="sxs-lookup"><span data-stu-id="340cf-174">Attribute Name</span></span> | <span data-ttu-id="340cf-175">Attribútum-érték</span><span class="sxs-lookup"><span data-stu-id="340cf-175">Attribute Value</span></span> |
    | --- | --- |    
    | <span data-ttu-id="340cf-176">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="340cf-176">Role</span></span>| <span data-ttu-id="340cf-177">User.assignedroles</span><span class="sxs-lookup"><span data-stu-id="340cf-177">user.assignedroles</span></span> |

    <span data-ttu-id="340cf-178">a.</span><span class="sxs-lookup"><span data-stu-id="340cf-178">a.</span></span> <span data-ttu-id="340cf-179">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="340cf-179">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_04.png)

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_servicechannel_05.png)
    
    <span data-ttu-id="340cf-182">b.</span><span class="sxs-lookup"><span data-stu-id="340cf-182">b.</span></span> <span data-ttu-id="340cf-183">A hello **neve** szövegmezőben, az adott sorhoz feltüntetett hello attribútum neve.</span><span class="sxs-lookup"><span data-stu-id="340cf-183">In hello **Name** textbox, type hello attribute name shown for that row.</span></span>
    
    <span data-ttu-id="340cf-184">c.</span><span class="sxs-lookup"><span data-stu-id="340cf-184">c.</span></span> <span data-ttu-id="340cf-185">A hello **érték** listájában, hello attribútuma Típusérték az adott sorhoz feltüntetett.</span><span class="sxs-lookup"><span data-stu-id="340cf-185">From hello **Value** list, type hello attribute value shown for that row.</span></span>
    
    <span data-ttu-id="340cf-186">d.</span><span class="sxs-lookup"><span data-stu-id="340cf-186">d.</span></span> <span data-ttu-id="340cf-187">Kattintson a **Ok**</span><span class="sxs-lookup"><span data-stu-id="340cf-187">Click **Ok**</span></span>
    
6. <span data-ttu-id="340cf-188">A hello **SAML-aláíró tanúsítványa** kattintson **tanúsítvány (Base64)** , és mentse a hello tanúsítványfájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="340cf-188">On hello **SAML Signing Certificate** section, click **Certificate (Base64)** and then save hello certificate file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_05.png) 

7. <span data-ttu-id="340cf-190">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="340cf-190">Click **Save**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial_general_400.png)

8. <span data-ttu-id="340cf-192">A hello **ServiceChannel konfigurációs** kattintson **konfigurálása ServiceChannel** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="340cf-192">On hello **ServiceChannel Configuration** section, click **Configure ServiceChannel** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="340cf-193">Ne feledje hello **SAML Enitity azonosító** a hello **rövid összefoglaló** szakasz.</span><span class="sxs-lookup"><span data-stu-id="340cf-193">Please note hello **SAML Enitity ID** from hello **Quick Reference** section.</span></span>

9. <span data-ttu-id="340cf-194">tooconfigure egyszeri bejelentkezést a **ServiceChannel** oldalon kell letöltött toosend hello **tanúsítvány (Base64)** és **SAML Entitásazonosító** túl[ ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us).</span><span class="sxs-lookup"><span data-stu-id="340cf-194">tooconfigure single sign-on on **ServiceChannel** side, you need toosend hello downloaded **certificate (Base64)** and **SAML Entity ID** too[ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us).</span></span> <span data-ttu-id="340cf-195">Azok fog beállítása ennek rendelés toohave hello SAML SSO kapcsolat mindkét oldalán megfelelően beállítva.</span><span class="sxs-lookup"><span data-stu-id="340cf-195">They will set this up in order toohave hello SAML SSO connection set properly on both sides.</span></span>

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="340cf-196">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="340cf-196">Creating an Azure AD test user</span></span>
<span data-ttu-id="340cf-197">hello ebben a szakaszban célja toocreate tesztfelhasználó Britta Simon nevű hello Azure felügyeleti portálon.</span><span class="sxs-lookup"><span data-stu-id="340cf-197">hello objective of this section is toocreate a test user in hello Azure Management portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="340cf-199">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="340cf-199">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="340cf-200">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="340cf-200">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="340cf-202">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="340cf-202">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="340cf-204">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="340cf-204">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="340cf-206">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="340cf-206">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-servicechannel-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="340cf-208">a.</span><span class="sxs-lookup"><span data-stu-id="340cf-208">a.</span></span> <span data-ttu-id="340cf-209">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="340cf-209">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="340cf-210">b.</span><span class="sxs-lookup"><span data-stu-id="340cf-210">b.</span></span> <span data-ttu-id="340cf-211">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="340cf-211">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="340cf-212">c.</span><span class="sxs-lookup"><span data-stu-id="340cf-212">c.</span></span> <span data-ttu-id="340cf-213">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="340cf-213">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="340cf-214">d.</span><span class="sxs-lookup"><span data-stu-id="340cf-214">d.</span></span> <span data-ttu-id="340cf-215">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="340cf-215">Click **Create**.</span></span> 

### <a name="creating-a-servicechannel-test-user"></a><span data-ttu-id="340cf-216">ServiceChannel tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="340cf-216">Creating a ServiceChannel test user</span></span>

<span data-ttu-id="340cf-217">Alkalmazás támogatja a csak az idő a felhasználók átadása, miután a felhasználók hitelesítésére hello alkalmazás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="340cf-217">Application supports Just in time user provisioning and after authentication users will be created in hello application automatically.</span></span> <span data-ttu-id="340cf-218">Teljes felhasználói történő üzembe helyezéséhez, lépjen kapcsolatba [ServiceChannel támogatási csoport](https://servicechannel.zendesk.com/hc/en-us)</span><span class="sxs-lookup"><span data-stu-id="340cf-218">For full user provisioning, please contact [ServiceChannel support team](https://servicechannel.zendesk.com/hc/en-us)</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="340cf-219">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="340cf-219">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="340cf-220">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooServiceChannel megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="340cf-220">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooServiceChannel.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="340cf-222">**tooassign Britta Simon tooServiceChannel, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="340cf-222">**tooassign Britta Simon tooServiceChannel, perform hello following steps:**</span></span>

1. <span data-ttu-id="340cf-223">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="340cf-223">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="340cf-225">Hello alkalmazások listában válassza ki a **ServiceChannel**.</span><span class="sxs-lookup"><span data-stu-id="340cf-225">In hello applications list, select **ServiceChannel**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-servicechannel-tutorial/tutorial-servicechannel_app01.png) 

3. <span data-ttu-id="340cf-227">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="340cf-227">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="340cf-229">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="340cf-229">Click **Add** button.</span></span> <span data-ttu-id="340cf-230">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="340cf-230">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="340cf-232">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="340cf-232">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="340cf-233">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="340cf-233">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="340cf-234">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="340cf-234">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="340cf-235">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="340cf-235">Testing single sign-on</span></span>

<span data-ttu-id="340cf-236">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="340cf-236">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="340cf-237">Ha a hozzáférési Panel hello hello ServiceChannel csempe gombra kattint, automatikusan bejelentkezett tooyour ServiceChannel alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="340cf-237">When you click hello ServiceChannel tile in hello Access Panel, you should get automatically signed-on tooyour ServiceChannel application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="340cf-238">További források</span><span class="sxs-lookup"><span data-stu-id="340cf-238">Additional resources</span></span>

* [<span data-ttu-id="340cf-239">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="340cf-239">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="340cf-240">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="340cf-240">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


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