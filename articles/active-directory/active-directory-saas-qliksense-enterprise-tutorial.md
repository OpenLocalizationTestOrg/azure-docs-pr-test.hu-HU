---
title: "Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és Qlik logika vállalati között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 8c27e340-2b25-47b6-bf1f-438be4c14f93
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: jeedes
ms.openlocfilehash: 153edb3f8cd41790d4e9a74f15cfb806e5e9e3b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-qlik-sense-enterprise"></a><span data-ttu-id="0cc7c-103">Oktatóanyag: Azure Active Directory-integráció a Qlik logika vállalati</span><span class="sxs-lookup"><span data-stu-id="0cc7c-103">Tutorial: Azure Active Directory integration with Qlik Sense Enterprise</span></span>

<span data-ttu-id="0cc7c-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate Qlik logika vállalati az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="0cc7c-104">In this tutorial, you learn how toointegrate Qlik Sense Enterprise with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="0cc7c-105">Qlik logika vállalati integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-105">Integrating Qlik Sense Enterprise with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="0cc7c-106">Az Azure AD hozzáférési tooQlik logika vállalati rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-106">You can control in Azure AD who has access tooQlik Sense Enterprise.</span></span>
- <span data-ttu-id="0cc7c-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooQlik logika vállalati (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="0cc7c-107">You can enable your users tooautomatically get signed-on tooQlik Sense Enterprise (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="0cc7c-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="0cc7c-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="0cc7c-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0cc7c-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0cc7c-110">Prerequisites</span></span>

<span data-ttu-id="0cc7c-111">az Azure AD integrálása a Qlik logika vállalati tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-111">tooconfigure Azure AD integration with Qlik Sense Enterprise, you need hello following items:</span></span>

- <span data-ttu-id="0cc7c-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="0cc7c-112">An Azure AD subscription</span></span>
- <span data-ttu-id="0cc7c-113">Egy Qlik logika vállalati egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="0cc7c-113">A Qlik Sense Enterprise single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="0cc7c-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="0cc7c-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="0cc7c-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="0cc7c-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, egy hónapos próbaverzió Itt kaphat: [próbaverzió ajánlat](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0cc7c-117">If you don't have an Azure AD trial environment, you can get a one-month trial here: [Trial offer](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="0cc7c-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-118">Scenario description</span></span>
<span data-ttu-id="0cc7c-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="0cc7c-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="0cc7c-121">Hello gyűjteményből Qlik logika vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-121">Adding Qlik Sense Enterprise from hello gallery</span></span>
2. <span data-ttu-id="0cc7c-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="0cc7c-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-qlik-sense-enterprise-from-hello-gallery"></a><span data-ttu-id="0cc7c-123">Hello gyűjteményből Qlik logika vállalati hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-123">Adding Qlik Sense Enterprise from hello gallery</span></span>
<span data-ttu-id="0cc7c-124">tooconfigure hello integrációs Qlik logika vállalat az Azure AD-be, meg kell tooadd Qlik logika vállalati hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-124">tooconfigure hello integration of Qlik Sense Enterprise into Azure AD, you need tooadd Qlik Sense Enterprise from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="0cc7c-125">**tooadd Qlik logika vállalati hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-125">**tooadd Qlik Sense Enterprise from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cc7c-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="0cc7c-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="0cc7c-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="0cc7c-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="0cc7c-133">Hello keresési mezőbe, írja be a **Qlik logika vállalati**, jelölje be **Qlik logika vállalati** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-133">In hello search box, type **Qlik Sense Enterprise**, select **Qlik Sense Enterprise** from result panel then click **Add** button tooadd hello application.</span></span>

    ![Qlik logika vállalati hello eredmények listájában](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="0cc7c-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="0cc7c-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés Qlik logika vállalati "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-136">In this section, you configure and test Azure AD single sign-on with Qlik Sense Enterprise based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="0cc7c-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói Qlik logika vállalati tooa felhasználó az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in Qlik Sense Enterprise is tooa user in Azure AD.</span></span> <span data-ttu-id="0cc7c-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello Qlik logika vállalati közötti kapcsolat kapcsolatot kell létrehozni toobe.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-138">In other words, a link relationship between an Azure AD user and hello related user in Qlik Sense Enterprise needs toobe established.</span></span>

<span data-ttu-id="0cc7c-139">Qlik logika vállalati, rendelje hozzá hello hello értékének **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-139">In Qlik Sense Enterprise, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="0cc7c-140">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a Qlik logika vállalati, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-140">tooconfigure and test Azure AD single sign-on with Qlik Sense Enterprise, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="0cc7c-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="0cc7c-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="0cc7c-143">**[Qlik logika vállalati tesztfelhasználó létrehozása](#create-a-qlik-sense-enterprise-test-user)**  -toohave egy megfelelője a Britta Simon Qlik logika vállalat, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-143">**[Create a Qlik Sense Enterprise test user](#create-a-qlik-sense-enterprise-test-user)** - toohave a counterpart of Britta Simon in Qlik Sense Enterprise that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="0cc7c-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="0cc7c-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="0cc7c-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="0cc7c-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az Qlik logika vállalati alkalmazás egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your Qlik Sense Enterprise application.</span></span>

<span data-ttu-id="0cc7c-148">**tooconfigure az Azure AD egyszeri bejelentkezést a Qlik logika vállalati, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-148">**tooconfigure Azure AD single sign-on with Qlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cc7c-149">Az Azure portál, a hello hello **Qlik logika vállalati** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-149">In hello Azure portal, on hello **Qlik Sense Enterprise** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="0cc7c-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_samlbase.png)

3. <span data-ttu-id="0cc7c-153">A hello **Qlik logika vállalati tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-153">On hello **Qlik Sense Enterprise Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk Qlik logika vállalati tartomány és az URL-címek](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_url.png)

    <span data-ttu-id="0cc7c-155">a.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-155">a.</span></span> <span data-ttu-id="0cc7c-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span><span class="sxs-lookup"><span data-stu-id="0cc7c-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<Qlik Sense Fully Qualifed Hostname>:443//samlauthn/`</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0cc7c-157">Vegye figyelembe, záró perjelet ezt az URI hello végén hello.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-157">Note hello trailing slash at hello end of this URI.</span></span> <span data-ttu-id="0cc7c-158">Szükség.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-158">It is required.</span></span>
    
    <span data-ttu-id="0cc7c-159">b.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-159">b.</span></span> <span data-ttu-id="0cc7c-160">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-160">In hello **Identifier** textbox, type a URL using hello following pattern:</span></span>
    | |
    |--|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qlikpoc.com`|
    | `https://<Qlik Sense Fully Qualifed Hostname>.qliksense.com`|

    > [!NOTE] 
    > <span data-ttu-id="0cc7c-161">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-161">These values are not real.</span></span> <span data-ttu-id="0cc7c-162">Ezeket az értékeket a hello tényleges bejelentkezési URL-cím és azonosítója, amelyeket ez az oktatóanyag, vagy forduljon a frissítés [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-162">Update these values with hello actual Sign-On URL and Identifier, Which are explained later in this tutorial or contact [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) tooget these values.</span></span> 

4. <span data-ttu-id="0cc7c-163">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-163">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_certificate.png) 

5. <span data-ttu-id="0cc7c-165">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-165">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="0cc7c-167">Készítse elő a hello összevonási metaadatok XML-fájlt, hogy feltöltheti tooQlik logika kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-167">Prepare hello Federation Metadata XML file so that you can upload that tooQlik Sense server.</span></span>
   
    > [!NOTE]
    > <span data-ttu-id="0cc7c-168">Hello IdP metaadatok toohello Qlik logika server feltöltését, mielőtt hello fájlt kell szerkeszteni toobe tooremove információk tooensure megfelelő működéséhez az Azure AD között és Qlik logika kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-168">Before uploading hello IdP metadata toohello Qlik Sense server, hello file needs toobe edited tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>
    
    ![QlikSense][qs24]
   
    <span data-ttu-id="0cc7c-170">a.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-170">a.</span></span> <span data-ttu-id="0cc7c-171">Nyissa meg szövegszerkesztőben az Azure portálról letöltött hello FederationMetaData.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-171">Open hello FederationMetaData.xml file, which you have downloaded from Azure portal in a text editor.</span></span>
   
    <span data-ttu-id="0cc7c-172">b.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-172">b.</span></span> <span data-ttu-id="0cc7c-173">Keresse meg hello érték **RoleDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-173">Search for hello value **RoleDescriptor**.</span></span>  <span data-ttu-id="0cc7c-174">Négy bejegyzést (nyitó és záró elemcímkék két pár) is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-174">There are four entries (two pairs of opening and closing element tags).</span></span>
   
    <span data-ttu-id="0cc7c-175">c.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-175">c.</span></span> <span data-ttu-id="0cc7c-176">Törölje hello RoleDescriptor címkéket és az összes adatot a kettő között hello fájlból.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-176">Delete hello RoleDescriptor tags and all information in between from hello file.</span></span>
   
    <span data-ttu-id="0cc7c-177">d.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-177">d.</span></span> <span data-ttu-id="0cc7c-178">Mentse hello fájlt, és a dokumentum későbbi használatra közelben tartani.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-178">Save hello file and keep it nearby for use later in this document.</span></span>

7. <span data-ttu-id="0cc7c-179">Keresse meg a toohello Qlik logika Qlik felügyeleti konzol (QMC) hozhat létre virtuális proxybeállításokkal felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-179">Navigate toohello Qlik Sense Qlik Management Console (QMC) as a user who can create virtual proxy configurations.</span></span>

8. <span data-ttu-id="0cc7c-180">Hello QMC, kattintson a hello **virtuális proxyk** menüpont.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-180">In hello QMC, click on hello **Virtual Proxies** menu item.</span></span>
   
    ![QlikSense][qs6] 

9. <span data-ttu-id="0cc7c-182">Hello a hello képernyő aljára, kattintson a hello **hozzon létre új** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-182">At hello bottom of hello screen, click hello **Create new** button.</span></span>
   
    ![QlikSense][qs7]

10. <span data-ttu-id="0cc7c-184">hello virtuális proxy Szerkesztés képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-184">hello Virtual proxy edit screen appears.</span></span>  <span data-ttu-id="0cc7c-185">Hello hello képernyő jobb oldalán található a konfigurációs beállítások láthatóvá tétele menü.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-185">On hello right side of hello screen is a menu for making configuration options visible.</span></span>
   
    ![QlikSense][qs9]

11. <span data-ttu-id="0cc7c-187">Hello azonosító menü beállítás be van jelölve adja meg a hello Azure virtuális proxykonfigurációt hello azonosító információkat.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-187">With hello Identification menu option checked, enter hello identifying information for hello Azure virtual proxy configuration.</span></span>
    
    ![QlikSense][qs8]  
    
    <span data-ttu-id="0cc7c-189">a.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-189">a.</span></span> <span data-ttu-id="0cc7c-190">Hello **leírás** mezője hello virtuális proxykonfigurációt rövid nevét.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-190">hello **Description** field is a friendly name for hello virtual proxy configuration.</span></span>  <span data-ttu-id="0cc7c-191">Adjon meg egy értéket leírását.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-191">Enter a value for a description.</span></span>
    
    <span data-ttu-id="0cc7c-192">b.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-192">b.</span></span> <span data-ttu-id="0cc7c-193">Hello **előtag** mező azonosítja hello virtuális proxy végpont tooQlik logika csatlakozni az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-193">hello **Prefix** field identifies hello virtual proxy endpoint for connecting tooQlik Sense with Azure AD Single Sign-On.</span></span>  <span data-ttu-id="0cc7c-194">Adja meg a virtuális proxy előtag egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-194">Enter a unique prefix name for this virtual proxy.</span></span>
    
    <span data-ttu-id="0cc7c-195">c.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-195">c.</span></span> <span data-ttu-id="0cc7c-196">**Munkamenet inaktivitás időkorlátja (perc)** az hello időkorlátot a virtuális proxyn keresztül történő kapcsolatokhoz.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-196">**Session inactivity timeout (minutes)** is hello timeout for connections through this virtual proxy.</span></span>
    
    <span data-ttu-id="0cc7c-197">d.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-197">d.</span></span> <span data-ttu-id="0cc7c-198">Hello **munkamenet cookie-k fejlécnév** hello Qlik logika munkamenet a felhasználó megkapja a sikeres hitelesítés után a munkamenet-azonosítót hello hello cookie nevét tárolja.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-198">hello **Session cookie header name** is hello cookie name storing hello session identifier for hello Qlik Sense session a user receives after successful authentication.</span></span>  <span data-ttu-id="0cc7c-199">Ez a név nem egyedi.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-199">This name must be unique.</span></span>

12. <span data-ttu-id="0cc7c-200">Kattintson a hello hitelesítési menü beállítás toomake azt látható.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-200">Click on hello Authentication menu option toomake it visible.</span></span>  <span data-ttu-id="0cc7c-201">hello hitelesítési képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-201">hello Authentication screen appears.</span></span>
    
    ![QlikSense][qs10]
    
    <span data-ttu-id="0cc7c-203">a.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-203">a.</span></span> <span data-ttu-id="0cc7c-204">Hello **névtelen hozzáférési mód** legördülő lista határozza meg, ha a névtelen felhasználók férhetnek hozzá Qlik logika hello virtuális proxyn keresztül.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-204">hello **Anonymous access mode** drop down determines if anonymous users may access Qlik Sense through hello virtual proxy.</span></span>  <span data-ttu-id="0cc7c-205">hello alapértelmezett beállítás nem névtelen felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-205">hello default option is No anonymous user.</span></span>
    
    <span data-ttu-id="0cc7c-206">b.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-206">b.</span></span> <span data-ttu-id="0cc7c-207">Hello **hitelesítési módszer** legördülő lista határozza meg, hogy hello hitelesítési séma hello virtuális proxy fog használni.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-207">hello **Authentication method** drop-down determines hello authentication scheme hello virtual proxy will use.</span></span>  <span data-ttu-id="0cc7c-208">Válassza ki a SAML hello legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-208">Select SAML from hello drop-down list.</span></span>  <span data-ttu-id="0cc7c-209">További beállítások következtében jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-209">More options appear as a result.</span></span>
    
    <span data-ttu-id="0cc7c-210">c.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-210">c.</span></span> <span data-ttu-id="0cc7c-211">A hello **SAML gazdagép URI mező**, bemeneti hello állomásnév felhasználók tooaccess Qlik logika a SAML-alapú virtuális proxyn keresztül történő adja meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-211">In hello **SAML host URI field**, input hello hostname users enter tooaccess Qlik Sense through this SAML virtual proxy.</span></span>  <span data-ttu-id="0cc7c-212">hello az állomásnév megadása hello Qlik logika server hello URI-azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-212">hello hostname is hello uri of hello Qlik Sense server.</span></span>
    
    <span data-ttu-id="0cc7c-213">d.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-213">d.</span></span> <span data-ttu-id="0cc7c-214">A hello **SAML Entitásazonosító**, adja meg ugyanazt az értéket a megadott hello SAML állomás URI mező hello.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-214">In hello **SAML entity ID**, enter hello same value entered for hello SAML host URI field.</span></span>
    
    <span data-ttu-id="0cc7c-215">e.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-215">e.</span></span> <span data-ttu-id="0cc7c-216">Hello **SAML IdP metaadatok** hello a korábbi szerkeszteni hello fájl **összevonási metaadatok szerkesztése az Azure AD-konfigurációból** szakasz.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-216">hello **SAML IdP metadata** is hello file edited earlier in hello **Edit Federation Metadata from Azure AD Configuration** section.</span></span>  <span data-ttu-id="0cc7c-217">**Hello IdP metaadatok feltöltése előtt hello fájlt kell szerkeszteni toobe** tooremove információk tooensure megfelelő működéséhez az Azure AD között és Qlik logika kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-217">**Before uploading hello IdP metadata, hello file needs toobe edited** tooremove information tooensure proper operation between Azure AD and Qlik Sense server.</span></span>  <span data-ttu-id="0cc7c-218">**Ha hello fájl még toobe szerkeszteni tekintse meg a fenti toohello utasítások.**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-218">**Please refer toohello instructions above if hello file has yet toobe edited.**</span></span>  <span data-ttu-id="0cc7c-219">Ha hello fájl kattintson a hello Tallózás gombra, és válassza hello szerkesztett metaadatok fájl tooupload azt toohello virtuális proxybeállításait.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-219">If hello file has been edited click on hello Browse button and select hello edited metadata file tooupload it toohello virtual proxy configuration.</span></span>
    
    <span data-ttu-id="0cc7c-220">f.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-220">f.</span></span> <span data-ttu-id="0cc7c-221">Adja meg a hello nevét vagy séma attribútumhivatkozás hello SAML attribútum hello képviselő **UserID** az Azure AD toohello Qlik logika server küld.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-221">Enter hello attribute name or schema reference for hello SAML attribute representing hello **UserID** Azure AD sends toohello Qlik Sense server.</span></span>  <span data-ttu-id="0cc7c-222">Hello Azure alkalmazás képernyők feladás egy vagy több konfigurációs séma referencia jellegű információt érhető el.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-222">Schema reference information is available in hello Azure app screens post configuration.</span></span>  <span data-ttu-id="0cc7c-223">toouse hello névattribútuma. Adjon meg `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-223">toouse hello name attribute, enter `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`.</span></span>
    
    <span data-ttu-id="0cc7c-224">g.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-224">g.</span></span> <span data-ttu-id="0cc7c-225">Hello meg hello **felhasználói directory** tooQlik logika kiszolgálóhoz az Azure AD-n keresztül hitelesítéshez csatolt toousers fogja tárolni.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-225">Enter hello value for hello **user directory** that will be attached toousers when they authenticate tooQlik Sense server through Azure AD.</span></span>  <span data-ttu-id="0cc7c-226">Szoftveresen kötött értékeket kell lennie.%n **szögletes szögletes zárójelbe []**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-226">Hardcoded values must be surrounded by **square brackets []**.</span></span>  <span data-ttu-id="0cc7c-227">toouse attribútum küldi hello Azure AD SAML-előfeltétel, a mezőben adja meg hello attribútum neve hello **nélkül** szögletes zárójelek között.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-227">toouse an attribute sent in hello Azure AD SAML assertion, enter hello name of hello attribute in this text box **without** square brackets.</span></span>
    
    <span data-ttu-id="0cc7c-228">h.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-228">h.</span></span> <span data-ttu-id="0cc7c-229">Hello **SAML aláírási algoritmus** beállítása hello szolgáltatás szolgáltató (a kis Qlik logika kiszolgálón) tanúsítvány-aláírási hello virtuális proxy konfigurációhoz.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-229">hello **SAML signing algorithm** sets hello service provider (in this case Qlik Sense server) certificate signing for hello virtual proxy configuration.</span></span>  <span data-ttu-id="0cc7c-230">Ha Qlik logika kiszolgáló Microsoft Enhanced RSA és az AES kriptográfiai szolgáltató használatával létrehozott megbízható tanúsítványt használ, módosítsa hello SAML aláírási algoritmus túl**SHA-256**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-230">If Qlik Sense server uses a trusted certificate generated using Microsoft Enhanced RSA and AES Cryptographic Provider, change hello SAML signing algorithm too**SHA-256**.</span></span>
    
    <span data-ttu-id="0cc7c-231">i.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-231">i.</span></span> <span data-ttu-id="0cc7c-232">hello SAML attribútum leképezési szakasz lehetővé teszi a csoportok küldött toobe tooQlik logika biztonsági szabályok használható például további attribútumokat.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-232">hello SAML attribute mapping section allows for additional attributes like groups toobe sent tooQlik Sense for use in security rules.</span></span>

13. <span data-ttu-id="0cc7c-233">Kattintson a hello **TERHELÉSELOSZTÁS** menü beállítás toomake azt látható.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-233">Click on hello **LOAD BALANCING** menu option toomake it visible.</span></span>  <span data-ttu-id="0cc7c-234">hello terheléselosztás képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-234">hello Load Balancing screen appears.</span></span>
    
    ![QlikSense][qs11]

14. <span data-ttu-id="0cc7c-236">Kattintson a hello **új csomópont hozzáadása a kiszolgáló** gombra, jelölje be a vezérlő csomópont, vagy csomópontok Qlik logika munkamenetek toofor terheléselosztás céljából küld, és kattintson hello **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-236">Click on hello **Add new server node** button, select engine node or nodes Qlik Sense will send sessions toofor load balancing purposes, and click hello **Add** button.</span></span>
    
    ![QlikSense][qs12]

15. <span data-ttu-id="0cc7c-238">Kattintson a hello Speciális menüben beállítás toomake azt látható.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-238">Click on hello Advanced menu option toomake it visible.</span></span> <span data-ttu-id="0cc7c-239">hello speciális képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-239">hello Advanced screen appears.</span></span>
    
    ![QlikSense][qs13]
    
    <span data-ttu-id="0cc7c-241">hello állomás fehér lista toohello Qlik logika kiszolgálóhoz kapcsolódáskor elfogadott állomásnevek azonosítja.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-241">hello Host white list identifies hostnames that are accepted when connecting toohello Qlik Sense server.</span></span>  <span data-ttu-id="0cc7c-242">**Adjon meg felhasználókat kell megadnia a tooQlik logika kiszolgálóhoz kapcsolódáskor hello állomásnevet.**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-242">**Enter hello hostname users will specify when connecting tooQlik Sense server.**</span></span> <span data-ttu-id="0cc7c-243">hello az állomásnév megadása hello azonos érték szerint hello SAML állomás uri hello https:// nélkül.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-243">hello hostname is hello same value as hello SAML host uri without hello https://.</span></span>

16. <span data-ttu-id="0cc7c-244">Kattintson a hello **alkalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-244">Click hello **Apply** button.</span></span>
    
    ![QlikSense][qs14]

17. <span data-ttu-id="0cc7c-246">Kattintson az OK tooaccept hello figyelmeztető üzenet arról, hogy proxyk csatolt toohello virtuális proxy újraindul.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-246">Click OK tooaccept hello warning message that states proxies linked toohello virtual proxy will be restarted.</span></span>
    
    ![QlikSense][qs15]

18. <span data-ttu-id="0cc7c-248">Az üdvözlő képernyőt hello jobb oldalán hello társított elemek menü jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-248">On hello right side of hello screen, hello Associated items menu appears.</span></span>  <span data-ttu-id="0cc7c-249">Kattintson a hello **proxyk** menüjét.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-249">Click on hello **Proxies** menu option.</span></span>
    
    ![QlikSense][qs16]

19. <span data-ttu-id="0cc7c-251">hello proxy képernyő jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-251">hello proxy screen appears.</span></span>  <span data-ttu-id="0cc7c-252">Kattintson a hello **hivatkozás** gombra kell hello alsó toolink proxy toohello virtuális proxyt.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-252">Click hello **Link** button at hello bottom toolink a proxy toohello virtual proxy.</span></span>
    
    ![QlikSense][qs17]

20. <span data-ttu-id="0cc7c-254">Jelölje be hello gyorsítótárproxy-csomópont, amely támogatja a virtuális proxy kapcsolat kattintson hello **hivatkozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-254">Select hello proxy node that will support this virtual proxy connection and click hello **Link** button.</span></span>  <span data-ttu-id="0cc7c-255">Csatolása, után hello proxy a társított proxyk alatt jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-255">After linking, hello proxy will be listed under associated proxies.</span></span>
    
    ![QlikSense][qs18]
  
    ![QlikSense][qs19]

21. <span data-ttu-id="0cc7c-258">Miután körülbelül öt tooten másodperc, a hello QMC frissítése message jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-258">After about five tooten seconds, hello Refresh QMC message will appear.</span></span>  <span data-ttu-id="0cc7c-259">Kattintson a hello **frissítése QMC** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-259">Click hello **Refresh QMC** button.</span></span>
    
    ![QlikSense][qs20]

22. <span data-ttu-id="0cc7c-261">Amikor frissíti a hello QMC, kattintson a hello **virtuális proxyk** menüpont.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-261">When hello QMC refreshes, click on hello **Virtual proxies** menu item.</span></span> <span data-ttu-id="0cc7c-262">hello új SAML-alapú virtuális proxy bejegyzés az üdvözlő képernyőt hello táblázatban felsorolt.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-262">hello new SAML virtual proxy entry is listed in hello table on hello screen.</span></span>  <span data-ttu-id="0cc7c-263">Hello virtuális proxy bejegyzésre egyetlen kattintással.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-263">Single click on hello virtual proxy entry.</span></span>
    
    ![QlikSense][qs51]

23. <span data-ttu-id="0cc7c-265">Hello a hello képernyő aljára hello letöltése SP metaadatok gomb aktiválódik.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-265">At hello bottom of hello screen, hello Download SP metadata button will activate.</span></span>  <span data-ttu-id="0cc7c-266">Kattintson a hello **letöltése SP metaadatok** gomb tooa toosave hello metaadatfájl.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-266">Click hello **Download SP metadata** button toosave hello metadata tooa file.</span></span>
    
    ![QlikSense][qs52]

24. <span data-ttu-id="0cc7c-268">Nyissa meg hello sp metaadatait tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-268">Open hello sp metadata file.</span></span>  <span data-ttu-id="0cc7c-269">Tekintse át az hello **entityid beállítást** bejegyzés és hello **AssertionConsumerService** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-269">Observe hello **entityID** entry and hello **AssertionConsumerService** entry.</span></span>  <span data-ttu-id="0cc7c-270">Ezek az értékek egyenértékű toohello **azonosító** és hello **bejelentkezési URL-cím** hello Azure AD alkalmazás-konfigurációban.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-270">These values are equivalent toohello **Identifier** and hello **Sign on URL** in hello Azure AD application configuration.</span></span> <span data-ttu-id="0cc7c-271">Illessze be ezeket az értékeket a hello **Qlik logika vállalati tartomány és az URL-címek** szakasz hello Azure AD alkalmazás konfigurációban, ha azok nem egyeznek, akkor le kell cserélni őket hello Azure AD alkalmazás-konfigurációs varázsló.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-271">Paste these values in hello **Qlik Sense Enterprise Domain and URLs** section in hello Azure AD application configuration if they are not matching, then you should replace them in hello Azure AD App configuration wizard.</span></span>
    
    ![QlikSense][qs53]

> [!TIP]
> <span data-ttu-id="0cc7c-273">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="0cc7c-273">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="0cc7c-274">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-274">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="0cc7c-275">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="0cc7c-275">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="0cc7c-276">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="0cc7c-276">Create an Azure AD test user</span></span>
<span data-ttu-id="0cc7c-277">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-277">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="0cc7c-279">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-279">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cc7c-280">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-280">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

   ![hello Azure Active Directory gomb](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="0cc7c-282">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-282">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

   ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="0cc7c-284">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-284">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

   ![hello Hozzáadás gomb](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="0cc7c-286">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="0cc7c-286">In hello **User** dialog box, perform hello following steps:</span></span>

   ![hello felhasználó párbeszédpanel](./media/active-directory-saas-qliksense-enterprise-tutorial/create_aaduser_04.png)

   <span data-ttu-id="0cc7c-288">a.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-288">a.</span></span> <span data-ttu-id="0cc7c-289">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-289">In hello **Name** box, type **BrittaSimon**.</span></span>

   <span data-ttu-id="0cc7c-290">b.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-290">b.</span></span> <span data-ttu-id="0cc7c-291">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-291">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

   <span data-ttu-id="0cc7c-292">c.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-292">c.</span></span> <span data-ttu-id="0cc7c-293">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-293">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

   <span data-ttu-id="0cc7c-294">d.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-294">d.</span></span> <span data-ttu-id="0cc7c-295">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-295">Click **Create**.</span></span>
 
### <a name="create-a-qlik-sense-enterprise-test-user"></a><span data-ttu-id="0cc7c-296">Qlik logika vállalati tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="0cc7c-296">Create a Qlik Sense Enterprise test user</span></span>

<span data-ttu-id="0cc7c-297">Ebben a szakaszban Britta Simon meghívta Qlik logika vállalati felhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-297">In this section, you create a user called Britta Simon in Qlik Sense Enterprise.</span></span> <span data-ttu-id="0cc7c-298">Együttműködve [Qlik logika vállalati ügyfél-támogatási csoport](https://www.qlik.com/us/services/support) felhasználót is hozzáadhat hello hello Qlik logika vállalati platform.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-298">Work with [Qlik Sense Enterprise Client support team](https://www.qlik.com/us/services/support) to add hello users in hello Qlik Sense Enterprise platform.</span></span> <span data-ttu-id="0cc7c-299">Felhasználók kell létrehoznia és aktiválni az egyszeri bejelentkezés használata előtt.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-299">Users must be created and activated before you use single sign-on.</span></span> 

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="0cc7c-300">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="0cc7c-300">Assign hello Azure AD test user</span></span>

<span data-ttu-id="0cc7c-301">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooQlik logika vállalati megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-301">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooQlik Sense Enterprise.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="0cc7c-303">**tooassign Britta Simon tooQlik logika vállalati, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="0cc7c-303">**tooassign Britta Simon tooQlik Sense Enterprise, perform hello following steps:**</span></span>

1. <span data-ttu-id="0cc7c-304">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-304">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="0cc7c-306">Hello alkalmazások listában válassza ki a **Qlik logika vállalati**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-306">In hello applications list, select **Qlik Sense Enterprise**.</span></span>

    ![hello Qlik logika vállalati hivatkozásra hello alkalmazások listája](./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksense-enterprise_app.png)  

3. <span data-ttu-id="0cc7c-308">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-308">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="0cc7c-310">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-310">Click **Add** button.</span></span> <span data-ttu-id="0cc7c-311">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-311">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="0cc7c-313">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-313">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="0cc7c-314">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-314">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="0cc7c-315">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-315">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="0cc7c-316">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="0cc7c-316">Test single sign-on</span></span>

<span data-ttu-id="0cc7c-317">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-317">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="0cc7c-318">Ha a hozzáférési Panel hello hello Qlik logika vállalati csempe gombra kattint, automatikusan bejelentkezett tooyour Qlik logika vállalati alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="0cc7c-318">When you click hello Qlik Sense Enterprise tile in hello Access Panel, you should get automatically signed-on tooyour Qlik Sense Enterprise application.</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="0cc7c-319">További források</span><span class="sxs-lookup"><span data-stu-id="0cc7c-319">Additional resources</span></span>

* [<span data-ttu-id="0cc7c-320">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="0cc7c-320">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="0cc7c-321">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="0cc7c-321">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_general_203.png

[qs6]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_06.png
[qs7]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_07.png
[qs8]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_08.png
[qs9]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_09.png
[qs10]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_10.png
[qs11]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_11.png
[qs12]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_12.png
[qs13]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_13.png
[qs14]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_14.png
[qs15]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_15.png
[qs16]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_16.png
[qs17]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_17.png
[qs18]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_18.png
[qs19]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_19.png
[qs20]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_20.png
[qs24]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_24.png
[qs51]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_51.png
[qs52]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_52.png
[qs53]: ./media/active-directory-saas-qliksense-enterprise-tutorial/tutorial_qliksenseenterprise_53.png

