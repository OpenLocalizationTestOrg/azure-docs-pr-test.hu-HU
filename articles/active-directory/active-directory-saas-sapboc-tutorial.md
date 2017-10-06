---
title: "Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure egyszeri bejelentkezés Azure Active Directory és az SAP Business objektumot felhő között."
services: active-directory
documentationCenter: na
author: jeevansd
manager: femila
ms.reviewer: joflore
ms.assetid: 6c5e44f0-4e52-463f-b879-834d80a55cdf
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jeedes
ms.openlocfilehash: a3e9bd93897271531f91bcbc50cd361e8a20551e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-azure-active-directory-integration-with-sap-business-object-cloud"></a><span data-ttu-id="d4368-103">Oktatóanyag: Azure Active Directory-integráció SAP üzleti objektum a felhő</span><span class="sxs-lookup"><span data-stu-id="d4368-103">Tutorial: Azure Active Directory integration with SAP Business Object Cloud</span></span>

<span data-ttu-id="d4368-104">Ebben az oktatóanyagban elsajátíthatja, hogyan toointegrate SAP Business objektumot felhőalapú Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="d4368-104">In this tutorial, you learn how toointegrate SAP Business Object Cloud with Azure Active Directory (Azure AD).</span></span>

<span data-ttu-id="d4368-105">A következő előnyöket SAP Business objektumot felhőalapú Azure AD-val integrálásakor hello elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="d4368-105">You get hello following benefits when you integrate SAP Business Object Cloud with Azure AD:</span></span>

- <span data-ttu-id="d4368-106">Az Azure ad-ben szabályozhatja, kik hozzáférés tooSAP üzleti objektumot felhő.</span><span class="sxs-lookup"><span data-stu-id="d4368-106">In Azure AD, you can control who has access tooSAP Business Object Cloud.</span></span>
- <span data-ttu-id="d4368-107">Automatikusan bejelentkezhet a felhasználók tooSAP üzleti objektumot felhő egyszeri bejelentkezést és a felhasználó Azure AD-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d4368-107">You can automatically sign in your users tooSAP Business Object Cloud by using single sign-on and a user's Azure AD account.</span></span>
- <span data-ttu-id="d4368-108">A fiók egyetlen, központi helyen, hello Azure-portálon kezelheti.</span><span class="sxs-lookup"><span data-stu-id="d4368-108">You can manage your accounts in one, central location, hello Azure portal.</span></span>

<span data-ttu-id="d4368-109">További információ az Azure AD-val egy szolgáltatott szoftverként (SaaS) alkalmazásintegráció szoftverként toolearn lásd [alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryval?](active-directory-appssoaccess-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d4368-109">toolearn more about software as a service (SaaS) app integration with Azure AD, see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d4368-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d4368-110">Prerequisites</span></span>

<span data-ttu-id="d4368-111">tooset mentése az Azure AD-integrációs SAP üzleti objektum a felhő, a következő elemek hello kell:</span><span class="sxs-lookup"><span data-stu-id="d4368-111">tooset up Azure AD integration with SAP Business Object Cloud, you need hello following items:</span></span>

- <span data-ttu-id="d4368-112">Az Azure AD szolgáltatásra</span><span class="sxs-lookup"><span data-stu-id="d4368-112">An Azure AD subscription</span></span>
- <span data-ttu-id="d4368-113">Egy SAP üzleti objektum felhőben, az egyszeri bejelentkezés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="d4368-113">An SAP Business Object Cloud, with single sign-on turned on</span></span>

> [!NOTE]
> <span data-ttu-id="d4368-114">Ha ebben az oktatóanyagban hello lépéseket teszteli, azt javasoljuk, hogy ne tesztelje azokat éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="d4368-114">If you test hello steps in this tutorial, we recommend that you don't test them in a production environment.</span></span>

<span data-ttu-id="d4368-115">Ebben az oktatóanyagban hello lépéseket tesztelési ajánlásokat:</span><span class="sxs-lookup"><span data-stu-id="d4368-115">Recommendations for testing hello steps in this tutorial:</span></span>

- <span data-ttu-id="d4368-116">Ne használja az éles környezetben, nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d4368-116">Do not use your production environment, unless it's necessary.</span></span>
- <span data-ttu-id="d4368-117">Ha még nem rendelkezik az Azure AD próbaverziójának környezetben, akkor [ingyenes egy hónapos próbaverzió beszerzése](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="d4368-117">If you don't have an Azure AD trial environment, you can [get a one-month free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="scenario-description"></a><span data-ttu-id="d4368-118">Forgatókönyv leírása</span><span class="sxs-lookup"><span data-stu-id="d4368-118">Scenario description</span></span>
<span data-ttu-id="d4368-119">Ebben az oktatóanyagban tesztelése az Azure AD egyszeri bejelentkezéshez egy tesztkörnyezetben.</span><span class="sxs-lookup"><span data-stu-id="d4368-119">In this tutorial, you test Azure AD single sign-on in a test environment.</span></span> 

<span data-ttu-id="d4368-120">Ebben az oktatóanyagban leírt hello forgatókönyvben két fő építőelemeket áll:</span><span class="sxs-lookup"><span data-stu-id="d4368-120">hello scenario outlined in this tutorial consists of two main building blocks:</span></span>

1. <span data-ttu-id="d4368-121">Adja hozzá az SAP Business objektumot felhő hello gyűjteményből.</span><span class="sxs-lookup"><span data-stu-id="d4368-121">Add SAP Business Object Cloud from hello gallery.</span></span>
2. <span data-ttu-id="d4368-122">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése.</span><span class="sxs-lookup"><span data-stu-id="d4368-122">Set up and test Azure AD single sign-on.</span></span>

## <a name="add-sap-business-object-cloud-from-hello-gallery"></a><span data-ttu-id="d4368-123">Adja hozzá az SAP Business objektumot felhő hello gyűjteményből</span><span class="sxs-lookup"><span data-stu-id="d4368-123">Add SAP Business Object Cloud from hello gallery</span></span>
<span data-ttu-id="d4368-124">SAP üzleti objektum felhőalapú Azure AD-val hello katalógusában, hello integrálása tooset adnia SAP Business objektumot felhő tooyour a felügyelt SaaS-alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d4368-124">tooset up hello integration of SAP Business Object Cloud with Azure AD, in hello gallery, add SAP Business Object Cloud tooyour list of managed SaaS apps.</span></span>

<span data-ttu-id="d4368-125">tooadd SAP Business objektumot felhő hello gyűjteményből:</span><span class="sxs-lookup"><span data-stu-id="d4368-125">tooadd SAP Business Object Cloud from hello gallery:</span></span>

1. <span data-ttu-id="d4368-126">A hello [Azure-portálon](https://portal.azure.com), a bal oldali menü hello, válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d4368-126">In hello [Azure portal](https://portal.azure.com), in hello left menu, select **Azure Active Directory**.</span></span> 

    ![hello Azure Active Directory gomb][1]

2. <span data-ttu-id="d4368-128">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d4368-128">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![hello vállalati alkalmazások lap][2]
    
3. <span data-ttu-id="d4368-130">tooadd egy új alkalmazást, válassza ki **új alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d4368-130">tooadd a new application, select **New application**.</span></span>

    ![hello új alkalmazás gomb][3]

4. <span data-ttu-id="d4368-132">Hello keresési mezőbe, írja be a **SAP Business objektumot felhő**.</span><span class="sxs-lookup"><span data-stu-id="d4368-132">In hello search box, enter **SAP Business Object Cloud**.</span></span>

    ![hello keresőmezőbe](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_search.png)

5. <span data-ttu-id="d4368-134">Hello eredmények panelen, jelölje ki a **SAP Business objektumot felhő**, majd válassza ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d4368-134">In hello results panel, select **SAP Business Object Cloud**, and then select **Add**.</span></span>

    ![SAP Business objektumot felhő hello eredmények listájában](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_addfromgallery.png)

##  <a name="set-up-and-test-azure-ad-single-sign-on"></a><span data-ttu-id="d4368-136">Állítsa be, és az Azure AD az egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d4368-136">Set up and test Azure AD single sign-on</span></span>

<span data-ttu-id="d4368-137">Ebben a szakaszban a beállítása és tesztelése az Azure AD az egyszeri bejelentkezés SAP üzleti objektum a felhő alapú nevű tesztfelhasználó *Britta Simon*.</span><span class="sxs-lookup"><span data-stu-id="d4368-137">In this section, you set up and test Azure AD single sign-on with SAP Business Object Cloud based on a test user named *Britta Simon*.</span></span>

<span data-ttu-id="d4368-138">Az egyszeri bejelentkezés toowork az Azure AD tooknow hello Azure AD tartozó felhasználói SAP Business objektumot felhőben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="d4368-138">For single sign-on toowork, Azure AD needs tooknow hello Azure AD counterpart user in SAP Business Object Cloud.</span></span> <span data-ttu-id="d4368-139">Az Azure AD-felhasználó és a kapcsolódó felhasználói hello SAP Business objektumot felhőben közötti kapcsolat kapcsolatot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="d4368-139">A link relationship between an Azure AD user and hello related user in SAP Business Object Cloud must be established.</span></span>

<span data-ttu-id="d4368-140">tooestablish hello hivatkozás felhőben SAP Business objektumot, a kapcsolat a **felhasználónév**, hello hello értéket **felhasználónév** az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="d4368-140">tooestablish hello link relationship, in SAP Business Object Cloud, for **Username**, assign hello value of hello **user name** in Azure AD.</span></span>

<span data-ttu-id="d4368-141">tooconfigure és tesztelése az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú, a következő feladatok teljes hello:</span><span class="sxs-lookup"><span data-stu-id="d4368-141">tooconfigure and test Azure AD single sign-on with SAP Business Object Cloud, complete hello following tasks:</span></span>

1. <span data-ttu-id="d4368-142">[Az Azure AD az egyszeri bejelentkezés beállítása](#set-up-azure-ad-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="d4368-142">[Set up Azure AD single sign-on](#set-up-azure-ad-single-sign-on).</span></span> <span data-ttu-id="d4368-143">Ez a szolgáltatás állít be egy felhasználó toouse.</span><span class="sxs-lookup"><span data-stu-id="d4368-143">Sets up a user toouse this feature.</span></span>
2. <span data-ttu-id="d4368-144">[Hozzon létre egy Azure AD-teszt felhasználó](#create-an-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="d4368-144">[Create an Azure AD test user](#create-an-azure-ad-test-user).</span></span> <span data-ttu-id="d4368-145">Az Azure AD tesztek egyszeri bejelentkezés Britta Simon hello felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="d4368-145">Tests Azure AD single sign-on with hello user Britta Simon.</span></span>
3. <span data-ttu-id="d4368-146">[Hozzon létre egy SAP Business objektumot felhő tesztfelhasználó](#create-an-sap-business-object-cloud-test-user).</span><span class="sxs-lookup"><span data-stu-id="d4368-146">[Create an SAP Business Object Cloud test user](#create-an-sap-business-object-cloud-test-user).</span></span> <span data-ttu-id="d4368-147">Hoz létre egy Britta Simon megfelelője a felhőben SAP üzleti objektumot, amely csatolt toohello hello felhasználói az Azure AD-ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="d4368-147">Creates a counterpart of Britta Simon in SAP Business Object Cloud that is linked toohello Azure AD representation of hello user.</span></span>
4. <span data-ttu-id="d4368-148">[Rendelje hozzá az Azure AD hello tesztfelhasználó](#assign-the-azure-ad-test-user).</span><span class="sxs-lookup"><span data-stu-id="d4368-148">[Assign hello Azure AD test user](#assign-the-azure-ad-test-user).</span></span> <span data-ttu-id="d4368-149">Beállítja az Azure AD az egyszeri bejelentkezés Britta Simon toouse.</span><span class="sxs-lookup"><span data-stu-id="d4368-149">Sets up Britta Simon toouse Azure AD single sign-on.</span></span>
5. <span data-ttu-id="d4368-150">[Egyszeri bejelentkezés tesztelése](#test-single-sign-on).</span><span class="sxs-lookup"><span data-stu-id="d4368-150">[Test single sign-on](#test-single-sign-on).</span></span> <span data-ttu-id="d4368-151">Ellenőrzi, hogy a hello megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="d4368-151">Verifies that hello configuration works.</span></span>

### <a name="set-up-azure-ad-single-sign-on"></a><span data-ttu-id="d4368-152">Az Azure AD az egyszeri bejelentkezés beállítása</span><span class="sxs-lookup"><span data-stu-id="d4368-152">Set up Azure AD single sign-on</span></span>

<span data-ttu-id="d4368-153">Ebben a szakaszban bekapcsolása az Azure AD-egyszeri bejelentkezés hello Azure-portálon a.</span><span class="sxs-lookup"><span data-stu-id="d4368-153">In this section, you turn on Azure AD single sign-on in hello Azure portal.</span></span> <span data-ttu-id="d4368-154">Ezután állítsa be az egyszeri bejelentkezés az SAP Business objektumot felhőalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="d4368-154">Then, you set up single sign-on in your SAP Business Object Cloud application.</span></span>

<span data-ttu-id="d4368-155">mentése az Azure AD egyszeri bejelentkezést a SAP Business objektumot felhőalapú tooset:</span><span class="sxs-lookup"><span data-stu-id="d4368-155">tooset up Azure AD single sign-on with SAP Business Object Cloud:</span></span>

1. <span data-ttu-id="d4368-156">Az Azure portál, a hello hello **SAP Business objektumot felhő** alkalmazás integrációs lapon jelölje be **egyszeri bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d4368-156">In hello Azure portal, on hello **SAP Business Object Cloud** application integration page, select **Single sign-on**.</span></span>

    ![Válassza ki az egyszeri bejelentkezés][4]

2. <span data-ttu-id="d4368-158">A hello **egyszeri bejelentkezés** lap, a **mód**, jelölje be **SAML-alapú bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="d4368-158">On hello **Single sign-on** page, for **Mode**, select **SAML-based Sign-on**.</span></span>
 
    ![Válassza ki a SAML-alapú bejelentkezés](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_samlbase.png)

3. <span data-ttu-id="d4368-160">A **SAP Business objektumot felhőalapú tartományt és URL-címek**, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d4368-160">Under **SAP Business Object Cloud Domain and URLs**, complete hello following steps:</span></span>

    1. <span data-ttu-id="d4368-161">A hello **bejelentkezési URL-cím** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:</span><span class="sxs-lookup"><span data-stu-id="d4368-161">In hello **Sign-on URL** box, enter a URL that has hello following pattern:</span></span> 
    | |
    |-|-|
    | `https://<sub-domain>.sapanalytics.cloud/` |
    | `https://<sub-domain>.sapbusinessobjects.cloud/` |

    2. <span data-ttu-id="d4368-162">A hello **azonosító** mezőbe írjon be egy URL-címet, amely rendelkezik a következő mintát hello:</span><span class="sxs-lookup"><span data-stu-id="d4368-162">In hello **Identifier** box, enter a URL that has hello following pattern:</span></span>
    | |
    |-|-|
    | `<sub-domain>.sapbusinessobjects.cloud` |
    | `<sub-domain>.sapanalytics.cloud` |

    ![SAP Business objektumot felhőalapú tartományt és URL-címek lap URL-címek](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_url.png)
 
    > [!NOTE] 
    > <span data-ttu-id="d4368-164">hello URL-értékei csak bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="d4368-164">hello values in these URLs are for demonstration only.</span></span> <span data-ttu-id="d4368-165">Hello tényleges frissítés hello értékek bejelentkezés URL-cím és azonosító URL-címet.</span><span class="sxs-lookup"><span data-stu-id="d4368-165">Update hello values with hello actual sign-on URL and identifier URL.</span></span> <span data-ttu-id="d4368-166">tooget hello bejelentkezési URL-címe, kapcsolattartási hello [SAP Business objektumot felhőalapú ügyfél-támogatási csoport](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span><span class="sxs-lookup"><span data-stu-id="d4368-166">tooget hello sign-on URL, contact hello [SAP Business Object Cloud Client support team](https://www.sap.com/product/analytics/cloud-analytics.support.html).</span></span> <span data-ttu-id="d4368-167">Hello azonosító URL-t kaphat hello SAP Business objektumot felhő metaadatok letöltése hello felügyeleti konzolról.</span><span class="sxs-lookup"><span data-stu-id="d4368-167">You can get hello identifier URL by downloading hello SAP Business Object Cloud metadata from hello admin console.</span></span> <span data-ttu-id="d4368-168">Ennek a magyarázatát hello oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d4368-168">This is explained later in hello tutorial.</span></span> 

4. <span data-ttu-id="d4368-169">A **SAML-aláíró tanúsítványa**, jelölje be **metaadatainak XML-kódja**.</span><span class="sxs-lookup"><span data-stu-id="d4368-169">Under **SAML Signing Certificate**, select **Metadata XML**.</span></span> <span data-ttu-id="d4368-170">Mentse hello metaadatait tartalmazó fájl a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="d4368-170">Then, save hello metadata file on your computer.</span></span>

    ![Válassza ki a metaadatok XML](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_certificate.png) 

5. <span data-ttu-id="d4368-172">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d4368-172">Select **Save**.</span></span>

    ![A mentés kiválasztása](./media/active-directory-saas-sapboc-tutorial/tutorial_general_400.png)

6. <span data-ttu-id="d4368-174">Egy másik webes böngészőablakban a bejelentkezés tooyour SAP Business objektumot felhő vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d4368-174">In a different web browser window, sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

7. <span data-ttu-id="d4368-175">Válassza ki **menü** > **rendszer** > **felügyeleti**.</span><span class="sxs-lookup"><span data-stu-id="d4368-175">Select **Menu** > **System** > **Administration**.</span></span>
    
    ![Válassza ki a menüben, majd a rendszer, majd felügyeleti](./media/active-directory-saas-sapboc-tutorial/config1.png)

8. <span data-ttu-id="d4368-177">A hello **biztonsági** lapra, jelölje be hello **szerkesztése** (toll) ikonra.</span><span class="sxs-lookup"><span data-stu-id="d4368-177">On hello **Security** tab, select hello **Edit** (pen) icon.</span></span>
    
    ![Hello biztonsági lapon jelölje be a hello Szerkesztés ikonra](./media/active-directory-saas-sapboc-tutorial/config2.png)  

9. <span data-ttu-id="d4368-179">A **hitelesítési módszer**, jelölje be **SAML-alapú egyszeri bejelentkezés (SSO)**.</span><span class="sxs-lookup"><span data-stu-id="d4368-179">For **Authentication Method**, select **SAML Single Sign-On (SSO)**.</span></span>

    ![SAML-alapú egyszeri bejelentkezést hello hitelesítési mód kiválasztása](./media/active-directory-saas-sapboc-tutorial/config3.png)  

10. <span data-ttu-id="d4368-181">toodownload hello szolgáltatás szolgáltató metaadatok (1. lépés), jelölje be **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="d4368-181">toodownload hello service provider metadata (Step 1), select **Download**.</span></span> <span data-ttu-id="d4368-182">Hello metaadat-fájlt, keresse meg és másolja hello **entityid beállítást** érték.</span><span class="sxs-lookup"><span data-stu-id="d4368-182">In hello metadata file, find and copy hello **entityID** value.</span></span> <span data-ttu-id="d4368-183">A hello Azure portál, a **SAP Business objektumot felhőalapú tartományt és URL-címek**, illessze be a hello érték a hello **azonosító** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d4368-183">In hello Azure portal, under **SAP Business Object Cloud Domain and URLs**, paste hello value in hello **Identifier** box.</span></span>

    ![Másolja és illessze be a hello entityid beállítást érték](./media/active-directory-saas-sapboc-tutorial/config4.png)  

11. <span data-ttu-id="d4368-185">tooupload hello szolgáltatás szolgáltató metaadatok (2. lépés) webhelyről letöltött hello fájlban hello Azure-portálon a **identitásszolgáltató feltöltése metaadatok**, jelölje be **feltöltése**.</span><span class="sxs-lookup"><span data-stu-id="d4368-185">tooupload hello service provider metadata (Step 2) in hello file that you downloaded from hello Azure portal, under **Upload Identity Provider metadata**, select **Upload**.</span></span>  

    ![A metaadatok feltöltése identitásszolgáltató, területen válassza a feltöltés](./media/active-directory-saas-sapboc-tutorial/config5.png)

12. <span data-ttu-id="d4368-187">A hello **felhasználói attribútum** listájában, jelölje be hello felhasználói attribútum (3. lépés), amelyet az toouse hoznia.</span><span class="sxs-lookup"><span data-stu-id="d4368-187">In hello **User Attribute** list, select hello user attribute (Step 3) that you want toouse for your implementation.</span></span> <span data-ttu-id="d4368-188">A felhasználói attribútum toohello identitásszolgáltató képezi le.</span><span class="sxs-lookup"><span data-stu-id="d4368-188">This user attribute maps toohello identity provider.</span></span> <span data-ttu-id="d4368-189">egy egyéni attribútum használatát hello hello felhasználói oldalon tooenter **egyéni SAML-leképezési** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d4368-189">tooenter a custom attribute on hello user's page, use hello **Custom SAML Mapping** option.</span></span> <span data-ttu-id="d4368-190">Vagy, akkor ki lehet **E-mail** vagy **Felhasználóazonosító** hello felhasználói attribútumaként.</span><span class="sxs-lookup"><span data-stu-id="d4368-190">Or, you can select either **Email** or **USER ID** as hello user attribute.</span></span> <span data-ttu-id="d4368-191">Ebben a példában, hogy kiválasztott **E-mail** mivel azt leképezése hello felhasználói azonosító jogcímet hello **userprincipalname** hello attribútum **felhasználói attribútumok** hello szakasz Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="d4368-191">In our example, we selected **Email** because we mapped hello user identifier claim with hello **userprincipalname** attribute in hello **User Attributes** section in hello Azure portal.</span></span> <span data-ttu-id="d4368-192">Ez lehetővé teszi egy egyedi felhasználói e-mail fiókot, amely minden sikeres SAML-válasz toohello SAP Business objektumot felhő kérelem küldése.</span><span class="sxs-lookup"><span data-stu-id="d4368-192">This provides a unique user email, which is sent toohello SAP Business Object Cloud application in every successful SAML response.</span></span>

    ![Válassza ki a felhasználói attribútum](./media/active-directory-saas-sapboc-tutorial/config6.png)

13. <span data-ttu-id="d4368-194">tooverify hello fiók hello identitásszolgáltatóval (4. lépés), hello **bejelentkezési hitelesítő adatok (E-mail)** hello a felhasználó e-mail címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d4368-194">tooverify hello account with hello identity provider (Step 4), in hello **Login Credential (Email)** box, enter hello user's email address.</span></span> <span data-ttu-id="d4368-195">Ezt követően válassza **ellenőrizze fiókja**.</span><span class="sxs-lookup"><span data-stu-id="d4368-195">Then, select **Verify Account**.</span></span> <span data-ttu-id="d4368-196">hello rendszer bejelentkezési hitelesítő adatok toohello felhasználói fiókot ad.</span><span class="sxs-lookup"><span data-stu-id="d4368-196">hello system adds sign-in credentials toohello user account.</span></span>

    ![Adja meg e-mail címét, és válassza ki azt a fiókot ellenőrzése](./media/active-directory-saas-sapboc-tutorial/config7.png)

14. <span data-ttu-id="d4368-198">Jelölje be hello **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d4368-198">Select hello **Save** icon.</span></span>

    ![Mentés ikonra](./media/active-directory-saas-sapboc-tutorial/save.png)

> [!TIP]
> <span data-ttu-id="d4368-200">Ezeket az utasításokat a hello tömör verziója olvasható [Azure-portálon](https://portal.azure.com), míg a állítja be az alkalmazás!</span><span class="sxs-lookup"><span data-stu-id="d4368-200">You can read a concise version of these instructions in hello [Azure portal](https://portal.azure.com), while you are setting up your app!</span></span> <span data-ttu-id="d4368-201">Hello app kiválasztásával hozzáadása után **Active Directory** > **vállalati alkalmazások**, jelölje be hello **egyszeri bejelentkezés** fülre. A beágyazott hello dokumentációja a hello **konfigurációs** szakasz hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="d4368-201">After you add hello app by selecting **Active Directory** > **Enterprise Applications**, select hello **Single Sign-On** tab. You can access hello embedded documentation in hello **Configuration** section, at hello bottom of hello page.</span></span> <span data-ttu-id="d4368-202">További információkért lásd: [az Azure AD dokumentációjában beágyazott]( https://go.microsoft.com/fwlink/?linkid=845985).</span><span class="sxs-lookup"><span data-stu-id="d4368-202">For more information, see [Azure AD embedded documentation]( https://go.microsoft.com/fwlink/?linkid=845985).</span></span>

### <a name="create-an-azure-ad-test-user"></a><span data-ttu-id="d4368-203">Hozzon létre egy Azure AD-teszt felhasználó</span><span class="sxs-lookup"><span data-stu-id="d4368-203">Create an Azure AD test user</span></span>
<span data-ttu-id="d4368-204">Ebben a szakaszban Britta Simon nevű hello Azure-portálon a tesztfelhasználó létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d4368-204">In this section, you create a test user named Britta Simon in hello Azure portal.</span></span>

<span data-ttu-id="d4368-205">az Azure AD tesztfelhasználó toocreate:</span><span class="sxs-lookup"><span data-stu-id="d4368-205">toocreate a test user in Azure AD:</span></span>

1. <span data-ttu-id="d4368-206">Hello Azure-portálon, hello bal oldali menüben válassza ki **Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="d4368-206">In hello Azure portal, in hello left menu, select **Azure Active Directory**.</span></span>

    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_01.png) 

2. <span data-ttu-id="d4368-208">toodisplay hello listában válassza ki a felhasználók **felhasználók és csoportok**, majd válassza ki **minden felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="d4368-208">toodisplay hello list of users, select **Users and groups**, and then select **All users**.</span></span>
    
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_02.png) 

3. <span data-ttu-id="d4368-210">tooopen hello **felhasználói** párbeszédpanelen jelölje ki **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="d4368-210">tooopen hello **User** dialog box, select **Add**.</span></span>
 
    ![Az Azure AD tesztfelhasználó létrehozása](./media/active-directory-saas-sapboc-tutorial/create_aaduser_03.png) 

4. <span data-ttu-id="d4368-212">A hello **felhasználói** teljes hello lépéseket a következő párbeszédpanelen:</span><span class="sxs-lookup"><span data-stu-id="d4368-212">In hello **User** dialog box, complete hello following steps:</span></span>
 
    1. <span data-ttu-id="d4368-213">A hello **neve** adja meg a **BrittaSimon**.</span><span class="sxs-lookup"><span data-stu-id="d4368-213">In hello **Name** box, enter **BrittaSimon**.</span></span>

    2. <span data-ttu-id="d4368-214">A hello **felhasználónév** hello felhasználó Britta Simon hello e-mail címét adja meg.</span><span class="sxs-lookup"><span data-stu-id="d4368-214">In hello **User name** box, enter hello email address of hello user Britta Simon.</span></span>

    3. <span data-ttu-id="d4368-215">Jelölje be hello **megjelenítése jelszó** jelölje be a jelölőnégyzetet, és jegyezze fel a hello hello érték **jelszó** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d4368-215">Select hello **Show Password** check box, and then write down hello value that's displayed in hello **Password** box.</span></span>

    4. <span data-ttu-id="d4368-216">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d4368-216">Select **Create**.</span></span>

        ![hello felhasználó párbeszédpanel](./media/active-directory-saas-sapboc-tutorial/create_aaduser_04.png) 

    ![Az Azure AD-felhasználó létrehozása][100]

### <a name="create-an-sap-business-object-cloud-test-user"></a><span data-ttu-id="d4368-219">Az SAP Business objektumot felhő tesztfelhasználó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d4368-219">Create an SAP Business Object Cloud test user</span></span>

<span data-ttu-id="d4368-220">Az Azure Active Directory-felhasználók ki kell építenie SAP Business objektumot felhőben, mielőtt bejelentkeznének tooSAP üzleti objektumot felhő.</span><span class="sxs-lookup"><span data-stu-id="d4368-220">Azure AD users must be provisioned in SAP Business Object Cloud before they can sign in tooSAP Business Object Cloud.</span></span> <span data-ttu-id="d4368-221">SAP Business objektumot felhőben kézi tevékenység.</span><span class="sxs-lookup"><span data-stu-id="d4368-221">In SAP Business Object Cloud, provisioning is a manual task.</span></span>

<span data-ttu-id="d4368-222">tooprovision egy felhasználói fiókot:</span><span class="sxs-lookup"><span data-stu-id="d4368-222">tooprovision a user account:</span></span>

1. <span data-ttu-id="d4368-223">Jelentkezzen be tooyour SAP Business objektumot felhő vállalati hely rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="d4368-223">Sign in tooyour SAP Business Object Cloud company site as an administrator.</span></span>

2. <span data-ttu-id="d4368-224">Válassza ki **menü** > **biztonsági** > **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="d4368-224">Select **Menu** > **Security** > **Users**.</span></span>

    ![Alkalmazott hozzáadása](./media/active-directory-saas-sapboc-tutorial/user1.png)

3. <span data-ttu-id="d4368-226">A hello **felhasználók** lapon tooadd új felhasználó adatait, kiválaszthat  **+** .</span><span class="sxs-lookup"><span data-stu-id="d4368-226">On hello **Users** page, tooadd new user details, select **+**.</span></span> 

    ![Felhasználók hozzáadására szolgáló oldala](./media/active-directory-saas-sapboc-tutorial/user4.png)

    <span data-ttu-id="d4368-228">Kövesse az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="d4368-228">Then, complete hello following steps:</span></span>

    1. <span data-ttu-id="d4368-229">A hello **Felhasználóazonosító** adja meg a Felhasználóazonosítót hello hello felhasználó, például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d4368-229">In hello **USER ID** box, enter hello user ID of hello user, like **Britta**.</span></span>

    2. <span data-ttu-id="d4368-230">A hello **UTÓNÉV** mezőbe írja be a hello első hello felhasználó nevét, például **Britta**.</span><span class="sxs-lookup"><span data-stu-id="d4368-230">In hello **FIRST NAME** box, enter hello first name of hello user, like **Britta**.</span></span>

    3. <span data-ttu-id="d4368-231">A hello **Vezetéknév** mezőbe írja be a hello utolsó hello felhasználó nevét, például **Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4368-231">In hello **LAST NAME** box, enter hello last name of hello user, like **Simon**.</span></span>

    4. <span data-ttu-id="d4368-232">A hello **MEGJELENÍTETT név** mezőbe írja be a hello teljes hello felhasználó nevét, például **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4368-232">In hello **DISPLAY NAME** box, enter hello full name of hello user, like **Britta Simon**.</span></span>

    5. <span data-ttu-id="d4368-233">A hello **E-MAIL** mezőbe írja be például hello felhasználó e-mail címe hello  **brittasimon@contoso.com** .</span><span class="sxs-lookup"><span data-stu-id="d4368-233">In hello **E-MAIL** box, enter hello email address of hello user, like **brittasimon@contoso.com**.</span></span>

    6. <span data-ttu-id="d4368-234">A hello **szerepkörök kiválasztása** lapon, hello megfelelő hello felhasználói szerepkört, majd válassza ki és **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4368-234">On hello **Select Roles** page, select hello appropriate role for hello user, and then select **OK**.</span></span>

      ![Szerepkör kiválasztása](./media/active-directory-saas-sapboc-tutorial/user3.png)

    7. <span data-ttu-id="d4368-236">Jelölje be hello **mentése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d4368-236">Select hello **Save** icon.</span></span>  


### <a name="assign-hello-azure-ad-test-user"></a><span data-ttu-id="d4368-237">Rendelje hozzá az Azure AD hello tesztfelhasználó számára</span><span class="sxs-lookup"><span data-stu-id="d4368-237">Assign hello Azure AD test user</span></span>

<span data-ttu-id="d4368-238">Ebben a szakaszban engedélyezése hello felhasználó Britta Simon toouse az Azure AD egyszeri bejelentkezést a hello felhasználói fiók hozzáférési tooSAP üzleti objektumot felhő megadásával.</span><span class="sxs-lookup"><span data-stu-id="d4368-238">In this section, you allow hello user Britta Simon toouse Azure AD single sign-on by granting hello user account access tooSAP Business Object Cloud.</span></span>

<span data-ttu-id="d4368-239">tooassign Britta Simon tooSAP üzleti objektumot felhő:</span><span class="sxs-lookup"><span data-stu-id="d4368-239">tooassign Britta Simon tooSAP Business Object Cloud:</span></span>

1. <span data-ttu-id="d4368-240">Hello Azure-portálon, a hello alkalmazások nézet megnyitásához, és folytassa a toohello könyvtár nézetben.</span><span class="sxs-lookup"><span data-stu-id="d4368-240">In hello Azure portal, open hello applications view, and then go toohello directory view.</span></span> <span data-ttu-id="d4368-241">Válassza ki **vállalati alkalmazások**, majd válassza ki **összes alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="d4368-241">Select **Enterprise applications**, and then select **All applications**.</span></span>

    ![Felhasználó hozzárendelése][201] 

2. <span data-ttu-id="d4368-243">Hello alkalmazások listában válassza ki a **SAP Business objektumot felhő**.</span><span class="sxs-lookup"><span data-stu-id="d4368-243">In hello applications list, select **SAP Business Object Cloud**.</span></span>

    ![Egyszeri bejelentkezés konfigurálása](./media/active-directory-saas-sapboc-tutorial/tutorial_sapboc_app.png) 

3. <span data-ttu-id="d4368-245">Hello bal oldali menüben válasszon ki **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d4368-245">In hello left menu, select **Users and groups**.</span></span>

    ![Felhasználók és csoportok kiválasztása][202] 

4. <span data-ttu-id="d4368-247">Válassza a **Hozzáadás** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d4368-247">Select **Add**.</span></span> <span data-ttu-id="d4368-248">Ezután a hello **hozzáadása hozzárendelés** lapon jelölje be **felhasználók és csoportok**.</span><span class="sxs-lookup"><span data-stu-id="d4368-248">Then, on hello **Add Assignment** page, select **Users and groups**.</span></span>

    ![hello hozzáadása hozzárendelés lap][203]

5. <span data-ttu-id="d4368-250">A hello **felhasználók és csoportok** lapra, jelölje be a felhasználók hello listáján **Britta Simon**.</span><span class="sxs-lookup"><span data-stu-id="d4368-250">On hello **Users and groups** page, in hello list of users, select **Britta Simon**.</span></span>

6. <span data-ttu-id="d4368-251">A hello **felhasználók és csoportok** lapon jelölje be **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="d4368-251">On hello **Users and groups** page, select **Select**.</span></span>

7. <span data-ttu-id="d4368-252">A hello **hozzáadása hozzárendelés** lapon jelölje be **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="d4368-252">On hello **Add Assignment** page, select **Assign**.</span></span>

![Hello felhasználói szerepkör hozzárendelése][200] 
    
### <a name="test-single-sign-on"></a><span data-ttu-id="d4368-254">Egyszeri bejelentkezés tesztelése</span><span class="sxs-lookup"><span data-stu-id="d4368-254">Test single sign-on</span></span>

<span data-ttu-id="d4368-255">Ebben a szakaszban az Azure AD egyszeri bejelentkezés beállításai hello hozzáférési panel segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="d4368-255">In this section, you test your Azure AD single sign-on configuration by using hello access panel.</span></span>

<span data-ttu-id="d4368-256">Hello SAP Business objektumot felhő csempe hello hozzáférési panelen válassza ki, amikor meg kell automatikusan megtörténik a tooyour SAP Business objektumot felhőalapú alkalmazásnál.</span><span class="sxs-lookup"><span data-stu-id="d4368-256">When you select hello SAP Business Object Cloud tile in hello access panel, you should be automatically signed in tooyour SAP Business Object Cloud application.</span></span>

<span data-ttu-id="d4368-257">Hello hozzáférési panel kapcsolatos további információkért lásd: [bemutatása toohello hozzáférési panel](active-directory-saas-access-panel-introduction.md).</span><span class="sxs-lookup"><span data-stu-id="d4368-257">For more information about hello access panel, see [Introduction toohello access panel](active-directory-saas-access-panel-introduction.md).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4368-258">További források</span><span class="sxs-lookup"><span data-stu-id="d4368-258">Additional resources</span></span>

* [<span data-ttu-id="d4368-259">Hogyan kapcsolatos bemutatók felsorolása toointegrate SaaS-alkalmazásokhoz az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d4368-259">List of tutorials on how toointegrate SaaS apps with Azure Active Directory</span></span>](active-directory-saas-tutorial-list.md)
* [<span data-ttu-id="d4368-260">Mi az az alkalmazás-hozzáférés és egyszeri bejelentkezés az Azure Active Directoryban?</span><span class="sxs-lookup"><span data-stu-id="d4368-260">What is application access and single sign-on with Azure Active Directory?</span></span>](active-directory-appssoaccess-whatis.md)


<!--Image references-->

[1]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_04.png

[100]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_201.png
[202]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_202.png
[203]: ./media/active-directory-saas-sapboc-tutorial/tutorial_general_203.png

