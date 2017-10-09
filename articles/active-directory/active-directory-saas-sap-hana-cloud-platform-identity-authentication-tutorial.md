---
title: "Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory között, és SAP HANA felhőalapú Platform identitás hitelesítés."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.assetid: 1c1320d1-7ba4-4b5f-926f-4996b44d9b5e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: jeedes
ms.reviewer: jeedes
ms.openlocfilehash: 2142770779ddb745555b47fc85b5457b573f9506
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana-cloud-platform-identity-authentication"></a><span data-ttu-id="4c0f9-103">Oktatóanyag: Azure Active Directory-integráció SAP HANA felhő Platform identitás hitelesítéssel</span><span class="sxs-lookup"><span data-stu-id="4c0f9-103">Tutorial: Azure Active Directory integration with SAP HANA Cloud Platform Identity Authentication</span></span>

<span data-ttu-id="4c0f9-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP HANA felhőalapú Platform identitás hitelesítés az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-104">In this tutorial, you learn how toointegrate SAP HANA Cloud Platform Identity Authentication with Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="4c0f9-105">A proxy IdP tooaccess SAP alkalmazások az Azure AD használatával, mint a fő IdP hello SAP HANA felhőalapú Platform identitás hitelesítés lesz.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-105">SAP HANA Cloud Platform Identity Authentication is used as a proxy IdP tooaccess SAP applications using Azure AD as hello main IdP.</span></span>

<span data-ttu-id="4c0f9-106">SAP HANA felhőalapú Platform identitás hitelesítés integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-106">Integrating SAP HANA Cloud Platform Identity Authentication with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="4c0f9-107">Megadhatja a hozzáférés tooSAP alkalmazás rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="4c0f9-107">You can control in Azure AD who has access tooSAP application</span></span>
- <span data-ttu-id="4c0f9-108">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP alkalmazások egyszeri bejelentkezés (SSO) és az Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="4c0f9-108">You can enable your users tooautomatically get signed-on tooSAP applications single sign-on (SSO) with their Azure AD accounts</span></span>
- <span data-ttu-id="4c0f9-109">Kezelheti a fiókokat, egy központi helyen - hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="4c0f9-109">You can manage your accounts in one central location - hello Azure classic portal</span></span>

<span data-ttu-id="4c0f9-110">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-110">If you want tooknow more details about SaaS app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="4c0f9-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="4c0f9-111">Prerequisites</span></span>

<span data-ttu-id="4c0f9-112">tooconfigure SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD integrálása, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-112">tooconfigure Azure AD integration with SAP HANA Cloud Platform Identity Authentication, you need hello following items:</span></span>

- <span data-ttu-id="4c0f9-113">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="4c0f9-113">An Azure AD subscription</span></span>
- <span data-ttu-id="4c0f9-114">A **SAP HANA felhőalapú Platform identitás hitelesítés** SSO előfizetés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4c0f9-114">A **SAP HANA Cloud Platform Identity Authentication** SSO enabled subscription</span></span>


>[!NOTE] 
><span data-ttu-id="4c0f9-115">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-115">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>
>

<span data-ttu-id="4c0f9-116">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-116">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="4c0f9-117">Ne használja az éles környezetben, ha ez nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-117">You should not use your production environment, unless this is necessary.</span></span>
- <span data-ttu-id="4c0f9-118">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, beszerezheti a [egy hónapos próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-118">If you don't have an Azure AD trial environment, you can get a [one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="4c0f9-119">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="4c0f9-119">Scenario description</span></span>
<span data-ttu-id="4c0f9-120">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-120">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span>

<span data-ttu-id="4c0f9-121">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-121">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="4c0f9-122">SAP HANA felhőalapú Platform identitás hitelesítés hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c0f9-122">Adding SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
2. <span data-ttu-id="4c0f9-123">Azure AD egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0f9-123">Configuring and testing Azure AD SSO</span></span>

<span data-ttu-id="4c0f9-124">Előtt ról hello technikai részleteket, létfontosságú toounderstand hello fogalmak: toolook fog.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-124">Before diving into hello technical details, it is vital toounderstand hello concepts you're going toolook at.</span></span> <span data-ttu-id="4c0f9-125">hello SAP HANA felhőalapú Platform identitás hitelesítés és az Azure Active Directory összevonási lehetővé teszi több alkalmazáshoz vagy szolgáltatáshoz (mint egy IdP) AAD védi az SAP-alkalmazásokkal és szolgáltatásokkal SAP HANA felhő Platform identitás által védett SSO tooimplement Hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-125">hello SAP HANA Cloud Platform Identity Authentication and Azure Active Directory federation enables you tooimplement SSO across applications or services protected by AAD (as an IdP) with SAP applications and services protected by SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="4c0f9-126">Jelenleg SAP HANA felhőalapú Platform identitás hitelesítés úgy működik, mint a Proxy identitásszolgáltató tooSAP-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-126">Currently, SAP HANA Cloud Platform Identity Authentication acts as a Proxy Identity Provider tooSAP-applications.</span></span> <span data-ttu-id="4c0f9-127">Az Azure Active Directory pedig a telepítő az identitásszolgáltató vezető hello funkcionál.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-127">Azure Active Directory in turn acts as hello leading Identity Provider in this setup.</span></span> 

<span data-ttu-id="4c0f9-128">hello a következő ábra ezt mutatja be:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-128">hello following diagram illustrates this:</span></span>    

![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/architecture-01.png)

<span data-ttu-id="4c0f9-130">Ez a beállítás a megbízható alkalmazások az Azure Active Directoryban az SAP HANA felhőalapú Platform identitás hitelesítés bérlő lesz beállítva.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-130">With this setup, your SAP HANA Cloud Platform Identity Authentication tenant will be configured as a trusted application in Azure Active Directory.</span></span> 

<span data-ttu-id="4c0f9-131">SAP-alkalmazások és szolgáltatások azt szeretné, hogy így keresztül tooprotect ezt követően konfigurált hello SAP HANA felhőalapú Platform identitás hitelesítés felügyeleti konzol!</span><span class="sxs-lookup"><span data-stu-id="4c0f9-131">All SAP applications and services you want tooprotect through this way are subsequently configured in hello SAP HANA Cloud Platform Identity Authentication management console!</span></span>

<span data-ttu-id="4c0f9-132">Ez azt jelenti, hogy engedélyezési tooSAP alkalmazások elérésének engedélyezésére és szolgáltatási igények tootake hely az SAP HANA felhőalapú Platform identitás hitelesítés (az Azure Active Directoryban engedélyezési megakadályozását tooconfiguring) a telepítéshez.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-132">This means that authorization for granting access tooSAP applications and services needs tootake place in SAP HANA Cloud Platform Identity Authentication for such a setup (as opposed tooconfiguring authorization in Azure Active Directory).</span></span>

<span data-ttu-id="4c0f9-133">SAP HANA felhőalapú Platform identitás hitelesítés konfigurálásával keresztül hello Azure Active Directory Marketplace alkalmazásként, a felesleges tootake kiszolgálásához szükséges az egyes jogcímek konfigurálása / SAML helyességi feltételek és átalakítások szükséges tooproduce egy érvényes hitelesítési jogkivonat az SAP-alkalmazásokból.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-133">By configuring SAP HANA Cloud Platform Identity Authentication as an application through hello Azure Active Directory Marketplace, you don't need tootake care of configuring needed individual claims / SAML assertions and transformations needed tooproduce a valid authentication token for SAP applications.</span></span>

>[!NOTE] 
><span data-ttu-id="4c0f9-134">Webes egyszeri bejelentkezés jelenleg csak a két fél által tesztelték.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-134">Currently Web SSO has been tested by both parties, only.</span></span> <span data-ttu-id="4c0f9-135">Alkalmazás-API vagy API-API kommunikációhoz szükséges adatfolyamok kell működnie, de nem lettek tesztelve, még.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-135">Flows needed for App-to-API or API-to-API communication should work but have not been tested, yet.</span></span> <span data-ttu-id="4c0f9-136">Azok a soron következő tevékenységek részeként kell vizsgálni.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-136">They will be tested as part of subsequent activities.</span></span>
>

## <a name="add-sap-hana-cloud-platform-identity-authentication-from-hello-gallery"></a><span data-ttu-id="4c0f9-137">Adja hozzá az SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="4c0f9-137">Add SAP HANA Cloud Platform Identity Authentication from hello gallery</span></span>
<span data-ttu-id="4c0f9-138">tooconfigure hello integrációja SAP HANA felhőalapú Platform identitás hitelesítés az Azure AD-be, meg kell tooadd SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-138">tooconfigure hello integration of SAP HANA Cloud Platform Identity Authentication into Azure AD, you need tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="4c0f9-139">**tooadd SAP HANA felhőalapú Platform identitás hitelesítés hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c0f9-139">**tooadd SAP HANA Cloud Platform Identity Authentication from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c0f9-140">A hello [ **Azure Management portal**](https://portal.azure.com), a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-140">In hello [**Azure Management portal**](https://portal.azure.com), on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![Active Directory][1]

2. <span data-ttu-id="4c0f9-142">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-142">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="4c0f9-143">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-143">Then go too**All applications**.</span></span>

    ![Alkalmazások][2]
    
3. <span data-ttu-id="4c0f9-145">Kattintson a **Hozzáadás** hello párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-145">Click **Add** button on hello top of hello dialog.</span></span>

    ![Alkalmazások][3]

4. <span data-ttu-id="4c0f9-147">Hello keresési mezőbe, írja be a **SAP HANA felhőalapú Platform identitás hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-147">In hello search box, type **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_01.png)

5. <span data-ttu-id="4c0f9-149">A hello eredmények panelen válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**, és kattintson a **hozzáadása** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-149">In hello results panel, select **SAP HANA Cloud Platform Identity Authentication**, and then click **Add** button tooadd hello application.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_02.png)


##  <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="4c0f9-151">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0f9-151">Configure and test Azure AD single sign-on</span></span>
<span data-ttu-id="4c0f9-152">Ebben a szakaszban, tesztelése és konfigurálása Azure AD SSO SAP HANA felhőalapú Platform identitás hitelesítés "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-152">In this section, you configure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="4c0f9-153">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SAP HANA felhőalapú Platform identitás hitelesítés tooa felhasználó az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-153">For SSO toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA Cloud Platform Identity Authentication is tooa user in Azure AD.</span></span> <span data-ttu-id="4c0f9-154">Ez azt jelenti hello kapcsolódó felhasználó a SAP HANA felhőalapú Platform identitás hitelesítés és az Azure AD-felhasználó közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-154">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA Cloud Platform Identity Authentication needs toobe established.</span></span>

<span data-ttu-id="4c0f9-155">Ez a hivatkozás kapcsolat létesíti hello hello értékkel **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** az SAP HANA felhőalapú Platform identitás hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-155">This link relationship is established by assigning hello value of hello **user name** in Azure AD as hello value of hello **Username** in SAP HANA Cloud Platform Identity Authentication.</span></span>

<span data-ttu-id="4c0f9-156">tooconfigure és tesztelési Azure AD SSO SAP HANA felhő Platform identitás hitelesítéssel, a következő építőelemeket toocomplete hello kell:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-156">tooconfigure and test Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="4c0f9-157">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-157">**[Configuring Azure AD single sign-on](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="4c0f9-158">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-158">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="4c0f9-159">**[Egy SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)**  -toohave egy SAP HANA felhő Platform identitás hitelesítést, amely az Azure AD csatolt toohello ábrázolása rá, hogy Britta Simon megfelelője.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-159">**[Creating a SAP HANA Cloud Platform Identity Authentication test user](#creating-a-sap-hana-cloud-platform-identity-authentication-test-user)** - toohave a counterpart of Britta Simon in SAP HANA Cloud Platform Identity Authentication that is linked toohello Azure AD representation of her.</span></span>
4. <span data-ttu-id="4c0f9-160">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-160">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="4c0f9-161">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-161">**[Testing single sign-on](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-sso"></a><span data-ttu-id="4c0f9-162">Az Azure AD-egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="4c0f9-162">Configuring Azure AD SSO</span></span>

<span data-ttu-id="4c0f9-163">Ebben a szakaszban engedélyezze az Azure AD egyszeri Bejelentkezést hello Azure felügyeleti portálon, és az SAP HANA felhőalapú Platform identitás hitelesítés alkalmazásban egyszeri bejelentkezés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-163">In this section, you enable Azure AD SSO in hello Azure Management portal and configure single sign-on in your SAP HANA Cloud Platform Identity Authentication application.</span></span>

<span data-ttu-id="4c0f9-164">SAP HANA felhőalapú Platform identitás hitelesítés alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-164">SAP HANA Cloud Platform Identity Authentication application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="4c0f9-165">Ezek az attribútumok értékének hello hello kezelése "**felhasználói attribútumok**" szakasz alkalmazás integráció lapján.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-165">You can manage hello values of these attributes from hello "**User Attributes**" section on application integration page.</span></span> <span data-ttu-id="4c0f9-166">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-166">hello following screenshot shows an example for this.</span></span>

![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_03.png)

<span data-ttu-id="4c0f9-168">**tooconfigure Azure AD SSO SAP HANA felhő Platform identitás hitelesítéssel, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c0f9-168">**tooconfigure Azure AD SSO with SAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c0f9-169">Hello Azure felügyeleti portálon, a hello **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-169">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="4c0f9-171">A hello **egyszeri bejelentkezés** párbeszédpanel, mint **mód** kiválasztása **SAML-alapú bejelentkezés** tooenable az egyszeri bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-171">On hello **Single sign-on** dialog, as **Mode** select **SAML-based Sign-on** tooenable single sign on.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása][5]

3. <span data-ttu-id="4c0f9-173">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel, ha az SAP-alkalmazást például a "Keresztnév" attribútumot vár.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-173">In hello **User Attributes** section on hello **Single sign-on** dialog, if your SAP application expects an attribute for example "firstName".</span></span> <span data-ttu-id="4c0f9-174">Hello SAML-jogkivonat attribútumok párbeszédpanelen adja hozzá a hello "Keresztnév" attribútumot.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-174">On hello SAML token attributes dialog, add hello "firstName" attribute.</span></span>
 1. <span data-ttu-id="4c0f9-175">Kattintson a **Hozzáadás attribútum** tooopen hello **attribútum hozzáadása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-175">Click **Add attribute** tooopen hello **Add Attribute** dialog.</span></span>
 
    ![Egyszeri bejelentkezés konfigurálása][6]

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_05.png)
 2. <span data-ttu-id="4c0f9-178">A hello **attribútumnév** szövegmezőhöz hello attribútum neve "Keresztnév".</span><span class="sxs-lookup"><span data-stu-id="4c0f9-178">In hello **Attribute Name** textbox, type hello attribute name "firstName".</span></span>
 3. <span data-ttu-id="4c0f9-179">A hello **attribútumérték** listájában, jelölje be hello attribútum értéke "user.givenname".</span><span class="sxs-lookup"><span data-stu-id="4c0f9-179">From hello **Attribute Value** list, select hello attribute value "user.givenname".</span></span>
 4. <span data-ttu-id="4c0f9-180">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-180">Click **Ok**.</span></span>

4. <span data-ttu-id="4c0f9-181">A hello **SAP HANA felhő Platform identitás hitelesítési tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-181">On hello **SAP HANA Cloud Platform Identity Authentication Domain and URLs** section, perform hello following steps:</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_06.png)
 1. <span data-ttu-id="4c0f9-183">A hello **URL-cím bejelentkezési** szövegmező, írja be a hello bejelentkezési URL-címen hello SAP alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-183">In hello **Sign On URL** textbox, type hello sign on URL for hello SAP application.</span></span>
 2. <span data-ttu-id="4c0f9-184">A hello **azonosító** szövegmezőhöz típusú hello érték a következő mintát:`<entity-id>.accounts.ondemand.com`</span><span class="sxs-lookup"><span data-stu-id="4c0f9-184">In hello **Identifier** textbox, type hello value following pattern: `<entity-id>.accounts.ondemand.com`</span></span> 
    * <span data-ttu-id="4c0f9-185">Ha ez az érték nem tudja, kövesse hello SAP HANA felhőalapú Platform identitás hitelesítés dokumentáció [bérlői SAML 2.0 konfigurációs](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-185">If you don't know this value, please follow hello SAP HANA Cloud Platform Identity Authentication documentation on [Tenant SAML 2.0 Configuration](https://help.hana.ondemand.com/cloud_identity/frameset.htm?e81a19b0067f4646982d7200a8dab3ca.html).</span></span>

5. <span data-ttu-id="4c0f9-186">A hello **SAP HANA felhő Platform hitelesítési Identitáskonfigurációs** kattintson **SAP HANA felhő Platform identitás hitelesítés beállítása** tooopen **bejelentkezéskonfigurálása** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-186">On hello **SAP HANA Cloud Platform Identity Authentication Configuration** section, click **Configure SAP HANA Cloud Platform Identity Authentication** tooopen **Configure sign-on** dialog.</span></span> <span data-ttu-id="4c0f9-187">Kattintson a **SAML XML metaadatok** , és mentse hello fájlt a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-187">Then, click on **SAML XML Metadata** and save hello file on your computer.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_07.png) 

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_08.png)

6. <span data-ttu-id="4c0f9-190">az alkalmazáshoz konfigurált SSO tooget nyissa meg tooSAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-190">tooget SSO configured for your application, go tooSAP HANA Cloud Platform Identity Authentication Administration Console.</span></span> <span data-ttu-id="4c0f9-191">hello URL-címnek a következő mintát hello:`https://<tenant-id>.accounts.ondemand.com/admin`</span><span class="sxs-lookup"><span data-stu-id="4c0f9-191">hello URL has hello following pattern: `https://<tenant-id>.accounts.ondemand.com/admin`</span></span>
 * <span data-ttu-id="4c0f9-192">Ezután kövesse a hello dokumentáció a SAP HANA felhőalapú Platform identitás hitelesítés túl[konfigurálása a Microsoft Azure AD vállalati identitás-szolgáltatóként SAP HANA felhő Platform identitás hitelesítési](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-192">Then, follow hello documentation on SAP HANA Cloud Platform Identity Authentication too[Configure Microsoft Azure AD as Corporate Identity Provider at SAP HANA Cloud Platform Identity Authentication](https://help.hana.ondemand.com/cloud_identity/frameset.htm?626b17331b4d4014b8790d3aea70b240.html).</span></span> 

7. <span data-ttu-id="4c0f9-193">Hello Azure felügyeleti portálon kattintson **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-193">In hello Azure Management portal, click **Save** button.</span></span>
8. <span data-ttu-id="4c0f9-194">Az alábbi lépések csak akkor, ha tooadd és az egyszeri bejelentkezés engedélyezése egy másik SAP alkalmazással hello továbbra is.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-194">Continue hello following steps only if you want tooadd and enable SSO for another SAP application.</span></span> <span data-ttu-id="4c0f9-195">Ismételje meg a hello szakasz "Hozzáadása SAP HANA felhő Platform identitás hitelesítés hello gyűjteményből" tooadd SAP HANA felhőalapú Platform identitás hitelesítés egy másik példánya.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-195">Repeat steps under hello section “Adding SAP HANA Cloud Platform Identity Authentication from hello gallery” tooadd another instance of SAP HANA Cloud Platform Identity Authentication.</span></span>
9. <span data-ttu-id="4c0f9-196">Hello Azure felügyeleti portálon, a hello **SAP HANA felhőalapú Platform identitás hitelesítés** alkalmazás integráció lapján, kattintson a **társított bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-196">In hello Azure Management portal, on hello **SAP HANA Cloud Platform Identity Authentication** application integration page, click **Linked Sign-on**.</span></span>

    ![Csatolt bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/linked_sign_on.png)
10. <span data-ttu-id="4c0f9-198">Ezt követően mentse hello konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-198">Then, save hello configuration.</span></span>

>[!NOTE] 
><span data-ttu-id="4c0f9-199">Új alkalmazás hello hello SSO konfigurációs hello előző SAP alkalmazás fogja használni.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-199">hello new application will leverage hello SSO configuration for hello previous SAP application.</span></span> <span data-ttu-id="4c0f9-200">Győződjön meg arról, hogy használjon hello vállalati Identitásszolgáltatók azonos hello SAP HANA felhő Platform identitás hitelesítési felügyeleti konzolon.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-200">Please make sure you use hello same Corporate Identity Providers in hello SAP HANA Cloud Platform Identity Authentication Administration Console.</span></span>
>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="4c0f9-201">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="4c0f9-201">Create an Azure AD test user</span></span>
<span data-ttu-id="4c0f9-202">hello ebben a szakaszban célja toocreate tesztfelhasználó hello Britta Simon nevű új portálon.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-202">hello objective of this section is toocreate a test user in hello new portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="4c0f9-204">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="4c0f9-204">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c0f9-205">A hello **Azure Management portal**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-205">In hello **Azure Management portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="4c0f9-207">Nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó** toodisplay hello azoknak a felhasználóknak.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-207">Go too**Users and groups** and click **All users** toodisplay hello list of users.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="4c0f9-209">Hello párbeszédpanel hello tetején kattintson **Hozzáadás** tooopen hello **felhasználói** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-209">At hello top of hello dialog click **Add** tooopen hello **User** dialog.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="4c0f9-211">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="4c0f9-211">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/create_aaduser_04.png) 
  1. <span data-ttu-id="4c0f9-213">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-213">In hello **Name** textbox, type **BrittaSimon**.</span></span>
  2. <span data-ttu-id="4c0f9-214">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-214">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>
  3. <span data-ttu-id="4c0f9-215">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-215">Select **Show Password** and write down hello value of hello **Password**.</span></span>
  4. <span data-ttu-id="4c0f9-216">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-216">Click **Create**.</span></span> 

### <a name="create-a-sap-hana-cloud-platform-identity-authentication-test-user"></a><span data-ttu-id="4c0f9-217">SAP HANA felhőalapú Platform identitás hitelesítés tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="4c0f9-217">Create a SAP HANA Cloud Platform Identity Authentication test user</span></span>

<span data-ttu-id="4c0f9-218">Toocreate egy SAP HANA felhőalapú Platform identitás hitelesítés a felhasználó nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-218">You don't need toocreate an user on SAP HANA Cloud Platform Identity Authentication.</span></span> <span data-ttu-id="4c0f9-219">Az Azure AD hello felhasználókhoz tartozó tárolóban lévő felhasználók számára hello SSO funkció használható.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-219">Users who are in hello Azure AD user store can use hello SSO functionality.</span></span>

<span data-ttu-id="4c0f9-220">SAP HANA felhőalapú Platform identitás hitelesítés támogatja hello identitás-összevonási lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-220">SAP HANA Cloud Platform Identity Authentication supports hello Identity Federation option.</span></span> <span data-ttu-id="4c0f9-221">Ez a beállítás lehetővé teszi, hogy hello alkalmazás toocheck hello vállalati identitásszolgáltató által hitelesített hello felhasználók esetén a hello felhasználói tároló a SAP HANA felhőalapú Platform identitás hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-221">This option allows hello application toocheck if hello users authenticated by hello corporate identity provider exist in hello user store of SAP HANA Cloud Platform Identity Authentication.</span></span> 

<span data-ttu-id="4c0f9-222">Hello alapértelmezett beállítást, a hello identitás-összevonási beállítás le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-222">In hello default setting, hello Identity Federation option is disabled.</span></span> <span data-ttu-id="4c0f9-223">Identitás-összevonási engedélyezve van, csak az SAP HANA felhőalapú Platform identitás hitelesítés importált hello felhasználók esetén képes tooaccess hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-223">If Identity Federation is enabled, only hello users that are imported in SAP HANA Cloud Platform Identity Authentication are able tooaccess hello application.</span></span> 

<span data-ttu-id="4c0f9-224">További információ a hogyan tooenable vagy letilthatja az SAP HANA felhő Platform identitás hitelesítéssel, identitás-összevonási: identitás-összevonási engedélyezése SAP HANA felhő Platform identitás hitelesítéssel a [identitás-összevonás konfigurálása a tároló az SAP HANA felhő Platform identitás felhasználóhitelesítés hello. ](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span><span class="sxs-lookup"><span data-stu-id="4c0f9-224">For more information about how tooenable or disable Identity Federation with SAP HANA Cloud Platform Identity Authentication, see Enable Identity Federation with SAP HANA Cloud Platform Identity Authentication in [Configure Identity Federation with hello User Store of SAP HANA Cloud Platform Identity Authentication.](https://help.hana.ondemand.com/cloud_identity/frameset.htm?c029bbbaefbf4350af15115396ba14e2.html).</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="4c0f9-225">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="4c0f9-225">Assign hello Azure AD test user</span></span>

<span data-ttu-id="4c0f9-226">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés saját hozzáférés tooSAP HANA felhőalapú Platform identitás hitelesítés megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-226">In this section, you enable Britta Simon toouse Azure single sign-on by granting her access tooSAP HANA Cloud Platform Identity Authentication.</span></span>

![Felhasználó hozzárendelése][200] 

<span data-ttu-id="4c0f9-228">**tooassign Britta Simon tooSAP HANA felhőalapú Platform identitás hitelesítés, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="4c0f9-228">**tooassign Britta Simon tooSAP HANA Cloud Platform Identity Authentication, perform hello following steps:**</span></span>

1. <span data-ttu-id="4c0f9-229">Hello Azure felügyeleti portálján, nyissa meg a hello alkalmazások megtekintése, majd toohello könyvtár nézetben keresse meg, és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-229">In hello Azure Management portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="4c0f9-231">Hello alkalmazások listában válassza ki a **SAP HANA felhőalapú Platform identitás hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-231">In hello applications list, select **SAP HANA Cloud Platform Identity Authentication**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_sap_cloud_identity_09.png)

3. <span data-ttu-id="4c0f9-233">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-233">In hello menu on hello left, click **Users and groups**.</span></span>

    ![Felhasználó hozzárendelése][202] 

4. <span data-ttu-id="4c0f9-235">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-235">Click **Add** button.</span></span> <span data-ttu-id="4c0f9-236">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-236">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![Felhasználó hozzárendelése][203]

5. <span data-ttu-id="4c0f9-238">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-238">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="4c0f9-239">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-239">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="4c0f9-240">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-240">Click **Assign** button on **Add Assignment** dialog.</span></span>
    

### <a name="test-single-sign-on"></a><span data-ttu-id="4c0f9-241">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="4c0f9-241">Test single sign-on</span></span>

<span data-ttu-id="4c0f9-242">Ebben a szakaszban tesztelése az Azure AD SSO konfigurációs hello hozzáférési Panel használatával.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-242">In this section, you test your Azure AD SSO configuration using hello Access Panel.</span></span>

<span data-ttu-id="4c0f9-243">Ha a hozzáférési Panel hello hello SAP HANA felhőalapú Platform identitás hitelesítés csempe gombra kattint, automatikusan bejelentkezett tooyour SAP HANA felhőalapú Platform identitás hitelesítés alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="4c0f9-243">When you click hello SAP HANA Cloud Platform Identity Authentication tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA Cloud Platform Identity Authentication application.</span></span>


## <a name="additional-resources"></a><span data-ttu-id="4c0f9-244">További források</span><span class="sxs-lookup"><span data-stu-id="4c0f9-244">Additional resources</span></span>

* [<span data-ttu-id="4c0f9-245">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4c0f9-245">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="4c0f9-246">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="4c0f9-246">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_04.png
[5]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_05.png
[6]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_06.png

[100]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sap-hana-cloud-platform-identity-authentication-tutorial/tutorial_general_203.png