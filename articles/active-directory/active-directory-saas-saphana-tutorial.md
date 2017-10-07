---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP HANA |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP HANA között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: cef4a146-f4b0-4e94-82de-f5227a4b462c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: jeedes
ms.openlocfilehash: 5ff6bfde0b1d7ab14025a4bc45199f9f826affd1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-hana"></a><span data-ttu-id="a38e7-103">Oktatóanyag: Azure Active Directoryval integrált SAP HANA</span><span class="sxs-lookup"><span data-stu-id="a38e7-103">Tutorial: Azure Active Directory integration with SAP HANA</span></span>

<span data-ttu-id="a38e7-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP HANA az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="a38e7-104">In this tutorial, you learn how toointegrate SAP HANA with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="a38e7-105">SAP HANA integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="a38e7-105">Integrating SAP HANA with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="a38e7-106">Megadhatja a hozzáférés tooSAP HANA rendelkező Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="a38e7-106">You can control in Azure AD who has access tooSAP HANA</span></span>
- <span data-ttu-id="a38e7-107">Engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP HANA (egyszeri bejelentkezés) a saját Azure AD-fiókok</span><span class="sxs-lookup"><span data-stu-id="a38e7-107">You can enable your users tooautomatically get signed-on tooSAP HANA (Single Sign-On) with their Azure AD accounts</span></span>
- <span data-ttu-id="a38e7-108">Kezelheti a fiókokat, egy központi helyen - hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a38e7-108">You can manage your accounts in one central location - hello Azure portal</span></span>

<span data-ttu-id="a38e7-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="a38e7-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="a38e7-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a38e7-110">Prerequisites</span></span>

<span data-ttu-id="a38e7-111">az Azure AD integrálása SAP HANA tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="a38e7-111">tooconfigure Azure AD integration with SAP HANA, you need hello following items:</span></span>

- <span data-ttu-id="a38e7-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="a38e7-112">An Azure AD subscription</span></span>
- <span data-ttu-id="a38e7-113">Egy SAP HANA egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="a38e7-113">A SAP HANA single sign-on enabled subscription</span></span>
- <span data-ttu-id="a38e7-114">Egy futó HANA példány bármely nyilvános IaaS, a helyszíni vagy Azure virtuális gépeken vagy SAP nagy példányok az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="a38e7-114">A running HANA Instance either on any public IaaS, on-premises, Azure VMs or SAP Large Instances in Azure</span></span>
- <span data-ttu-id="a38e7-115">hello XSA felügyeleti webes felülete, valamint a HANA Studio telepítve hello HANA példány.</span><span class="sxs-lookup"><span data-stu-id="a38e7-115">hello XSA Administration Web Interface as well as HANA Studio installed on hello HANA instance.</span></span>

> [!NOTE]
> <span data-ttu-id="a38e7-116">tootest hello lépéseit az oktatóanyag, nem javasoljuk a SAP HANA éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a38e7-116">tootest hello steps in this tutorial, we do not recommend using a production environment of SAP HANA.</span></span> <span data-ttu-id="a38e7-117">Vizsgálat hello integrációs először fejlesztési vagy átmeneti környezet hello alkalmazás, majd használja hello éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="a38e7-117">Test hello integration first in development or staging environment of hello application and then use hello production environment.</span></span>

<span data-ttu-id="a38e7-118">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="a38e7-118">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="a38e7-119">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a38e7-119">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="a38e7-120">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a38e7-120">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="a38e7-121">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="a38e7-121">Scenario description</span></span>
<span data-ttu-id="a38e7-122">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="a38e7-122">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="a38e7-123">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="a38e7-123">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="a38e7-124">SAP HANA hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a38e7-124">Adding SAP HANA from hello gallery</span></span>
2. <span data-ttu-id="a38e7-125">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a38e7-125">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-hana-from-hello-gallery"></a><span data-ttu-id="a38e7-126">SAP HANA hozzáadása hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="a38e7-126">Adding SAP HANA from hello gallery</span></span>
<span data-ttu-id="a38e7-127">tooconfigure hello integrációja SAP HANA az Azure AD-be, meg kell tooadd SAP HANA hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="a38e7-127">tooconfigure hello integration of SAP HANA into Azure AD, you need tooadd SAP HANA from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="a38e7-128">**tooadd SAP HANA hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a38e7-128">**tooadd SAP HANA from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="a38e7-129">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-129">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="a38e7-131">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-131">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="a38e7-132">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-132">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="a38e7-134">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="a38e7-134">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="a38e7-136">Hello keresési mezőbe, írja be a **SAP HANA**, jelölje be **SAP HANA** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-136">In hello search box, type **SAP HANA**, select **SAP HANA** from result panel then click **Add** button tooadd hello application.</span></span> 

    ![Új alkalmazás hello](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a><span data-ttu-id="a38e7-138">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="a38e7-138">Configuring and testing Azure AD single sign-on</span></span>
<span data-ttu-id="a38e7-139">Ebben a szakaszban, tesztelése és konfigurálása az Azure AD egyszeri bejelentkezést az SAP HANA "Britta Simon." nevű tesztfelhasználó alapján</span><span class="sxs-lookup"><span data-stu-id="a38e7-139">In this section, you configure and test Azure AD single sign-on with SAP HANA based on a test user called "Britta Simon."</span></span>

<span data-ttu-id="a38e7-140">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello megfelelőjére felhasználó SAP HANA tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="a38e7-140">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP HANA is tooa user in Azure AD.</span></span> <span data-ttu-id="a38e7-141">Ez azt jelenti egy Azure AD-felhasználó és a kapcsolódó felhasználó hello SAP HANA közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="a38e7-141">In other words, a link relationship between an Azure AD user and hello related user in SAP HANA needs toobe established.</span></span>

<span data-ttu-id="a38e7-142">SAP HANA, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a38e7-142">In SAP HANA, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="a38e7-143">tooconfigure és az Azure AD az egyszeri bejelentkezés SAP HANA-teszthez, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="a38e7-143">tooconfigure and test Azure AD single sign-on with SAP HANA, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="a38e7-144">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configuring-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a38e7-144">**[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="a38e7-145">**[Az Azure AD tesztfelhasználó létrehozása](#creating-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="a38e7-145">**[Creating an Azure AD test user](#creating-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="a38e7-146">**[Egy SAP HANA tesztfelhasználó létrehozása](#creating-a-sap-hana-test-user)**  -toohave egy megfelelője a Britta Simon az SAP HANA, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="a38e7-146">**[Creating a SAP HANA test user](#creating-a-sap-hana-test-user)** - toohave a counterpart of Britta Simon in SAP HANA that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="a38e7-147">**[Hozzárendelése az Azure AD hello tesztfelhasználó](#assigning-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a38e7-147">**[Assigning hello Azure AD test user](#assigning-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="a38e7-148">**[Egyszeri bejelentkezés tesztelése](#testing-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="a38e7-148">**[Testing Single Sign-On](#testing-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configuring-azure-ad-single-sign-on"></a><span data-ttu-id="a38e7-149">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a38e7-149">Configuring Azure AD single sign-on</span></span>

<span data-ttu-id="a38e7-150">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése és konfigurálása egyszeri bejelentkezéshez az SAP HANA-alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="a38e7-150">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP HANA application.</span></span>

<span data-ttu-id="a38e7-151">**az Azure AD tooconfigure egyszeri bejelentkezést az SAP HANA, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="a38e7-151">**tooconfigure Azure AD single sign-on with SAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="a38e7-152">Az Azure portál, a hello hello **SAP HANA** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-152">In hello Azure portal, on hello **SAP HANA** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása][4]

2. <span data-ttu-id="a38e7-154">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="a38e7-154">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_samlbase.png)

3. <span data-ttu-id="a38e7-156">A hello **SAP HANA-tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a38e7-156">On hello **SAP HANA Domain and URLs** section, perform hello following steps:</span></span>

    ![Tartomány- és URL-címeket az egyszeri bejelentkezés információk](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_url.png)

    <span data-ttu-id="a38e7-158">a.</span><span class="sxs-lookup"><span data-stu-id="a38e7-158">a.</span></span> <span data-ttu-id="a38e7-159">A hello **azonosító** szövegmezőhöz típusú, mint:`HA100`</span><span class="sxs-lookup"><span data-stu-id="a38e7-159">In hello **Identifier** textbox, type as: `HA100`</span></span> 

    <span data-ttu-id="a38e7-160">b.</span><span class="sxs-lookup"><span data-stu-id="a38e7-160">b.</span></span> <span data-ttu-id="a38e7-161">A hello **válasz URL-CÍMEN** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span><span class="sxs-lookup"><span data-stu-id="a38e7-161">In hello **Reply URL** textbox, type a URL using hello following pattern: `https://<Customer-SAP-instance-url>/sap/hana/xs/saml/login.xscfunc`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a38e7-162">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="a38e7-162">These values are not real.</span></span> <span data-ttu-id="a38e7-163">Frissítheti ezeket az értékeket hello tényleges azonosítója és a válasz URL-címmel.</span><span class="sxs-lookup"><span data-stu-id="a38e7-163">Update these values with hello actual Identifier and Reply URL.</span></span> <span data-ttu-id="a38e7-164">Ügyfél [SAP HANA-ügyfél-támogatási csoport](https://cloudplatform.sap.com/contact.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="a38e7-164">Contact [SAP HANA Client support team](https://cloudplatform.sap.com/contact.html) tooget these values.</span></span> 

4. <span data-ttu-id="a38e7-165">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a38e7-165">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_certificate.png) 

    >[!Note]
    ><span data-ttu-id="a38e7-167">Ha a tanúsítvány nincs aktív majd aktiválja az hello kattintva "Ellenőrizze új tanúsítvány aktív" jelölőnégyzetet a hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a38e7-167">If certificate is not active then make it active by clicking hello “Make new certificate active” checkbox in hello Azure AD.</span></span> 

5. <span data-ttu-id="a38e7-168">SAP HANA-alkalmazás hello SAML helyességi feltételek vár egy meghatározott formátumban.</span><span class="sxs-lookup"><span data-stu-id="a38e7-168">SAP HANA application expects hello SAML assertions in a specific format.</span></span> <span data-ttu-id="a38e7-169">a következő képernyőkép hello ezen mutat egy példát.</span><span class="sxs-lookup"><span data-stu-id="a38e7-169">hello following screenshot shows an example for this.</span></span> <span data-ttu-id="a38e7-170">Itt azt leképezett hello **felhasználói azonosító** rendelkező **ExtractMailPrefix()** funkciója **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-170">Here we have mapped hello **User Identifier** with **ExtractMailPrefix()** function of **user.mail**.</span></span> <span data-ttu-id="a38e7-171">Így az hello előtag értéke az e-mailek hello felhasználó, amelynek van hello egyedi felhasználói azonosító.</span><span class="sxs-lookup"><span data-stu-id="a38e7-171">This gives hello prefix value of email of hello user which is hello unique User ID.</span></span> <span data-ttu-id="a38e7-172">Minden sikeres választ küldött toohello SAP HANA-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a38e7-172">This is sent toohello SAP HANA application in every successful response.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-saphana-tutorial/attribute.png)

6. <span data-ttu-id="a38e7-174">A hello **felhasználói attribútumok** szakaszt, hello **egyszeri bejelentkezés** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="a38e7-174">In hello **User Attributes** section on hello **Single sign-on** dialog:</span></span>

    <span data-ttu-id="a38e7-175">a.</span><span class="sxs-lookup"><span data-stu-id="a38e7-175">a.</span></span> <span data-ttu-id="a38e7-176">A hello **felhasználói azonosító** legördülő listában válassza ki **ExtractMailPrefix**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-176">In hello **User Identifier** dropdown list, select **ExtractMailPrefix**.</span></span>
    
    <span data-ttu-id="a38e7-177">b.</span><span class="sxs-lookup"><span data-stu-id="a38e7-177">b.</span></span> <span data-ttu-id="a38e7-178">A hello **Mail** legördülő listában válassza ki **user.mail**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-178">In hello **Mail** dropdown list, select **user.mail**.</span></span>

7. <span data-ttu-id="a38e7-179">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-179">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-saphana-tutorial/tutorial_general_400.png)
    
8. <span data-ttu-id="a38e7-181">tooconfigure egyszeri bejelentkezést a **SAP HANA** oldalon, a bejelentkezési tooyour **HANA XSA Webkonzol** toohello tallózással megfelelő HTTPS-végpont.</span><span class="sxs-lookup"><span data-stu-id="a38e7-181">tooconfigure single sign-on on **SAP HANA** side, login tooyour **HANA XSA Web Console**  by browsing toohello respective HTTPS-endpoint.</span></span>

    > [!Note]
    > <span data-ttu-id="a38e7-182">Hello alapértelmezés szerint a hello URL-címet átirányítja a felhasználókat hello kérelem tooa bejelentkezési képernyő, amely egy hitelesített SAP HANA adatbázis felhasználói toocomplete hello bejelentkezési folyamat hello hitelesítő adatok szükségesek.</span><span class="sxs-lookup"><span data-stu-id="a38e7-182">In hello default configuration, hello URL redirects hello request tooa logon screen, which requires hello credentials of an authenticated SAP HANA database user toocomplete hello logon process.</span></span> <span data-ttu-id="a38e7-183">hello bejelentkező felhasználó hello jogosultságokkal szükséges tooperform SAML-alapú felügyeleti feladatok kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="a38e7-183">hello user who logs on must have hello privileges required tooperform SAML administration tasks.</span></span>

9. <span data-ttu-id="a38e7-184">Hello XSA webes felülete, lépjen a túl**SAML-Identitásszolgáltatóként** ott, kattintson a hello **"+"** -hello alján, hello képernyő toodisplay hello identitás szolgáltató adatai hozzáadása panel gombra, majd hajtsa végre hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="a38e7-184">In hello XSA Web Interface, navigate too**SAML Identity Provider** and from there, click hello **“+”** -button on hello bottom of hello screen toodisplay hello Add Identity Provider Info pane and perform hello following steps:</span></span>

    ![Identitás-szolgáltató felvétele](./media/active-directory-saas-saphana-tutorial/sap1.png)

    <span data-ttu-id="a38e7-186">a.</span><span class="sxs-lookup"><span data-stu-id="a38e7-186">a.</span></span> <span data-ttu-id="a38e7-187">A hello **identitás szolgáltató adatai hozzáadása** ablaktáblán, Beillesztés hello tartalmát hello metaadat-XML fájlok, amelyek már letöltötte az Azure portálról történő hello **metaadatok** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a38e7-187">In hello **Add Identity Provider Info** pane, paste hello contents of hello Metadata XML, which you have downloaded from Azure portal into hello **Metadata** textbox.</span></span>

    ![Identitásszolgáltató beállítások hozzáadása](./media/active-directory-saas-saphana-tutorial/sap2.png)

    <span data-ttu-id="a38e7-189">b.</span><span class="sxs-lookup"><span data-stu-id="a38e7-189">b.</span></span> <span data-ttu-id="a38e7-190">Hello XML-dokumentum tartalmát hello érvényesek, ha folyamat elemzése hello hello szükséges adatokat tooinsert kibontja a hello **tulajdonosa Entitásazonosító és kibocsátó** hello általános adatok mezőinek képernyőn területen, és a hello hello mező URL-címe Képernyő célterületre, például  **SingleSignOn és alap URL-címe (*)**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-190">If hello contents of hello XML document are valid, hello parsing process extracts hello information required tooinsert into hello **Subject, Entity ID, and Issuer** fields in hello General Data screen area, and hello URL fields in hello Destination screen area, for example, **Base URL and SingleSignOn URL (*)**.</span></span>

    ![Identitásszolgáltató beállítások hozzáadása](./media/active-directory-saas-saphana-tutorial/sap3.png)

    <span data-ttu-id="a38e7-192">c.</span><span class="sxs-lookup"><span data-stu-id="a38e7-192">c.</span></span> <span data-ttu-id="a38e7-193">Az általános adatok hello hello név mezőben területen kattintson a jobb adjon meg egy nevet az új SAML SSO identitásszolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="a38e7-193">In hello Name box of hello General Data screen area, enter a name for hello new SAML SSO identity provider.</span></span>

    > [!Note]
    > <span data-ttu-id="a38e7-194">hello SAML IDP hello nevét kötelező megadni, és egyedi; kell lennie Ha kiválasztható SAML hello hitelesítési módszer az SAP HANA XS alkalmazások toouse, például hello hitelesítési képernyő területén hello XS összetevő-felügyeleti eszköz szerepel érhető el, akkor jelenik meg, SAML IDPs hello listája.</span><span class="sxs-lookup"><span data-stu-id="a38e7-194">hello name of hello SAML IDP is mandatory and must be unique; it appears in hello list of available SAML IDPs that is displayed, if you select SAML as hello authentication method for SAP HANA XS applications toouse, for example, in hello Authentication screen area of hello XS Artifact Administration tool.</span></span>

10. <span data-ttu-id="a38e7-195">Mentse a hello új SAML-Identitásszolgáltatóként hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="a38e7-195">Save hello details of hello new SAML identity provider.</span></span> <span data-ttu-id="a38e7-196">Válasszon **mentése** toosave hello hello SAML-Identitásszolgáltatóként részleteit, és adja hozzá a hello új SAML IDP toohello listája ismert SAML IDPs.</span><span class="sxs-lookup"><span data-stu-id="a38e7-196">Choose **Save** toosave hello details of hello SAML identity provider and add hello new SAML IDP toohello list of known SAML IDPs.</span></span>

    ![Mentés gombja](./media/active-directory-saas-saphana-tutorial/sap4.png)

11. <span data-ttu-id="a38e7-198">Hello rendszer tulajdonságainak hello belül HANA Studio **konfigurációs** lapján csak a beállítások alapján szűrése **saml** , és módosítsa a hello **assertion_timeout** a**10 másodpercnél** túl**120 másodperc**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-198">In HANA Studio within hello system properties of hello **Configuration** tab, just filter settings by **saml** and adjust hello **assertion_timeout** from **10 sec** too**120 sec**.</span></span>

    ![assertion_timeout beállítás](./media/active-directory-saas-saphana-tutorial/sap7.png)

> [!TIP]
> <span data-ttu-id="a38e7-200">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="a38e7-200">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="a38e7-201">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="a38e7-201">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="a38e7-202">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="a38e7-202">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="creating-an-azure-ad-test-user"></a><span data-ttu-id="a38e7-203">Az Azure AD tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a38e7-203">Creating an Azure AD test user</span></span>
<span data-ttu-id="a38e7-204">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="a38e7-204">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Az Azure AD-felhasználó létrehozása][100]

<span data-ttu-id="a38e7-206">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="a38e7-206">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="a38e7-207">A hello **Azure-portálon**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-207">In hello **Azure portal**, on hello left navigation pane, click **Azure Active Directory** icon.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-saphana-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="a38e7-209">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok** kattintson **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-209">toodisplay hello list of users, go too**Users and groups** and click **All users**.</span></span>
    
    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-saphana-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="a38e7-211">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello felül hello párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a38e7-211">tooopen hello **User** dialog, click **Add** on hello top of hello dialog.</span></span>
 
    ![hello Hozzáadás gomb](./media/active-directory-saas-saphana-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="a38e7-213">A hello **felhasználói** párbeszédpanel lapon, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="a38e7-213">On hello **User** dialog page, perform hello following steps:</span></span>
 
    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-saphana-tutorial/create_aaduser_04.png) 

    <span data-ttu-id="a38e7-215">a.</span><span class="sxs-lookup"><span data-stu-id="a38e7-215">a.</span></span> <span data-ttu-id="a38e7-216">A hello **neve** szövegmezőhöz típus **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-216">In hello **Name** textbox, type **BrittaSimon**.</span></span>

    <span data-ttu-id="a38e7-217">b.</span><span class="sxs-lookup"><span data-stu-id="a38e7-217">b.</span></span> <span data-ttu-id="a38e7-218">A hello **felhasználónév** szövegmezőhöz típus hello **e-mail cím** a BrittaSimon.</span><span class="sxs-lookup"><span data-stu-id="a38e7-218">In hello **User name** textbox, type hello **email address** of BrittaSimon.</span></span>

    <span data-ttu-id="a38e7-219">c.</span><span class="sxs-lookup"><span data-stu-id="a38e7-219">c.</span></span> <span data-ttu-id="a38e7-220">Válassza ki **megjelenítése jelszó** írja le hello hello értékének **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-220">Select **Show Password** and write down hello value of hello **Password**.</span></span>

    <span data-ttu-id="a38e7-221">d.</span><span class="sxs-lookup"><span data-stu-id="a38e7-221">d.</span></span> <span data-ttu-id="a38e7-222">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-222">Click **Create**.</span></span>
 
### <a name="creating-a-sap-hana-test-user"></a><span data-ttu-id="a38e7-223">Egy SAP HANA tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="a38e7-223">Creating a SAP HANA test user</span></span>

<span data-ttu-id="a38e7-224">az Azure AD tooenable felhasználók toolog a tooSAP HANA, akkor ki kell építenie az SAP HANA.</span><span class="sxs-lookup"><span data-stu-id="a38e7-224">tooenable Azure AD users toolog in tooSAP HANA, they must be provisioned into SAP HANA.</span></span>
<span data-ttu-id="a38e7-225">SAP HANA támogatja just-in-time kiosztást, amely alapértelmezés szerint van engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="a38e7-225">SAP HANA supports just-in-time provisioning, which is by default enabled.</span></span>

<span data-ttu-id="a38e7-226">Ha egy felhasználó toocreate manuálisan kell, hajtsa végre a lépéseket követve hello:</span><span class="sxs-lookup"><span data-stu-id="a38e7-226">If you need toocreate a user manually, perform hello following steps:</span></span>

>[!Note]
><span data-ttu-id="a38e7-227">Hello hello felhasználó által használt külső hitelesítés módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="a38e7-227">You can change hello external authentication used by hello user.</span></span>
<span data-ttu-id="a38e7-228">A külső rendszerek, például egy Kerberos rendszer a külső felhasználók hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="a38e7-228">External users are authenticated using an external system, for example a Kerberos system.</span></span> <span data-ttu-id="a38e7-229">Külső identitások kapcsolatos részletes információkért forduljon a [tartományi rendszergazda](https://cloudplatform.sap.com/contact.html).</span><span class="sxs-lookup"><span data-stu-id="a38e7-229">For detailed information about external identities, contact your [domain administrator](https://cloudplatform.sap.com/contact.html).</span></span>

1. <span data-ttu-id="a38e7-230">Nyissa meg hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) , rendszergazda, és engedélyezze a SAML SSO hello adatbázis-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a38e7-230">Open hello [SAP HANA Studio](https://help.sap.com/viewer/a2a49126a5c546a9864aae22c05c3d0e/2.0.01/en-us) as an administrator and enable hello DB-User for SAML SSO.</span></span>

    ![Felhasználó létrehozása](./media/active-directory-saas-saphana-tutorial/sap5.png)

2. <span data-ttu-id="a38e7-232">Osztásjelek hello láthatatlan jelölőnégyzet toohello a bal oldali **SAML** és hello beállítása hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-232">Tick hello invisible checkbox toohello left of **SAML** and follow hello Configure link.</span></span>

3. <span data-ttu-id="a38e7-233">Kattintson a **Hozzáadás** tooadd hello SAML IDP, és kattintson a **OK** kiválasztásával hello megfelelő SAML IDP.</span><span class="sxs-lookup"><span data-stu-id="a38e7-233">Click **Add** tooadd hello SAML IDP and click **OK** selecting hello appropriate SAML IDP.</span></span>

4. <span data-ttu-id="a38e7-234">Adja hozzá a hello **külső identitások** (például)</span><span class="sxs-lookup"><span data-stu-id="a38e7-234">Add hello **External Identity** (ex.</span></span> <span data-ttu-id="a38e7-235">Itt BrittaSimon), vagy válasszon **"Bármely"** kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-235">BrittaSimon here) or choose **"Any"** and click **OK**.</span></span>

    >[!Note]
    ><span data-ttu-id="a38e7-236">Ha nincs bejelölve a "Bármely" jelölőnégyzetet, majd HANA hello a felhasználónév kell tooexactly egyezés hello hello UPN hello tartományi utótag elé hello felhasználó nevét (pl. BrittaSimon@contoso.com válna a HANA BrittaSimon).</span><span class="sxs-lookup"><span data-stu-id="a38e7-236">If "ANY" check-box is not checked, then hello user name in HANA needs tooexactly match hello name of hello user in hello UPN before hello domain suffix (i.e. BrittaSimon@contoso.com would become BrittaSimon in HANA).</span></span>

5. <span data-ttu-id="a38e7-237">Tesztelési célokra, rendelje hozzá az összes **"XS"** toohello felhasználói szerepköröket.</span><span class="sxs-lookup"><span data-stu-id="a38e7-237">For testing purposes, assign all **"XS"** roles toohello user.</span></span>

    ![szerepkörök hozzárendelése](./media/active-directory-saas-saphana-tutorial/sap6.png)

    > [!TIP]
    > <span data-ttu-id="a38e7-239">A használati esetek, csak a megfelelő engedélyeket kell adnia.</span><span class="sxs-lookup"><span data-stu-id="a38e7-239">You should give those permissions appropriate for your use cases, only.</span></span>

6. <span data-ttu-id="a38e7-240">Hello felhasználói mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a38e7-240">Save hello user.</span></span>

### <a name="assigning-hello-azure-ad-test-user"></a><span data-ttu-id="a38e7-241">Az Azure AD hello tesztfelhasználó hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="a38e7-241">Assigning hello Azure AD test user</span></span>

<span data-ttu-id="a38e7-242">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAP HANA megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="a38e7-242">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP HANA.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="a38e7-244">**tooassign Britta Simon tooSAP HANA, hajtsa végre a következő lépéseket hello:**</span><span class="sxs-lookup"><span data-stu-id="a38e7-244">**tooassign Britta Simon tooSAP HANA, perform hello following steps:**</span></span>

1. <span data-ttu-id="a38e7-245">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-245">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="a38e7-247">Hello alkalmazások listában válassza ki a **SAP HANA**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-247">In hello applications list, select **SAP HANA**.</span></span>

    ![Felhasználó hozzárendelése](./media/active-directory-saas-saphana-tutorial/tutorial_saphana_app.png) 

3. <span data-ttu-id="a38e7-249">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="a38e7-249">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202] 

4. <span data-ttu-id="a38e7-251">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="a38e7-251">Click **Add** button.</span></span> <span data-ttu-id="a38e7-252">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a38e7-252">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="a38e7-254">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="a38e7-254">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="a38e7-255">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a38e7-255">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="a38e7-256">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="a38e7-256">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="testing-single-sign-on"></a><span data-ttu-id="a38e7-257">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="a38e7-257">Testing single sign-on</span></span>

<span data-ttu-id="a38e7-258">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="a38e7-258">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="a38e7-259">Ha a hozzáférési Panel hello hello SAP HANA csempe gombra kattint, SAP HANA-alkalmazás automatikusan bejelentkezett tooyour szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="a38e7-259">When you click hello SAP HANA tile in hello Access Panel, you should get automatically signed-on tooyour SAP HANA application.</span></span>
<span data-ttu-id="a38e7-260">A hozzáférési Panel kapcsolatos további információkért lásd: [hozzáférési Panel bemutatása toohello](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="a38e7-260">For more information about the Access Panel, see [Introduction toohello Access Panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a38e7-261">További források</span><span class="sxs-lookup"><span data-stu-id="a38e7-261">Additional resources</span></span>

* [<span data-ttu-id="a38e7-262">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a38e7-262">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="a38e7-263">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="a38e7-263">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)



<!--Image references-->

[1]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-saphana-tutorial/tutorial_general_203.png

