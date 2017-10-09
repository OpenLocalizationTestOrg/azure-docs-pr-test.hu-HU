---
title: "Oktatóanyag: Azure Active Directoryval integrált SAP Business ByDesign |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP Business ByDesign között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 82938920-33ba-47cb-b141-511b46d19e66
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: jeedes
ms.openlocfilehash: c14714fd27f8d7fc555f25c7be83fad2b0d7f333
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-bydesign"></a><span data-ttu-id="020b4-103">Oktatóanyag: Azure Active Directory-integráció az SAP Business ByDesign megoldással</span><span class="sxs-lookup"><span data-stu-id="020b4-103">Tutorial: Azure Active Directory integration with SAP Business ByDesign</span></span>

<span data-ttu-id="020b4-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP Business ByDesign az Azure Active Directoryval (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="020b4-104">In this tutorial, you learn how toointegrate SAP Business ByDesign with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="020b4-105">SAP Business ByDesign integrálása az Azure AD lehetővé teszi a következő előnyöket hello:</span><span class="sxs-lookup"><span data-stu-id="020b4-105">Integrating SAP Business ByDesign with Azure AD provides you with hello following benefits:</span></span>

- <span data-ttu-id="020b4-106">Az Azure AD hozzáférési tooSAP üzleti ByDesign rendelkező szabályozhatja.</span><span class="sxs-lookup"><span data-stu-id="020b4-106">You can control in Azure AD who has access tooSAP Business ByDesign.</span></span>
- <span data-ttu-id="020b4-107">Az Azure AD-fiókok a engedélyezheti a felhasználók tooautomatically get bejelentkezett tooSAP üzleti ByDesign (egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="020b4-107">You can enable your users tooautomatically get signed-on tooSAP Business ByDesign (Single Sign-On) with their Azure AD accounts.</span></span>
- <span data-ttu-id="020b4-108">A fiók egyetlen központi helyen - hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="020b4-108">You can manage your accounts in one central location - hello Azure portal.</span></span>

<span data-ttu-id="020b4-109">Ha azt szeretné, tooknow az Azure AD SaaS integrálásáról további információkat, lásd: [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="020b4-109">If you want tooknow more details about SaaS app integration with Azure AD, see [what is application access and single sign-on with Azure Active Directory](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="020b4-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="020b4-110">Prerequisites</span></span>

<span data-ttu-id="020b4-111">az Azure AD integrálása SAP Business ByDesign tooconfigure, kell a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="020b4-111">tooconfigure Azure AD integration with SAP Business ByDesign, you need hello following items:</span></span>

- <span data-ttu-id="020b4-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="020b4-112">An Azure AD subscription</span></span>
- <span data-ttu-id="020b4-113">Az SAP Business ByDesign egyszeri bejelentkezés engedélyezve van az előfizetés</span><span class="sxs-lookup"><span data-stu-id="020b4-113">An SAP Business ByDesign single sign-on enabled subscription</span></span>

> [!NOTE]
> <span data-ttu-id="020b4-114">tootest hello lépéseit az oktatóanyag, ne használja éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="020b4-114">tootest hello steps in this tutorial, we do not recommend using a production environment.</span></span>

<span data-ttu-id="020b4-115">Ebben az oktatóanyagban tootest hello lépéseiért ajánlott ezen javaslatok:</span><span class="sxs-lookup"><span data-stu-id="020b4-115">tootest hello steps in this tutorial, you should follow these recommendations:</span></span>

- <span data-ttu-id="020b4-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="020b4-116">Do not use your production environment, unless it is necessary.</span></span>
- <span data-ttu-id="020b4-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="020b4-117">If you don't have an Azure AD trial environment, you can [get a one-month trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="020b4-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="020b4-118">Scenario description</span></span>
<span data-ttu-id="020b4-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="020b4-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> <span data-ttu-id="020b4-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="020b4-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="020b4-121">Hozzáadása az SAP Business ByDesign hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="020b4-121">Adding SAP Business ByDesign from hello gallery</span></span>
2. <span data-ttu-id="020b4-122">És tesztelés az Azure AD konfigurálása egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="020b4-122">Configuring and testing Azure AD single sign-on</span></span>

## <a name="adding-sap-business-bydesign-from-hello-gallery"></a><span data-ttu-id="020b4-123">Hozzáadása az SAP Business ByDesign hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="020b4-123">Adding SAP Business ByDesign from hello gallery</span></span>
<span data-ttu-id="020b4-124">tooconfigure hello integrációja SAP Business ByDesign az Azure AD-be, meg kell tooadd SAP Business ByDesign hello gyűjtemény tooyour felügyelt SaaS-alkalmazások listája.</span><span class="sxs-lookup"><span data-stu-id="020b4-124">tooconfigure hello integration of SAP Business ByDesign into Azure AD, you need tooadd SAP Business ByDesign from hello gallery tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="020b4-125">**tooadd SAP Business ByDesign hello gyűjteményből, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="020b4-125">**tooadd SAP Business ByDesign from hello gallery, perform hello following steps:**</span></span>

1. <span data-ttu-id="020b4-126">A hello  **[Azure-portálon](https://portal.azure.com)**, a hello bal oldali navigációs panelen, kattintson a **Azure Active Directory** ikonra.</span><span class="sxs-lookup"><span data-stu-id="020b4-126">In hello **[Azure portal](https://portal.azure.com)**, on hello left navigation panel, click **Azure Active Directory** icon.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="020b4-128">Keresse meg a túl**vállalati alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="020b4-128">Navigate too**Enterprise applications**.</span></span> <span data-ttu-id="020b4-129">Keresse meg a túl**összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="020b4-129">Then go too**All applications**.</span></span>

    ![hello vállalati alkalmazások panel][2]
    
3. <span data-ttu-id="020b4-131">Új alkalmazás tooadd, kattintson a **új alkalmazás** párbeszédpanel tetején hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="020b4-131">tooadd new application, click **New application** button on hello top of dialog.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="020b4-133">Hello keresési mezőbe, írja be a **SAP Business ByDesign**, jelölje be **SAP Business ByDesign** eredmény panelen kattintson a **Hozzáadás** tooadd hello alkalmazás gombra.</span><span class="sxs-lookup"><span data-stu-id="020b4-133">In hello search box, type **SAP Business ByDesign**, select **SAP Business ByDesign** from result panel then click **Add** button tooadd hello application.</span></span>

    ![SAP Business ByDesign hello eredmények listájában](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="020b4-135">Az Azure AD az egyszeri bejelentkezés tesztelése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="020b4-135">Configure and test Azure AD single sign-on</span></span>

<span data-ttu-id="020b4-136">Ebben a szakaszban, konfigurálás és tesztelés az Azure AD az egyszeri bejelentkezés SAP Business ByDesign "Britta Simon" nevű tesztfelhasználó alapján.</span><span class="sxs-lookup"><span data-stu-id="020b4-136">In this section, you configure and test Azure AD single sign-on with SAP Business ByDesign based on a test user called "Britta Simon".</span></span>

<span data-ttu-id="020b4-137">Az egyszeri bejelentkezés toowork az Azure AD kell tooknow milyen hello tartozó felhasználói az SAP Business ByDesign tooa felhasználói az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="020b4-137">For single sign-on toowork, Azure AD needs tooknow what hello counterpart user in SAP Business ByDesign is tooa user in Azure AD.</span></span> <span data-ttu-id="020b4-138">Ez azt jelenti az Azure AD-felhasználó és a kapcsolódó felhasználói hello az SAP Business ByDesign közötti kapcsolat kapcsolatot kell toobe létrejött.</span><span class="sxs-lookup"><span data-stu-id="020b4-138">In other words, a link relationship between an Azure AD user and hello related user in SAP Business ByDesign needs toobe established.</span></span>

<span data-ttu-id="020b4-139">Az SAP Business ByDesign, rendelje a hello hello értékét **felhasználónév** hello értékeként hello Azure AD-ben **felhasználónév** tooestablish hello hivatkozás kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="020b4-139">In SAP Business ByDesign, assign hello value of hello **user name** in Azure AD as hello value of hello **Username** tooestablish hello link relationship.</span></span>

<span data-ttu-id="020b4-140">tooconfigure és az SAP Business ByDesign az Azure AD az egyszeri bejelentkezés tesztelése, a következő építőelemeket toocomplete hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="020b4-140">tooconfigure and test Azure AD single sign-on with SAP Business ByDesign, you need toocomplete hello following building blocks:</span></span>

1. <span data-ttu-id="020b4-141">**[Az Azure AD az egyszeri bejelentkezés konfigurálása](#configure-azure-ad-single-sign-on)**  -tooenable a felhasználók toouse ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="020b4-141">**[Configure Azure AD Single Sign-On](#configure-azure-ad-single-sign-on)** - tooenable your users toouse this feature.</span></span>
2. <span data-ttu-id="020b4-142">**[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user)**  -tootest az Azure AD egyszeri bejelentkezést a Britta Simon.</span><span class="sxs-lookup"><span data-stu-id="020b4-142">**[Create an Azure AD test user](#create-an-azure-ad-test-user)** - tootest Azure AD single sign-on with Britta Simon.</span></span>
3. <span data-ttu-id="020b4-143">**[Hozzon létre egy SAP Business ByDesign tesztfelhasználó](#create-an-sap-business-bydesign-test-user)**  -toohave egy megfelelője a Britta Simon az SAP Business ByDesign, amely a felhasználó csatolt toohello az Azure AD ábrázolása.</span><span class="sxs-lookup"><span data-stu-id="020b4-143">**[Create an SAP Business ByDesign test user](#create-an-sap-business-bydesign-test-user)** - toohave a counterpart of Britta Simon in SAP Business ByDesign that is linked toohello Azure AD representation of user.</span></span>
4. <span data-ttu-id="020b4-144">**[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user)**  -tooenable Britta Simon toouse az Azure AD egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="020b4-144">**[Assign hello Azure AD test user](#assign-the-azure-ad-test-user)** - tooenable Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="020b4-145">**[Egyszeri bejelentkezés tesztelése](#test-single-sign-on)**  -tooverify e hello konfigurációs működik.</span><span class="sxs-lookup"><span data-stu-id="020b4-145">**[Test single sign-on](#test-single-sign-on)** - tooverify whether hello configuration works.</span></span>

### <a name="configure-azure-ad-single-sign-on"></a><span data-ttu-id="020b4-146">Az Azure AD az egyszeri bejelentkezés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="020b4-146">Configure Azure AD single sign-on</span></span>

<span data-ttu-id="020b4-147">Ebben a szakaszban az Azure AD az egyszeri bejelentkezés az Azure-portálon hello engedélyezése, és az SAP Business ByDesign alkalmazásban egyszeri bejelentkezés beállítása.</span><span class="sxs-lookup"><span data-stu-id="020b4-147">In this section, you enable Azure AD single sign-on in hello Azure portal and configure single sign-on in your SAP Business ByDesign application.</span></span>

<span data-ttu-id="020b4-148">**az SAP Business ByDesign, az Azure AD az egyszeri bejelentkezés tooconfigure hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="020b4-148">**tooconfigure Azure AD single sign-on with SAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="020b4-149">Az Azure portál, a hello hello **SAP Business ByDesign** alkalmazás integráció lapján, kattintson a **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="020b4-149">In hello Azure portal, on hello **SAP Business ByDesign** application integration page, click **Single sign-on**.</span></span>

    ![Egyszeri bejelentkezés kapcsolat konfigurálása][4]

2. <span data-ttu-id="020b4-151">A hello **egyszeri bejelentkezés** párbeszédablakban válassza **mód** , **SAML-alapú bejelentkezés** tooenable egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="020b4-151">On hello **Single sign-on** dialog, select **Mode** as   **SAML-based Sign-on** tooenable single sign-on.</span></span>
 
    ![Egyszeri bejelentkezés párbeszédpanel](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_samlbase.png)

3. <span data-ttu-id="020b4-153">A hello **SAP Business ByDesign tartomány és az URL-címek** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="020b4-153">On hello **SAP Business ByDesign Domain and URLs** section, perform hello following steps:</span></span>

    ![Az egyszeri bejelentkezés információk SAP Business ByDesign tartomány és az URL-címek](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_url.png)

    <span data-ttu-id="020b4-155">a.</span><span class="sxs-lookup"><span data-stu-id="020b4-155">a.</span></span> <span data-ttu-id="020b4-156">A hello **bejelentkezési URL-cím** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="020b4-156">In hello **Sign-on URL** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    <span data-ttu-id="020b4-157">b.</span><span class="sxs-lookup"><span data-stu-id="020b4-157">b.</span></span> <span data-ttu-id="020b4-158">A hello **azonosító** szövegmezőhöz URL-címet a következő mintát hello használatával írja be:`https://<servername>.sapbydesign.com`</span><span class="sxs-lookup"><span data-stu-id="020b4-158">In hello **Identifier** textbox, type a URL using hello following pattern: `https://<servername>.sapbydesign.com`</span></span>

    > [!NOTE] 
    > <span data-ttu-id="020b4-159">Ezek az értékek nincsenek valós.</span><span class="sxs-lookup"><span data-stu-id="020b4-159">These values are not real.</span></span> <span data-ttu-id="020b4-160">Frissítse a bejelentkezési URL-cím és azonosító a hello tényleges értékek.</span><span class="sxs-lookup"><span data-stu-id="020b4-160">Update these values with hello actual Sign-On URL and Identifier.</span></span> <span data-ttu-id="020b4-161">Ügyfél [SAP Business ByDesign ügyfél-támogatási csoport](https://www.sap.com/products/cloud-analytics.support.html) tooget ezeket az értékeket.</span><span class="sxs-lookup"><span data-stu-id="020b4-161">Contact [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooget these values.</span></span>

4. <span data-ttu-id="020b4-162">A hello **felhasználói attribútumok** csoportjában hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="020b4-162">On hello **User Attributes** section, perform hello following steps:</span></span>

    ![SAP Business ByDesign attribútum szakasz](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_attribute.png)
    
    <span data-ttu-id="020b4-164">a.</span><span class="sxs-lookup"><span data-stu-id="020b4-164">a.</span></span> <span data-ttu-id="020b4-165">A **felhasználói azonosító** listában, jelölje be hello **ExtractMailPrefix()** függvény.</span><span class="sxs-lookup"><span data-stu-id="020b4-165">In **User Identifier** list, select hello **ExtractMailPrefix()** function.</span></span>
    
    <span data-ttu-id="020b4-166">b.</span><span class="sxs-lookup"><span data-stu-id="020b4-166">b.</span></span> <span data-ttu-id="020b4-167">A hello **Mail** listán, válassza hello felhasználói attribútum a megvalósítás toouse használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="020b4-167">From hello **Mail** list, select hello user attribute you want toouse for your implementation.</span></span> <span data-ttu-id="020b4-168">Például ha azt szeretné, hogy az egyedi felhasználói azonosítóval EmployeeID toouse hello és hello ExtensionAttribute2 tárolt hello attribútum értéke, majd válassza ki user.extensionattribute2.</span><span class="sxs-lookup"><span data-stu-id="020b4-168">For example, if you want toouse hello EmployeeID as unique user identifier and you have stored hello attribute value in hello ExtensionAttribute2, then select user.extensionattribute2.</span></span>   

5. <span data-ttu-id="020b4-169">A hello **SAML-aláíró tanúsítványa** kattintson **metaadatainak XML-kódja** , és mentse a hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="020b4-169">On hello **SAML Signing Certificate** section, click **Metadata XML** and then save hello metadata file on your computer.</span></span>

    ![hello tanúsítvány letöltési hivatkozását](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_certificate.png) 

6. <span data-ttu-id="020b4-171">Kattintson a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="020b4-171">Click **Save** button.</span></span>

    ![Egyszeri bejelentkezés Mentés gombra konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_400.png)

7. <span data-ttu-id="020b4-173">A hello **SAP Business ByDesign konfigurációs** kattintson **SAP Business ByDesign konfigurálása** tooopen **bejelentkezés konfigurálása** ablak.</span><span class="sxs-lookup"><span data-stu-id="020b4-173">On hello **SAP Business ByDesign Configuration** section, click **Configure SAP Business ByDesign** tooopen **Configure sign-on** window.</span></span> <span data-ttu-id="020b4-174">Másolás hello **SAML-alapú egyszeri bejelentkezési URL-címe** a hello **rövid összefoglaló szakasz.**</span><span class="sxs-lookup"><span data-stu-id="020b4-174">Copy hello **SAML Single Sign-On Service URL** from hello **Quick Reference section.**</span></span>

    ![SAP Business ByDesign konfiguráció](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_configure.png) 

8. <span data-ttu-id="020b4-176">az alkalmazáshoz konfigurált SSO tooget hello a következő lépéseket hajtsa végre:</span><span class="sxs-lookup"><span data-stu-id="020b4-176">tooget SSO configured for your application, perform hello following steps:</span></span>
   
    <span data-ttu-id="020b4-177">a.</span><span class="sxs-lookup"><span data-stu-id="020b4-177">a.</span></span> <span data-ttu-id="020b4-178">Bejelentkezés tooyour SAP Business ByDesign portál, rendszergazdai jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="020b4-178">Sign on tooyour SAP Business ByDesign portal with administrator rights.</span></span>
   
    <span data-ttu-id="020b4-179">b.</span><span class="sxs-lookup"><span data-stu-id="020b4-179">b.</span></span> <span data-ttu-id="020b4-180">Keresse meg a túl**alkalmazás- és felhasználói gyakori feladatot** hello kattintson **identitásszolgáltató** fülre.</span><span class="sxs-lookup"><span data-stu-id="020b4-180">Navigate too**Application and User Management Common Task** and click hello **Identity Provider** tab.</span></span>
   
    <span data-ttu-id="020b4-181">c.</span><span class="sxs-lookup"><span data-stu-id="020b4-181">c.</span></span> <span data-ttu-id="020b4-182">Kattintson a **új identitásszolgáltató** és select hello metaadatok XML-fájl az Azure-portálon hello letöltött.</span><span class="sxs-lookup"><span data-stu-id="020b4-182">Click **New Identity Provider** and select hello metadata XML file that you have downloaded from hello Azure portal.</span></span> <span data-ttu-id="020b4-183">Hello metaadatok importálásával hello rendszer automatikusan feltölti a hello szükséges aláírási tanúsítvány és a titkosítási tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="020b4-183">By importing hello metadata, hello system automatically uploads hello required signature certificate and encryption certificate.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_54.png)
   
    <span data-ttu-id="020b4-185">d.</span><span class="sxs-lookup"><span data-stu-id="020b4-185">d.</span></span> <span data-ttu-id="020b4-186">tooinclude hello **helyességi feltétel ügyfél szolgáltatás URL-címe** történő hello SAML-kérelmet, válassza ki a **helyességi feltétel fogyasztói szolgáltatás URL-címek**.</span><span class="sxs-lookup"><span data-stu-id="020b4-186">tooinclude hello **Assertion Consumer Service URL** into hello SAML request, select **Include Assertion Consumer Service URL**.</span></span>
   
    <span data-ttu-id="020b4-187">e.</span><span class="sxs-lookup"><span data-stu-id="020b4-187">e.</span></span> <span data-ttu-id="020b4-188">Kattintson a **egyszeri bejelentkezés aktiválása**.</span><span class="sxs-lookup"><span data-stu-id="020b4-188">Click **Activate Single Sign-On**.</span></span>
   
    <span data-ttu-id="020b4-189">f.</span><span class="sxs-lookup"><span data-stu-id="020b4-189">f.</span></span> <span data-ttu-id="020b4-190">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="020b4-190">Save your changes.</span></span>
   
    <span data-ttu-id="020b4-191">g.</span><span class="sxs-lookup"><span data-stu-id="020b4-191">g.</span></span> <span data-ttu-id="020b4-192">Kattintson a hello **a rendszer** fülre.</span><span class="sxs-lookup"><span data-stu-id="020b4-192">Click hello **My System** tab.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_52.png)
   
    <span data-ttu-id="020b4-194">h.</span><span class="sxs-lookup"><span data-stu-id="020b4-194">h.</span></span> <span data-ttu-id="020b4-195">Beillesztés **SAML-alapú egyszeri bejelentkezési URL-címe**, amelyek akkor másolta, a hello Azure-portálon azt hello **Azure AD bejelentkezési URL-címen** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="020b4-195">Paste **SAML Single Sign-On Service URL**, which you have copied from hello Azure portal it into hello **Azure AD Sign On URL** textbox.</span></span>
   
    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_53.png)
   
    <span data-ttu-id="020b4-197">i.</span><span class="sxs-lookup"><span data-stu-id="020b4-197">i.</span></span> <span data-ttu-id="020b4-198">Adja meg, hogy hello alkalmazott manuálisan választhat naplózás a felhasználói Azonosítót és jelszót vagy az SSO kiválasztásával **manuális Identity Provider kijelölés**.</span><span class="sxs-lookup"><span data-stu-id="020b4-198">Specify whether hello employee can manually choose between logging on with user ID and password or SSO by selecting **Manual Identity Provider Selection**.</span></span>
   
    <span data-ttu-id="020b4-199">j.</span><span class="sxs-lookup"><span data-stu-id="020b4-199">j.</span></span> <span data-ttu-id="020b4-200">A hello **egyszeri bejelentkezési URL-cím** szakasz hello alkalmazott toologon toohello rendszer által használandó hello URL-címét.</span><span class="sxs-lookup"><span data-stu-id="020b4-200">In hello **SSO URL** section, specify hello URL that should be used by hello employee toologon toohello system.</span></span> 
    <span data-ttu-id="020b4-201">Hello URL-cím küldött tooEmployee legördülő lista a következő beállítások hello választhat:</span><span class="sxs-lookup"><span data-stu-id="020b4-201">In hello URL Sent tooEmployee dropdown list, you can choose between hello following options:</span></span>
   
    <span data-ttu-id="020b4-202">**Nem egyszeri bejelentkezési URL-címe**</span><span class="sxs-lookup"><span data-stu-id="020b4-202">**Non-SSO URL**</span></span>
   
    <span data-ttu-id="020b4-203">hello csak a hello normál rendszer URL-cím toohello alkalmazott küldi.</span><span class="sxs-lookup"><span data-stu-id="020b4-203">hello system sends only hello normal system URL toohello employee.</span></span> <span data-ttu-id="020b4-204">hello alkalmazott nem jelentkezik be egyszeri Bejelentkezést, és kell jelszó használata vagy a tanúsítvány helyette.</span><span class="sxs-lookup"><span data-stu-id="020b4-204">hello employee cannot log on using SSO, and must use password or certificate instead.</span></span>
   
    <span data-ttu-id="020b4-205">**EGYSZERI BEJELENTKEZÉSI URL-CÍME**</span><span class="sxs-lookup"><span data-stu-id="020b4-205">**SSO URL**</span></span> 
   
    <span data-ttu-id="020b4-206">hello csak hello egyszeri bejelentkezési URL-cím toohello alkalmazott küldi.</span><span class="sxs-lookup"><span data-stu-id="020b4-206">hello system sends only hello SSO URL toohello employee.</span></span> <span data-ttu-id="020b4-207">hello alkalmazott Egyszeri bejelentkezéshez használhatja.</span><span class="sxs-lookup"><span data-stu-id="020b4-207">hello employee can log on using SSO.</span></span> <span data-ttu-id="020b4-208">Hitelesítési kérelem hello IdP van irányítva.</span><span class="sxs-lookup"><span data-stu-id="020b4-208">Authentication request is redirected through hello IdP.</span></span>
   
    <span data-ttu-id="020b4-209">**Automatikus kiválasztásához.**</span><span class="sxs-lookup"><span data-stu-id="020b4-209">**Automatic Selection**</span></span>
   
    <span data-ttu-id="020b4-210">Ha az SSO nem aktív, hello küldi hello normál rendszer URL-cím toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="020b4-210">If SSO is not active, hello system sends hello normal system URL toohello employee.</span></span> <span data-ttu-id="020b4-211">Ha egyszeri bejelentkezés aktív, a hello rendszer ellenőrzi, hogy hello alkalmazott jelszóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="020b4-211">If SSO is active, hello system checks whether hello employee has a password.</span></span> <span data-ttu-id="020b4-212">A jelszó nem érhető el, ha mind SSO és URL-címe nem-SSO küldött toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="020b4-212">If a password is available, both SSO URL and Non-SSO URL are sent toohello employee.</span></span> <span data-ttu-id="020b4-213">Azonban ha hello alkalmazott nincs jelszava, csak hello egyszeri bejelentkezési URL-cím küldött toohello alkalmazott.</span><span class="sxs-lookup"><span data-stu-id="020b4-213">However, if hello employee has no password, only hello SSO URL is sent toohello employee.</span></span>
   
    <span data-ttu-id="020b4-214">k.</span><span class="sxs-lookup"><span data-stu-id="020b4-214">k.</span></span> <span data-ttu-id="020b4-215">Mentse a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="020b4-215">Save your changes.</span></span>

> [!TIP]
> <span data-ttu-id="020b4-216">Ezek az utasítások belül hello tömör verziója most olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás hello!</span><span class="sxs-lookup"><span data-stu-id="020b4-216">You can now read a concise version of these instructions inside hello [Azure portal](https://portal.azure.com), while you are setting up hello app!</span></span>  <span data-ttu-id="020b4-217">Ezt az alkalmazást a hello hozzáadása után **Active Directory > Vállalati alkalmazások** egyszerűen kattintson a hello **egyszeri bejelentkezés** lapra, és hozzáférést hello beágyazott keresztül hello dokumentáció  **Konfigurációs** szakasz hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="020b4-217">After adding this app from hello **Active Directory > Enterprise Applications** section, simply click hello **Single Sign-On** tab and access hello embedded documentation through hello **Configuration** section at hello bottom.</span></span> <span data-ttu-id="020b4-218">További szolgáltatásról hello embedded dokumentációjából itt: [az Azure AD beágyazott dokumentáció]( https://go.microsoft.com/fwlink/?linkid=845985)</span><span class="sxs-lookup"><span data-stu-id="020b4-218">You can read more about hello embedded documentation feature here: [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985)</span></span>
> 

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="020b4-219">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="020b4-219">Create an Azure AD test user</span></span>

<span data-ttu-id="020b4-220">hello ebben a szakaszban célja toocreate hello Britta Simon nevű Azure-portálon a tesztfelhasználó.</span><span class="sxs-lookup"><span data-stu-id="020b4-220">hello objective of this section is toocreate a test user in hello Azure portal called Britta Simon.</span></span>

   ![Hozzon létre egy Azure AD-teszt felhasználó][100]

<span data-ttu-id="020b4-222">**az Azure AD-tesztfelhasználó toocreate hello a következő lépéseket hajtsa végre:**</span><span class="sxs-lookup"><span data-stu-id="020b4-222">**toocreate a test user in Azure AD, perform hello following steps:**</span></span>

1. <span data-ttu-id="020b4-223">A hello Azure-portálon, hello bal oldali ablaktáblában kattintson a hello **Azure Active Directory** gombra.</span><span class="sxs-lookup"><span data-stu-id="020b4-223">In hello Azure portal, in hello left pane, click hello **Azure Active Directory** button.</span></span>

    ![hello Azure Active Directory gomb](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_01.png)

2. <span data-ttu-id="020b4-225">toodisplay hello azoknak a felhasználóknak, nyissa meg túl**felhasználók és csoportok**, és kattintson a **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="020b4-225">toodisplay hello list of users, go too**Users and groups**, and then click **All users**.</span></span>

    ![hello "Felhasználók és csoportok" és "Minden felhasználó" hivatkozások](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_02.png)

3. <span data-ttu-id="020b4-227">tooopen hello **felhasználói** párbeszédpanel, kattintson a **Hozzáadás** hello hello tetején **minden felhasználó** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="020b4-227">tooopen hello **User** dialog box, click **Add** at hello top of hello **All Users** dialog box.</span></span>

    ![hello Hozzáadás gomb](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_03.png)

4. <span data-ttu-id="020b4-229">A hello **felhasználói** párbeszédpanelen hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="020b4-229">In hello **User** dialog box, perform hello following steps:</span></span>

    ![hello felhasználó párbeszédpanel](./media/active-directory-saas-sapbusinessbydesign-tutorial/create_aaduser_04.png)

    <span data-ttu-id="020b4-231">a.</span><span class="sxs-lookup"><span data-stu-id="020b4-231">a.</span></span> <span data-ttu-id="020b4-232">A hello **neve** mezőbe írja be **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="020b4-232">In hello **Name** box, type **BrittaSimon**.</span></span>

    <span data-ttu-id="020b4-233">b.</span><span class="sxs-lookup"><span data-stu-id="020b4-233">b.</span></span> <span data-ttu-id="020b4-234">A hello **felhasználónév** mezőben, a felhasználó Britta Simon típus hello e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="020b4-234">In hello **User name** box, type hello email address of user Britta Simon.</span></span>

    <span data-ttu-id="020b4-235">c.</span><span class="sxs-lookup"><span data-stu-id="020b4-235">c.</span></span> <span data-ttu-id="020b4-236">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="020b4-236">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    <span data-ttu-id="020b4-237">d.</span><span class="sxs-lookup"><span data-stu-id="020b4-237">d.</span></span> <span data-ttu-id="020b4-238">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="020b4-238">Click **Create**.</span></span>
 
### <a name="create-an-sap-business-bydesign-test-user"></a><span data-ttu-id="020b4-239">Az SAP Business ByDesign tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="020b4-239">Create an SAP Business ByDesign test user</span></span>

<span data-ttu-id="020b4-240">Ebben a szakaszban egy SAP Business ByDesign Britta Simon nevű felhasználót hoz létre.</span><span class="sxs-lookup"><span data-stu-id="020b4-240">In this section, you create a user called Britta Simon in SAP Business ByDesign.</span></span> <span data-ttu-id="020b4-241">Adjon együttműködve [SAP Business ByDesign ügyfél-támogatási csoport](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello felhasználók hello SAP Business ByDesign platform.</span><span class="sxs-lookup"><span data-stu-id="020b4-241">Please work with [SAP Business ByDesign Client support team](https://www.sap.com/products/cloud-analytics.support.html) tooadd hello users in hello SAP Business ByDesign platform.</span></span> 

> [!NOTE]
> <span data-ttu-id="020b4-242">Győződjön meg arról, hogy NameID érték hello SAP Business ByDesign platform hello felhasználónév mezője az egyezniük kell.</span><span class="sxs-lookup"><span data-stu-id="020b4-242">Please make sure that NameID value should match with hello username field in hello SAP Business ByDesign platform.</span></span>

### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="020b4-243">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="020b4-243">Assign hello Azure AD test user</span></span>

<span data-ttu-id="020b4-244">Ebben a szakaszban a Britta Simon toouse Azure egyszeri bejelentkezés hozzáférés tooSAP üzleti ByDesign megadásával engedélyeznie.</span><span class="sxs-lookup"><span data-stu-id="020b4-244">In this section, you enable Britta Simon toouse Azure single sign-on by granting access tooSAP Business ByDesign.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 

<span data-ttu-id="020b4-246">**tooassign Britta Simon tooSAP üzleti ByDesign, hajtsa végre a lépéseket követve hello:**</span><span class="sxs-lookup"><span data-stu-id="020b4-246">**tooassign Britta Simon tooSAP Business ByDesign, perform hello following steps:**</span></span>

1. <span data-ttu-id="020b4-247">A hello Azure-portálon, nyissa meg hello alkalmazások megtekintése, és majd toohello könyvtár nézetben keresse meg és nyissa meg túl**vállalati alkalmazások** kattintson **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="020b4-247">In hello Azure portal, open hello applications view, and then navigate toohello directory view and go too**Enterprise applications** then click **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="020b4-249">Hello alkalmazások listában válassza ki a **SAP Business ByDesign**.</span><span class="sxs-lookup"><span data-stu-id="020b4-249">In hello applications list, select **SAP Business ByDesign**.</span></span>

    ![hello SAP Business ByDesign hivatkozásra hello alkalmazások listája](./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_sapbusinessbydesign_app.png)  

3. <span data-ttu-id="020b4-251">Hello hello bal oldali menüben kattintson a **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="020b4-251">In hello menu on hello left, click **Users and groups**.</span></span>

    ![hello "Felhasználók és csoportok" hivatkozásra.][202]

4. <span data-ttu-id="020b4-253">Kattintson a **Hozzáadás** gombra.</span><span class="sxs-lookup"><span data-stu-id="020b4-253">Click **Add** button.</span></span> <span data-ttu-id="020b4-254">Válassza ki **felhasználók és csoportok** a **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="020b4-254">Then select **Users and groups** on **Add Assignment** dialog.</span></span>

    ![hello hozzárendelés hozzáadása panelen][203]

5. <span data-ttu-id="020b4-256">A **felhasználók és csoportok** párbeszédablakban válassza **Britta Simon** hello felhasználók listában.</span><span class="sxs-lookup"><span data-stu-id="020b4-256">On **Users and groups** dialog, select **Britta Simon** in hello Users list.</span></span>

6. <span data-ttu-id="020b4-257">Kattintson a **válasszon** gombra **felhasználók és csoportok** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="020b4-257">Click **Select** button on **Users and groups** dialog.</span></span>

7. <span data-ttu-id="020b4-258">Kattintson a **hozzárendelése** gombra **hozzáadása hozzárendelés** párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="020b4-258">Click **Assign** button on **Add Assignment** dialog.</span></span>
    
### <a name="test-single-sign-on"></a><span data-ttu-id="020b4-259">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="020b4-259">Test single sign-on</span></span>

<span data-ttu-id="020b4-260">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési Panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="020b4-260">In this section, you test your Azure AD single sign-on configuration using hello Access Panel.</span></span>

<span data-ttu-id="020b4-261">Ha a hozzáférési Panel hello hello SAP Business ByDesign csempe gombra kattint, automatikusan bejelentkezett tooyour SAP Business ByDesign alkalmazás szerezheti be.</span><span class="sxs-lookup"><span data-stu-id="020b4-261">When you click hello SAP Business ByDesign tile in hello Access Panel, you should get automatically signed-on tooyour SAP Business ByDesign application.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="020b4-262">További források</span><span class="sxs-lookup"><span data-stu-id="020b4-262">Additional resources</span></span>

* [<span data-ttu-id="020b4-263">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="020b4-263">List of Tutorials on How tooIntegrate SaaS Apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="020b4-264">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="020b4-264">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapbusinessbydesign-tutorial/tutorial_general_203.png

