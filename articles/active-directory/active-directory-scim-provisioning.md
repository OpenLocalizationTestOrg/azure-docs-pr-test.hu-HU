---
title: "Rendszert használ, a tartományok közötti Identity Management automatikusan létesítsen felhasználók és csoportok az Azure Active Directory alkalmazásokhoz |} Microsoft Docs"
description: "Az Azure Active Directory automatikusan telepíthetik a felhasználókat és csoportokat a webszolgáltatás által a felülettel, a SCIM protokoll specifikációja definiálva van fronted alkalmazás vagy identitás tároló"
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 4d86f3dc-e2d3-4bde-81a3-4a0e092551c0
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2017
ms.author: asmalser
ms.reviewer: asmalser
ms.custom: aaddev;it-pro;oldportal
ms.openlocfilehash: 91978cee88d55c99bcb63c63cdaf01581ae84668
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="using-system-for-cross-domain-identity-management-to-automatically-provision-users-and-groups-from-azure-active-directory-to-applications"></a><span data-ttu-id="5b7ff-103">A tartományok közötti Identity Management rendszert használ automatikusan a felhasználók és csoportok az Azure Active Directory alkalmazások telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="5b7ff-103">Using System for Cross-Domain Identity Management to automatically provision users and groups from Azure Active Directory to applications</span></span>

## <a name="overview"></a><span data-ttu-id="5b7ff-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="5b7ff-104">Overview</span></span>
<span data-ttu-id="5b7ff-105">Azure Active Directory (Azure AD) automatikusan is konfigurálta a felhasználókat és csoportokat, a felülettel webszolgáltatás által az fronted alkalmazás vagy identitás tároló definiálva a [a tartományok közötti Identity Management (SCIM) 2.0-s protokoll specifikációja rendszer](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-105">Azure Active Directory (Azure AD) can automatically provision users and groups to any application or identity store that is fronted by a web service with the interface defined in the [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="5b7ff-106">Az Azure Active Directory küldhet kérelmek létrehozása, módosítása, illetve törlési rendelt felhasználók és csoportok a webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-106">Azure Active Directory can send requests to create, modify, or delete assigned users and groups to the web service.</span></span> <span data-ttu-id="5b7ff-107">A webszolgáltatás majd így felszabadulhatnak ezeket a kérelmeket, olyan műveleteket végez a célként megadott identitás tárolására.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-107">The web service can then translate those requests into operations on the target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="5b7ff-108">A Microsoft javasolja, hogy az Azure Portalon található [Azure AD felügyeleti központból](https://aad.portal.azure.com) kezelje az Azure AD-t az ebben a cikkben javasolt klasszikus Azure portál helyett.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="5b7ff-109">![][0]
*1. ábra: A webszolgáltatáson keresztül identitás tárolóhoz az Azure Active Directory kiépítés.*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-109">![][0]
*Figure 1: Provisioning from Azure Active Directory to an identity store via a web service*</span></span>

<span data-ttu-id="5b7ff-110">Ez a funkció használható együtt a "állapotba hozása a saját alkalmazás" képességgel rendelkező Azure AD-ben engedélyezése az egyszeri bejelentkezést és automatikus kiépítést alkalmazásokat, amelyek adja meg, vagy amelyek fronted SCIM webszolgáltatás által a felhasználói.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-110">This capability can be used in conjunction with the “bring your own app” capability in Azure AD to enable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="5b7ff-111">Számos két alkalmazási helyzetei SCIM használata az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="5b7ff-112">**Felhasználók és csoportok SCIM támogató alkalmazások kiépítés** alkalmazásokat, amelyek támogatják az SCIM 2.0 és OAuth tulajdonosi jogkivonatok használnak, a hitelesítés az Azure AD konfigurálása nélkül működik.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-112">**Provisioning users and groups to applications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="5b7ff-113">**Saját kiépítési megoldás az alkalmazások, amelyek támogatják a többi API-alapú üzembe helyezés** nem SCIM alkalmazások, létrehozhat egy SCIM végpont lefordítani az Azure AD SCIM végpont és az alkalmazás támogatja-e a felhasználók átadása az API-k között.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint to translate between the Azure AD SCIM endpoint and any API the application supports for user provisioning.</span></span> <span data-ttu-id="5b7ff-114">Is segítséget a SCIM végpont, nyújtunk a közös nyelvi infrastruktúra (CLI) tárak, amelyek bemutatják a adjon meg egy SCIM végpont és SCIM üzenetek fordítása mintakódok együtt.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-114">To help you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how to do provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-to-applications-that-support-scim"></a><span data-ttu-id="5b7ff-115">Felhasználók és csoportok SCIM támogató alkalmazások kiépítése</span><span class="sxs-lookup"><span data-stu-id="5b7ff-115">Provisioning users and groups to applications that support SCIM</span></span>
<span data-ttu-id="5b7ff-116">Az Azure AD beállítható úgy, hogy automatikusan a hozzárendelt rendelkezés felhasználókat és csoportokat, amelyek megvalósítják az alkalmazások egy [tartományok közötti identitáskezeléshez 2 (SCIM) rendszer](https://tools.ietf.org/html/draft-ietf-scim-api-19) webes szolgáltatás, és fogadja el az OAuth tulajdonosi jogkivonatokat a hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-116">Azure AD can be configured to automatically provision assigned users and groups to applications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="5b7ff-117">Belül a SCIM 2.0-s specifikációnak alkalmazások követelménynek kell megfelelnie:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-117">Within the SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="5b7ff-118">Támogatja az felhasználói és/vagy csoportok szerint szakasz 3.3 SCIM protokoll létrehozását.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-118">Supports creating users and/or groups, as per section 3.3 of the SCIM protocol.</span></span>  
* <span data-ttu-id="5b7ff-119">Támogatja a felhasználók és/vagy csoportok párosítása a patch kéréseknek szakasz 3.5.2 SCIM protokoll szerinti módosítását.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="5b7ff-120">Támogatja az adott szakasz 3.4.1 SCIM protokoll ismert erőforrás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-120">Supports retrieving a known resource as per section 3.4.1 of the SCIM protocol.</span></span>  
* <span data-ttu-id="5b7ff-121">Támogatja a felhasználók és/vagy csoportok szerint szakasz 3.4.2 SCIM protokoll lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-121">Supports querying users and/or groups, as per section 3.4.2 of the SCIM protocol.</span></span>  <span data-ttu-id="5b7ff-122">Alapértelmezés szerint externalId rendszer megkérdezi a felhasználókat és csoportokat a rendszer megkérdezi a displayName által.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="5b7ff-123">Felhasználói azonosító és a kezelő szakasz 3.4.2 SCIM protokoll szerinti lekérdezése támogatja.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-123">Supports querying user by ID and by manager as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="5b7ff-124">Támogatja az ID és a tag szakasz 3.4.2 SCIM protokoll szerinti csoportok lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-124">Supports querying groups by ID and by member as per section 3.4.2 of the SCIM protocol.</span></span>  
* <span data-ttu-id="5b7ff-125">SCIM protokoll szerinti szakasz 2.1 engedélyezési OAuth tulajdonosi jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of the SCIM protocol.</span></span>

<span data-ttu-id="5b7ff-126">Kérje meg az alkalmazás-szolgáltatót, és ezeket a követelményeket is kompatibilisek állapotkimutatások az alkalmazás szolgáltató dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="5b7ff-127">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5b7ff-127">Getting started</span></span>
<span data-ttu-id="5b7ff-128">Ez a cikk a leírt SCIM profil támogató alkalmazások Azure Active Directory, az Azure AD application gallery a "nem galéria alkalmazás" funkciójával lehet csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-128">Applications that support the SCIM profile described in this article can be connected to Azure Active Directory using the "non-gallery application" feature in the Azure AD application gallery.</span></span> <span data-ttu-id="5b7ff-129">A csatlakozás után a Azure AD át 20 percenként, ahol azt az alkalmazás SCIM végpont hozzárendelt felhasználók és csoportok, lekérdezi és hoz létre vagy módosítja őket a hozzárendelés részletek alapján fut a szinkronizálási folyamat.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries the application's SCIM endpoint for assigned users and groups, and creates or modifies them according to the assignment details.</span></span>

<span data-ttu-id="5b7ff-130">**Alkalmazást, amely támogatja a SCIM:**</span><span class="sxs-lookup"><span data-stu-id="5b7ff-130">**To connect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="5b7ff-131">Jelentkezzen be [az Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-131">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="5b7ff-132">Keresse meg a ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-132">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="5b7ff-133">Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikonra az alkalmazás objektum létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-133">Enter a name for your application, and click **Add** icon to create an app object.</span></span>
    
  <span data-ttu-id="5b7ff-134">![][1]
  *2. ábra: Az Azure AD application gallery*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="5b7ff-135">Az eredményül kapott képernyőn válassza ki a **kiépítési** lapon a bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-135">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="5b7ff-136">Az a **kiépítési üzemmódban** menü **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-136">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="5b7ff-137">![][2]
  *3. ábra: Konfigurálása kiosztás az Azure portálon*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-137">![][2]
*Figure 3: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="5b7ff-138">Az a **bérlői URL-cím** mezőbe írja be az alkalmazás SCIM végpont URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-138">In the **Tenant URL** field, enter the URL of the application's SCIM endpoint.</span></span> <span data-ttu-id="5b7ff-139">Példa: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="5b7ff-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="5b7ff-140">Ha a SCIM végpont az OAuth tulajdonosi jogkivonat nem az Azure AD egy kibocsátótól igényel, majd másolja a szükséges OAuth tulajdonosi jogkivonatot az opcionális **titkos Token** mező.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-140">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="5b7ff-141">Ez a mező üresen marad, ha az Azure AD az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="5b7ff-142">Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="5b7ff-143">Kattintson a **kapcsolat tesztelése** kell rendelkeznie az Azure Active Directory megpróbál csatlakozni a SCIM végpont gombra.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-143">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="5b7ff-144">Ha a kísérlet sikertelen, hiba információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-144">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="5b7ff-145">Ha a kísérel meg csatlakozni az alkalmazás Succeed, majd kattintson a **mentése** mentéséhez rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-145">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="5b7ff-146">Az a **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-146">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="5b7ff-147">Válassza ki egyenként tekintse át a az alkalmazás Azure Active Directoryból szinkronizált attribútumok.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-147">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="5b7ff-148">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználókat és csoportokat a frissítési műveletek az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-148">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="5b7ff-149">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-149">Select the Save button to commit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="5b7ff-150">Igény szerint letilthatja a "csoport" leképezési letiltásával objektumok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-150">You can optionally disable syncing of group objects by disabling the "groups" mapping.</span></span> 

11. <span data-ttu-id="5b7ff-151">A **beállítások**, a **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-151">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="5b7ff-152">Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálás felhasználók és csoportok hozzárendelve a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="5b7ff-153">A konfiguráció befejezése után módosítsa a **kiépítési állapot** való **a**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-153">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="5b7ff-154">Kattintson a **mentése** elindítani az Azure AD szolgáltatás kiépítését.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-154">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="5b7ff-155">Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), ügyeljen arra, hogy válassza ki a **felhasználók és csoportok** lapra, és rendeljen a felhasználók és/vagy csoportok, szinkronizálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-155">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="5b7ff-156">Ha a kezdeti szinkronizálás elindult, a **naplók** lapon figyelemmel a folyamat állapotát, amely tartalmazza az alkalmazás a létesítési szolgáltatás által végzett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-156">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="5b7ff-157">Olvassa el az Azure AD-naplók kiépítés módjáról további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-157">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="5b7ff-158">A kezdeti szinkronizálás végrehajtásához ezt követő szinkronizálások, amely körülbelül 20 percenként történik, amíg a szolgáltatás fut-nál több időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-158">The initial sync takes longer to perform than subsequent syncs, which occur approximately every 20 minutes as long as the service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="5b7ff-159">Bármely alkalmazás a saját kiépítési megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b7ff-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="5b7ff-160">Hozzon létre egy SCIM webszolgáltatás-bővítmény is eltárolni, amely az Azure Active Directoryval, engedélyezheti a egyetlen bejelentkezést és automatikus felhasználólétesítés szinte bármilyen alkalmazáshoz, amely a kiépítés API REST vagy SOAP felhasználó biztosít.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="5b7ff-161">Itt látható, hogyan működik:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-161">Here’s how it works:</span></span>

1. <span data-ttu-id="5b7ff-162">Az Azure AD biztosít a közös nyelvi infrastruktúra szalagtár nevű [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="5b7ff-163">Rendszerintegrátorok és a fejlesztők használhatja ezt a szalagtárat hozhat létre és telepíthet egy SCIM-alapú webszolgáltatás végpontja csatlakozni tudnak az Azure AD bármely alkalmazás identitás tárolására.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-163">System integrators and developers can use this library to create and deploy a SCIM-based web service endpoint capable of connecting Azure AD to any application’s identity store.</span></span>
2. <span data-ttu-id="5b7ff-164">A webszolgáltatás a szabványos felhasználói séma hozzárendelése a felhasználó séma- és az alkalmazás által igényelt protokoll hozzárendelések valósíthatók meg.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-164">Mappings are implemented in the web service to map the standardized user schema to the user schema and protocol required by the application.</span></span>
3. <span data-ttu-id="5b7ff-165">A végponti URL-cím regisztrálva van az Azure AD-egyéni alkalmazás az alkalmazás-katalógus részeként.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-165">The endpoint URL is registered in Azure AD as part of a custom application in the application gallery.</span></span>
4. <span data-ttu-id="5b7ff-166">Ez az alkalmazás az Azure AD-felhasználók és csoportok vannak hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-166">Users and groups are assigned to this application in Azure AD.</span></span> <span data-ttu-id="5b7ff-167">Hozzárendelés, akkor a célalkalmazásnak történő szinkronizálásának engedélyezése egy várólistára kerülnek.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-167">Upon assignment, they are put into a queue to be synchronized to the target application.</span></span> <span data-ttu-id="5b7ff-168">A szinkronizálási folyamat a várólista kezelése 20 percenként fut.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-168">The synchronization process handling the queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="5b7ff-169">Kódminták</span><span class="sxs-lookup"><span data-stu-id="5b7ff-169">Code Samples</span></span>
<span data-ttu-id="5b7ff-170">Ez a folyamat egyszerűbb, amelynek annak [Kódminták](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) feltéve, hogy hozzon létre egy SCIM webszolgáltatási végpontot, és mutassa be, az Automatikus kiépítés.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-170">To make this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="5b7ff-171">Egy minta megtartja sorait a CSV-felhasználók és csoportok közti szolgáltatóra van.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="5b7ff-172">A másik pedig a szolgáltató, amely az Amazon Web Services identitás és hozzáférés-kezelés szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-172">The other is of a provider that operates on the Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="5b7ff-173">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="5b7ff-173">**Prerequisites**</span></span>

* <span data-ttu-id="5b7ff-174">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="5b7ff-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="5b7ff-175">Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="5b7ff-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="5b7ff-176">Windows számítógép, amely támogatja az ASP.NET keretrendszer 4.5-ös, a SCIM végpont használható.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-176">Windows machine that supports the ASP.NET framework 4.5 to be used as the SCIM endpoint.</span></span> <span data-ttu-id="5b7ff-177">Ezen a számítógépen a felhőből elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-177">This machine must be accessible from the cloud</span></span>
* [<span data-ttu-id="5b7ff-178">Prémium szintű Azure AD egy próba- vagy licencelt verziójával egy Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="5b7ff-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="5b7ff-179">Az Amazon AWS minta van szükség a szalagtárak a [AWS eszközkészlet a Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-179">The Amazon AWS sample requires libraries from the [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="5b7ff-180">További információkért lásd: a minta az információs fájl.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-180">For more information, see the README file included with the sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="5b7ff-181">Első lépések</span><span class="sxs-lookup"><span data-stu-id="5b7ff-181">Getting Started</span></span>
<span data-ttu-id="5b7ff-182">Az egy SCIM végpontot, amelyhez is fogadja el a kiépítési kérelmekre, az Azure AD végrehajtásához legkönnyebben létrehozásához és telepítéséhez a kódminta, amely a kiépített felhasználók számára egy vesszővel tagolt (CSV) fájl.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-182">The easiest way to implement a SCIM endpoint that can accept provisioning requests from Azure AD is to build and deploy the code sample that outputs the provisioned users to a comma-separated value (CSV) file.</span></span>

<span data-ttu-id="5b7ff-183">**A minta SCIM-végpont létrehozása:**</span><span class="sxs-lookup"><span data-stu-id="5b7ff-183">**To create a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="5b7ff-184">Töltse le a kód a minta csomagjához [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="5b7ff-184">Download the code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="5b7ff-185">Bontsa ki a csomagot, és helyezze el a Windows-számítógép C:\AzureAD-BYOA-Provisioning-Samples\ például egy helyen.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-185">Unzip the package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="5b7ff-186">Ebben a mappában nyissa meg a Visual Studio FileProvisioningAgent megoldás.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-186">In this folder, launch the FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="5b7ff-187">Válassza ki **eszközök > Kódtárcsomag-kezelő > Csomagkezelő konzol**, és a következő parancsok a FileProvisioningAgent projekt oldani a megoldás hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute the following commands for the FileProvisioningAgent project to resolve the solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="5b7ff-188">A FileProvisioningAgent projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-188">Build the FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="5b7ff-189">Indítsa el a (rendszergazdaként) a Windows parancssori alkalmazás, és használja a **cd** paranccsal lépjen be a **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mappa.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-189">Launch the Command Prompt application in Windows (as an Administrator), and use the **cd** command to change the directory to your **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="5b7ff-190">A következő parancsot, a Windows-számítógép IP-cím vagy tartománynév kiszolgálónevét < ip-cím > cseréje:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-190">Run the following command, replacing <ip-address> with the IP address or domain name of the Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="5b7ff-191">A Windows **Windows-beállítások > hálózat és Internet beállítások**, jelölje be a **Windows tűzfal > Speciális beállítások**, és hozzon létre egy **bejövő forgalomra vonatkozó szabály** , amely lehetővé teszi, hogy a befelé irányuló port 9000.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-191">In Windows under **Windows Settings > Network & Internet Settings**, select the **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access to port 9000.</span></span>
9. <span data-ttu-id="5b7ff-192">Ha a Windows-számítógép útválasztó mögött, az útválasztó kell megadni a portot, amely kommunikál az internettel 9000, és a port 9000 a Windows-számítógép közötti hálózati hozzáférési fordítási végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-192">If the Windows machine is behind a router, the router needs to be configured to perform Network Access Translation between its port 9000 that is exposed to the internet, and port 9000 on the Windows machine.</span></span> <span data-ttu-id="5b7ff-193">Ez azért szükséges, az Azure AD-be tudják elérni az ehhez a végponthoz, a felhőben.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-193">This is required for Azure AD to be able to access this endpoint in the cloud.</span></span>

<span data-ttu-id="5b7ff-194">**A minta SCIM végpont regisztrálása az Azure ad-ben:**</span><span class="sxs-lookup"><span data-stu-id="5b7ff-194">**To register the sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="5b7ff-195">Jelentkezzen be [az Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-195">Sign in to [the Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="5b7ff-196">Keresse meg a ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-196">Browse to **Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="5b7ff-197">Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikonra az alkalmazás objektum létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-197">Enter a name for your application, and click **Add** icon to create an app object.</span></span> <span data-ttu-id="5b7ff-198">Az application objektum létrehozása a cél alkalmazás kellene lenniük történő, és egyszeri bejelentkezés, és nem csak a SCIM végpont végrehajtási képviselő készült.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-198">The application object created is intended to represent the target app you would be provisioning to and implementing single sign-on for, and not just the SCIM endpoint.</span></span>
4. <span data-ttu-id="5b7ff-199">Az eredményül kapott képernyőn válassza ki a **kiépítési** lapon a bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-199">In the resulting screen, select the **Provisioning** tab in the left column.</span></span>
5. <span data-ttu-id="5b7ff-200">Az a **kiépítési üzemmódban** menü **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-200">In the **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="5b7ff-201">![][2]
  *4. ábra: Konfigurálása kiosztás az Azure portálon*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-201">![][2]
*Figure 4: Configuring provisioning in the Azure portal*</span></span>
    
6. <span data-ttu-id="5b7ff-202">Az a **bérlői URL-cím** mezőbe írja be az internet elérhetővé tett URL-cím és port a SCIM végpont.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-202">In the **Tenant URL** field, enter the internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="5b7ff-203">Ez lenne valamit, például http://testmachine.contoso.com:9000 vagy http://<ip-address>:9000/, ahol a < ip-cím > az interneten közzétéve az IP cím.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is the internet exposed IP address.</span></span>  
7. <span data-ttu-id="5b7ff-204">Ha a SCIM végpont az OAuth tulajdonosi jogkivonat nem az Azure AD egy kibocsátótól igényel, majd másolja a szükséges OAuth tulajdonosi jogkivonatot az opcionális **titkos Token** mező.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-204">If the SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy the required OAuth bearer token into the optional **Secret Token** field.</span></span> <span data-ttu-id="5b7ff-205">Ez a mező üresen marad, ha az Azure AD tartalmazza az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="5b7ff-206">Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="5b7ff-207">Kattintson a **kapcsolat tesztelése** kell rendelkeznie az Azure Active Directory megpróbál csatlakozni a SCIM végpont gombra.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-207">Click the **Test Connection** button to have Azure Active Directory attempt to connect to the SCIM endpoint.</span></span> <span data-ttu-id="5b7ff-208">Ha a kísérlet sikertelen, hiba információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-208">If the attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="5b7ff-209">Ha a kísérel meg csatlakozni az alkalmazás Succeed, majd kattintson a **mentése** mentéséhez rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-209">If the attempts to connect to the application succeed, then click **Save** to save the admin credentials.</span></span>
10. <span data-ttu-id="5b7ff-210">Az a **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-210">In the **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="5b7ff-211">Válassza ki egyenként tekintse át a az alkalmazás Azure Active Directoryból szinkronizált attribútumok.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-211">Select each one to review the attributes that are synchronized from Azure Active Directory to your app.</span></span> <span data-ttu-id="5b7ff-212">A kiválasztott attribútumok **egyező** tulajdonságok használatával felel meg a felhasználókat és csoportokat a frissítési műveletek az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-212">The attributes selected as **Matching** properties are used to match the users and groups in your app for update operations.</span></span> <span data-ttu-id="5b7ff-213">Válassza ki a Mentés gombra a módosítások véglegesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-213">Select the Save button to commit any changes.</span></span>
11. <span data-ttu-id="5b7ff-214">A **beállítások**, a **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-214">Under **Settings**, the **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="5b7ff-215">Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálás felhasználók és csoportok hozzárendelve a **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in the **Users and groups** tab.</span></span>
12. <span data-ttu-id="5b7ff-216">A konfiguráció befejezése után módosítsa a **kiépítési állapot** való **a**.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-216">Once your configuration is complete, change the **Provisioning Status** to **On**.</span></span>
13. <span data-ttu-id="5b7ff-217">Kattintson a **mentése** elindítani az Azure AD szolgáltatás kiépítését.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-217">Click **Save** to start the Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="5b7ff-218">Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), ügyeljen arra, hogy válassza ki a **felhasználók és csoportok** lapra, és rendeljen a felhasználók és/vagy csoportok, szinkronizálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-218">If syncing only assigned users and groups (recommended), be sure to select the **Users and groups** tab and assign the users and/or groups you wish to sync.</span></span>

<span data-ttu-id="5b7ff-219">Ha a kezdeti szinkronizálás elindult, a **naplók** lapon figyelemmel a folyamat állapotát, amely tartalmazza az alkalmazás a létesítési szolgáltatás által végzett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-219">Once the initial synchronization has started, you can use the **Audit logs** tab to monitor progress, which shows all actions performed by the provisioning service on your app.</span></span> <span data-ttu-id="5b7ff-220">Olvassa el az Azure AD-naplók kiépítés módjáról további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="5b7ff-220">For more information on how to read the Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="5b7ff-221">Az utolsó lépés a minta ellenőrzése során, hogy a \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mappát a Windows-számítógépen nyissa meg a TargetFile.csv fájlt.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-221">The final step in verifying the sample is to open the TargetFile.csv file in the \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="5b7ff-222">Az üzembe helyezési folyamat futtatása után ez a fájl társított összes részleteit, és kiosztása a felhasználók és csoportok jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-222">Once the provisioning process is run, this file shows the details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="5b7ff-223">Fejlesztő függvénytárak</span><span class="sxs-lookup"><span data-stu-id="5b7ff-223">Development libraries</span></span>
<span data-ttu-id="5b7ff-224">A saját webes szolgáltatás, amely megfelel a SCIM specifikációjának elkészítéséhez először ismerkedjen meg az alábbi kódtárak Microsoft egyre gyorsabban jelennek meg a fejlesztési folyamat segítségével biztosítja:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-224">To develop your own web service that conforms to the SCIM specification, first familiarize yourself with the following libraries provided by Microsoft to help accelerate the development process:</span></span> 

1. <span data-ttu-id="5b7ff-225">Közös nyelvi infrastruktúra (CLI) szalagtárak alapján, hogy az infrastrukturális, például a C# nyelv felkínált való használatra.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="5b7ff-226">A tárak egyik [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarál illesztőfelület, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, az alábbi ábrán látható: a könyvtárak segítségével a fejlesztők egy osztály, amely lehet hivatkozni a, általános, szolgáltatóként felülettel volna megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in the following illustration:  A developer using the libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="5b7ff-227">A könyvtárak engedélyezése a fejlesztői központi telepítése egy webszolgáltatás, amely megfelel a SCIM megadását.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-227">The libraries enable the developer to deploy a web service that conforms to the SCIM specification.</span></span> <span data-ttu-id="5b7ff-228">A webszolgáltatás Internet Information Services, vagy bármilyen végrehajtható közös nyelvi infrastruktúra szerelvény vagy lehet üzemeltetni.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-228">The web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="5b7ff-229">A szolgáltató metódusok, amely a fejlesztők által az egyes identitás-tárolására való működésre volna programozott lefordítását kérelem.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-229">Request is translated into calls to the provider’s methods, which would be programmed by the developer to operate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="5b7ff-230">[Express route kezelők](http://expressjs.com/guide/routing.html) érhető el elemzés node.js kérelem objektumokból-hívások (amelyeket a SCIM specification) végrehajtott egy node.js webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by the SCIM specification), made to a node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="5b7ff-231">Egy egyéni SCIM végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="5b7ff-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="5b7ff-232">A CLI-tárakat használ, a tárak használó fejlesztők tárolhatja bármilyen végrehajtható közös nyelvi infrastruktúra szerelvényen belül, vagy az Internet Information Services belül a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-232">Using the CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="5b7ff-233">Itt látható mintakód egy végrehajtható szerelvényben, a következő címen: http://localhost:9000 szolgáltatás üzemeltetéséhez:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-233">Here is sample code for hosting a service within an executable assembly, at the address http://localhost:9000:</span></span> 

    private static void Main(string[] arguments)
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IProvider and 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
      new DevelopersMonitor();
    Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
      new DevelopersProvider(arguments[1]);
    Microsoft.SystemForCrossDomainIdentityManagement.Service webService = null;
    try
    {
        webService = new WebService(monitor, provider);
        webService.Start("http://localhost:9000");

        Console.ReadKey(true);
    }
    finally
    {
        if (webService != null)
        {
            webService.Dispose();
            webService = null;
        }
    }
    }

    public class WebService : Microsoft.SystemForCrossDomainIdentityManagement.Service
    {
    private Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor;
    private Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider;

    public WebService(
      Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitoringBehavior, 
      Microsoft.SystemForCrossDomainIdentityManagement.IProvider providerBehavior)
    {
        this.monitor = monitoringBehavior;
        this.provider = providerBehavior;
    }

    public override IMonitor MonitoringBehavior
    {
        get
        {
            return this.monitor;
        }

        set
        {
            this.monitor = value;
        }
    }

    public override IProvider ProviderBehavior
    {
        get
        {
            return this.provider;
        }

        set
        {
            this.provider = value;
        }
    }
    }

<span data-ttu-id="5b7ff-234">Ez a szolgáltatás egy HTTP címet és a kiszolgáló hitelesítési tanúsítvánnyal kell rendelkeznie, amelynek a legfelső szintű hitelesítésszolgáltató a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-234">This service must have an HTTP address and server authentication certificate of which the root certification authority is one of the following:</span></span> 

* <span data-ttu-id="5b7ff-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="5b7ff-235">CNNIC</span></span>
* <span data-ttu-id="5b7ff-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="5b7ff-236">Comodo</span></span>
* <span data-ttu-id="5b7ff-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="5b7ff-237">CyberTrust</span></span>
* <span data-ttu-id="5b7ff-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="5b7ff-238">DigiCert</span></span>
* <span data-ttu-id="5b7ff-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="5b7ff-239">GeoTrust</span></span>
* <span data-ttu-id="5b7ff-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="5b7ff-240">GlobalSign</span></span>
* <span data-ttu-id="5b7ff-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="5b7ff-241">Go Daddy</span></span>
* <span data-ttu-id="5b7ff-242">A VeriSign szolgáltatótól</span><span class="sxs-lookup"><span data-stu-id="5b7ff-242">Verisign</span></span>
* <span data-ttu-id="5b7ff-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="5b7ff-243">WoSign</span></span>

<span data-ttu-id="5b7ff-244">Olyan kiszolgálói hitelesítési tanúsítványt is kell kötve egy hálózati rendszerhéj-segédprogrammal történő Windows-gazdagépen:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-244">A server authentication certificate can be bound to a port on a Windows host using the network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="5b7ff-245">Itt certhash megadott érték a tanúsítvány ujjlenyomatát a appid argumentum a megadott érték pedig tetszőleges globálisan egyedi azonosító.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-245">Here, the value provided for the certhash argument is the thumbprint of the certificate, while the value provided for the appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="5b7ff-246">Az Internet Information Services belül a szolgáltatás futtatásához, a fejlesztő volna fejlesztheti a CLA könyvtár kódszerelvényből az egy osztályt indítási alapértelmezett névtér: a szerelvény.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-246">To host the service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in the default namespace of the assembly.</span></span>  <span data-ttu-id="5b7ff-247">Íme egy példa ilyen osztály:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-247">Here is a sample of such a class:</span></span> 

    public class Startup
    {
    // Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IMonitor and  
    // Microsoft.SystemForCrossDomainIdentityManagement.Service are all defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Service.dll.  

    Microsoft.SystemForCrossDomainIdentityManagement.IWebApplicationStarter starter;

    public Startup()
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IMonitor monitor = 
          new DevelopersMonitor();
        Microsoft.SystemForCrossDomainIdentityManagement.IProvider provider = 
          new DevelopersProvider();
        this.starter = 
          new Microsoft.SystemForCrossDomainIdentityManagement.WebApplicationStarter(
            provider, 
            monitor);
    }

    public void Configuration(
      Owin.IAppBuilder builder) // Defined in in Owin.dll.  
    {
        this.starter.ConfigureApplication(builder);
    }
    }

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="5b7ff-248">Kezelési végpont hitelesítés</span><span class="sxs-lookup"><span data-stu-id="5b7ff-248">Handling endpoint authentication</span></span>
<span data-ttu-id="5b7ff-249">Az Azure Active Directory kérések tartalmazzák az OAuth 2.0 tulajdonosi jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="5b7ff-250">Minden szolgáltatás, a kérelem fogadása hitelesítenie kell a kibocsátó, hogy az Azure Active Directory nevében a várt Azure Active Directory-bérlőt az Azure Active Directory Graph webszolgáltatás elérésére.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-250">Any service receiving the request should authenticate the issuer as being Azure Active Directory on behalf of the expected Azure Active Directory tenant, for access to the Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="5b7ff-251">A jogkivonat a kibocsátó azonosít egy iss jogcímet, például "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span><span class="sxs-lookup"><span data-stu-id="5b7ff-251">In the token, the issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="5b7ff-252">Ebben a példában a jogcím értéke alapszintű címéből https://sts.windows.net, mint a kibocsátó Azure Active Directory azonosítja a cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 relatív címet szegmens pedig az Azure Active Directory-bérlő nevében, amely a token ki egy egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-252">In this example, the base address of the claim value, https://sts.windows.net, identifies Azure Active Directory as the issuer, while the relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of the Azure Active Directory tenant on behalf of which the token was issued.</span></span>  <span data-ttu-id="5b7ff-253">Ha a jogkivonat az Azure Active Directory Graph webszolgáltatás eléréséhez adta ki, majd szolgáltatáshoz, 00000002-0000-0000-c000-000000000000 azonosítóját kell lennie a token és jogcím értéke.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-253">If the token was issued for accessing the Azure Active Directory Graph web service, then the identifier of that service, 00000002-0000-0000-c000-000000000000, should be in the value of the token’s aud claim.</span></span>  

<span data-ttu-id="5b7ff-254">A fejlesztők a SCIM szolgáltatás létrehozása a Microsoft által biztosított CLA könyvtárak segítségével hitelesítheti a kérelmeket az Azure Active Directoryból a Microsoft.Owin.Security.ActiveDirectory csomag használata a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-254">Developers using the CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using the Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="5b7ff-255">A szolgáltató megvalósíthatja a Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tulajdonság azt egy metódust kell meghívni, amikor a szolgáltatás el van indítva az vissza:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-255">In a provider, implement the Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method to be called whenever the service is started:</span></span> 

  ````
    public override Action\<Owin.IAppBuilder, System.Web.Http.HttpConfiguration.HttpConfiguration\> StartupBehavior
    {
      get
      {
        return this.OnServiceStartup;
      }
    }

    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder,  // Defined in Owin.dll.  
      System.Web.Http.HttpConfiguration configuration)  // Defined in System.Web.Http.dll.  
    {
    }
  ````

2. <span data-ttu-id="5b7ff-256">Ez a módszer minden kérelem hitelesítve szem előtt a megadott tenantot, az Azure AD Graph webszolgáltatás elérésére nevében Azure Active Directory által kiadott tokennek, a szolgáltatás végpontok bármelyikére rendelkezik hozzáadása a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-256">Add the following code to that method to have any request to any of the service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access to the Azure AD Graph web service:</span></span> 

  ````
    private void OnServiceStartup(
      Owin.IAppBuilder applicationBuilder IAppBuilder applicationBuilder, 
      System.Web.Http.HttpConfiguration HttpConfiguration configuration)
    {
      // IFilter is defined in System.Web.Http.dll.  
      System.Web.Http.Filters.IFilter authorizationFilter = 
        new System.Web.Http.AuthorizeAttribute(); // Defined in System.Web.Http.dll.configuration.Filters.Add(authorizationFilter);

      // SystemIdentityModel.Tokens.TokenValidationParameters is defined in    
      // System.IdentityModel.Token.Jwt.dll.
      SystemIdentityModel.Tokens.TokenValidationParameters tokenValidationParameters =     
        new TokenValidationParameters()
        {
          ValidAudience = "00000002-0000-0000-c000-000000000000"
        };

      // WindowsAzureActiveDirectoryBearerAuthenticationOptions is defined in 
      // Microsoft.Owin.Security.ActiveDirectory.dll
      Microsoft.Owin.Security.ActiveDirectory.
      WindowsAzureActiveDirectoryBearerAuthenticationOptions authenticationOptions =
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions()    {
        TokenValidationParameters = tokenValidationParameters,
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute the appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="5b7ff-257">Felhasználó- és séma</span><span class="sxs-lookup"><span data-stu-id="5b7ff-257">User and group schema</span></span>
<span data-ttu-id="5b7ff-258">Az Azure Active Directory kétféle típusú erőforrások SCIM webszolgáltatásokhoz építhető ki.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-258">Azure Active Directory can provision two types of resources to SCIM web services.</span></span>  <span data-ttu-id="5b7ff-259">Ilyen típusú erőforrások a felhasználók és csoportok.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="5b7ff-260">Felhasználói erőforrásokat azonosítja a sémaazonosítót urn: ietf:params:scim:schemas:extension:enterprise:2.0:User, amely szerepel a protokoll-meghatározása: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-260">User resources are identified by the schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="5b7ff-261">Az alapértelmezett leképezését a felhasználók az Azure Active Directoryban urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrások attribútumait alább tábla 1.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-261">The default mapping of the attributes of users in Azure Active Directory to the attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="5b7ff-262">Erőforrások azonosítják a sémaazonosítót http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-262">Group resources are identified by the schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="5b7ff-263">Táblázat 2, az alábbi, az alapértelmezett leképezést csoportok az Azure Active Directoryban attribútumait http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group erőforrások attribútumát.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-263">Table 2, below, shows the default mapping of the attributes of groups in Azure Active Directory to the attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="5b7ff-264">1. táblázat: Alapértelmezett felhasználói címtárattribútum-leképezésben</span><span class="sxs-lookup"><span data-stu-id="5b7ff-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="5b7ff-265">Az Azure Active Directory-felhasználó</span><span class="sxs-lookup"><span data-stu-id="5b7ff-265">Azure Active Directory user</span></span> | <span data-ttu-id="5b7ff-266">urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="5b7ff-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="5b7ff-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="5b7ff-267">IsSoftDeleted</span></span> |<span data-ttu-id="5b7ff-268">Aktív</span><span class="sxs-lookup"><span data-stu-id="5b7ff-268">active</span></span> |
| <span data-ttu-id="5b7ff-269">displayName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-269">displayName</span></span> |<span data-ttu-id="5b7ff-270">displayName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-270">displayName</span></span> |
| <span data-ttu-id="5b7ff-271">Telefax-TelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="5b7ff-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="5b7ff-272">.value phoneNumbers [típus eq "fax"]</span><span class="sxs-lookup"><span data-stu-id="5b7ff-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="5b7ff-273">givenName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-273">givenName</span></span> |<span data-ttu-id="5b7ff-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-274">name.givenName</span></span> |
| <span data-ttu-id="5b7ff-275">Beosztás</span><span class="sxs-lookup"><span data-stu-id="5b7ff-275">jobTitle</span></span> |<span data-ttu-id="5b7ff-276">Cím</span><span class="sxs-lookup"><span data-stu-id="5b7ff-276">title</span></span> |
| <span data-ttu-id="5b7ff-277">mail</span><span class="sxs-lookup"><span data-stu-id="5b7ff-277">mail</span></span> |<span data-ttu-id="5b7ff-278">e-mailek [típus eq "munkahelyi"] .value</span><span class="sxs-lookup"><span data-stu-id="5b7ff-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="5b7ff-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="5b7ff-279">mailNickname</span></span> |<span data-ttu-id="5b7ff-280">externalId</span><span class="sxs-lookup"><span data-stu-id="5b7ff-280">externalId</span></span> |
| <span data-ttu-id="5b7ff-281">Manager</span><span class="sxs-lookup"><span data-stu-id="5b7ff-281">manager</span></span> |<span data-ttu-id="5b7ff-282">Manager</span><span class="sxs-lookup"><span data-stu-id="5b7ff-282">manager</span></span> |
| <span data-ttu-id="5b7ff-283">Mobileszköz</span><span class="sxs-lookup"><span data-stu-id="5b7ff-283">mobile</span></span> |<span data-ttu-id="5b7ff-284">.value phoneNumbers [típus eq "mobileszköz"]</span><span class="sxs-lookup"><span data-stu-id="5b7ff-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="5b7ff-285">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="5b7ff-285">objectId</span></span> |<span data-ttu-id="5b7ff-286">id</span><span class="sxs-lookup"><span data-stu-id="5b7ff-286">id</span></span> |
| <span data-ttu-id="5b7ff-287">Irányítószám</span><span class="sxs-lookup"><span data-stu-id="5b7ff-287">postalCode</span></span> |<span data-ttu-id="5b7ff-288">[típus eq "munkahelyi"] címek .postalCode</span><span class="sxs-lookup"><span data-stu-id="5b7ff-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="5b7ff-289">proxy-címek</span><span class="sxs-lookup"><span data-stu-id="5b7ff-289">proxy-Addresses</span></span> |<span data-ttu-id="5b7ff-290">[Írja be az "egyéb" eq] e-maileket. Érték</span><span class="sxs-lookup"><span data-stu-id="5b7ff-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="5b7ff-291">fizikai-kézbesítés-OfficeName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="5b7ff-292">[Írja be az "egyéb" eq] címek. Formázott</span><span class="sxs-lookup"><span data-stu-id="5b7ff-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="5b7ff-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="5b7ff-293">streetAddress</span></span> |<span data-ttu-id="5b7ff-294">[típus eq "munkahelyi"] címek .streetAddress</span><span class="sxs-lookup"><span data-stu-id="5b7ff-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="5b7ff-295">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="5b7ff-295">surname</span></span> |<span data-ttu-id="5b7ff-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-296">name.familyName</span></span> |
| <span data-ttu-id="5b7ff-297">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="5b7ff-297">telephone-Number</span></span> |<span data-ttu-id="5b7ff-298">.value phoneNumbers [típus eq "munkahelyi"]</span><span class="sxs-lookup"><span data-stu-id="5b7ff-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="5b7ff-299">felhasználó-egyszerű név</span><span class="sxs-lookup"><span data-stu-id="5b7ff-299">user-PrincipalName</span></span> |<span data-ttu-id="5b7ff-300">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="5b7ff-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="5b7ff-301">2. táblázat: Alapértelmezett attribútum leképezése</span><span class="sxs-lookup"><span data-stu-id="5b7ff-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="5b7ff-302">Azure Active Directory-csoportok</span><span class="sxs-lookup"><span data-stu-id="5b7ff-302">Azure Active Directory group</span></span> | <span data-ttu-id="5b7ff-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="5b7ff-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="5b7ff-304">displayName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-304">displayName</span></span> |<span data-ttu-id="5b7ff-305">externalId</span><span class="sxs-lookup"><span data-stu-id="5b7ff-305">externalId</span></span> |
| <span data-ttu-id="5b7ff-306">mail</span><span class="sxs-lookup"><span data-stu-id="5b7ff-306">mail</span></span> |<span data-ttu-id="5b7ff-307">e-mailek [típus eq "munkahelyi"] .value</span><span class="sxs-lookup"><span data-stu-id="5b7ff-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="5b7ff-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="5b7ff-308">mailNickname</span></span> |<span data-ttu-id="5b7ff-309">displayName</span><span class="sxs-lookup"><span data-stu-id="5b7ff-309">displayName</span></span> |
| <span data-ttu-id="5b7ff-310">Tagok</span><span class="sxs-lookup"><span data-stu-id="5b7ff-310">members</span></span> |<span data-ttu-id="5b7ff-311">Tagok</span><span class="sxs-lookup"><span data-stu-id="5b7ff-311">members</span></span> |
| <span data-ttu-id="5b7ff-312">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="5b7ff-312">objectId</span></span> |<span data-ttu-id="5b7ff-313">id</span><span class="sxs-lookup"><span data-stu-id="5b7ff-313">id</span></span> |
| <span data-ttu-id="5b7ff-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="5b7ff-314">proxyAddresses</span></span> |<span data-ttu-id="5b7ff-315">[Írja be az "egyéb" eq] e-maileket. Érték</span><span class="sxs-lookup"><span data-stu-id="5b7ff-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="5b7ff-316">Felhasználói üzembe helyezést és megszüntetést</span><span class="sxs-lookup"><span data-stu-id="5b7ff-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="5b7ff-317">A következő ábra azt mutatja, hogy Azure Active Directory küld SCIM szolgáltatás egy olyan identitás-tárolóban egy másik felhasználó életciklusának kezelését az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-317">The following illustration shows the messages that Azure Active Directory sends to a SCIM service to manage the lifecycle of a user in another identity store.</span></span> <span data-ttu-id="5b7ff-318">Az ábrán is látható, hogyan a CLI könyvtárak készletével megvalósított SCIM szolgáltatás által biztosított Microsoft rendszerbeli kiépítésének e szolgáltatások ezeket a kérelmeket jelenti azt, hogy a szolgáltató metódusok.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-318">The diagram also shows how a SCIM service implemented using the CLI libraries provided by Microsoft for building such services translate those requests into calls to the methods of a provider.</span></span>  

<span data-ttu-id="5b7ff-319">![][4]
*5. ábra: A felhasználók átadása, és megszüntetést feladatütemezési*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="5b7ff-320">Az Azure Active Directory lekérdezi egy felhasználó számára a szolgáltatás az Azure AD-ben a felhasználó mailNickname attribútum értékének megfelelő externalId attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-320">Azure Active Directory queries the service for a user with an externalId attribute value matching the mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="5b7ff-321">A lekérdezés például ebben a példában, amelynek jyoung egy olyan felhasználó, az Azure Active Directoryban egy mailNickname mintát Hypertext Transfer Protocol (HTTP) kérelmet fejezi ki:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-321">The query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="5b7ff-322">Ha a szolgáltatás a Microsoft által előírt végrehajtási SCIM szolgáltatások közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd a kérést lefordítását a szolgáltató lekérdezési metódus hívásakor.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-322">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span>  <span data-ttu-id="5b7ff-323">Ez a metódus aláírása:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-323">Here is the signature of that method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Protocol.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource[]> Query(
      Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters parameters, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="5b7ff-324">Ez a Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters felület definíciója:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-324">Here is the definition of the Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
  ````
    public interface IQueryParameters: 
      Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
        System.Collections.Generic.IReadOnlyCollection <Microsoft.SystemForCrossDomainIdentityManagement.IFilter> AlternateFilters 
        { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IRetrievalParameters
    {
      system.Collections.Generic.IReadOnlyCollection<string> ExcludedAttributePaths 
      { get; }
      System.Collections.Generic.IReadOnlyCollection<string> RequestedAttributePaths 
      { get; }
      string SchemaIdentifier 
      { get; }
    }

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IFilter
    {
        Microsoft.SystemForCrossDomainIdentityManagement.IFilter AdditionalFilter 
          { get; set; }
        string AttributePath 
          { get; } 
        Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator FilterOperator 
          { get; }
        string ComparisonValue 
          { get; }
    }

    public enum Microsoft.SystemForCrossDomainIdentityManagement.ComparisonOperator
    {
        Equals
    }
  ````
  <span data-ttu-id="5b7ff-325">A következő példában a lekérdezés egy felhasználó a externalId attribútum egy megadott értékkel a lekérdezés metódusnak átadott argumentumok értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-325">In the following sample of a query for a user with a given value for the externalId attribute, values of the arguments passed to the Query method are:</span></span> 
  * <span data-ttu-id="5b7ff-326">a paraméterek. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="5b7ff-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="5b7ff-327">a paraméterek. AlternateFilters.ElementAt(0). AttributePath: "externalId"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="5b7ff-328">a paraméterek. AlternateFilters.ElementAt(0). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="5b7ff-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="5b7ff-329">a paraméterek. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="5b7ff-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Kérelemazonosító"]</span><span class="sxs-lookup"><span data-stu-id="5b7ff-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="5b7ff-331">Ha egy lekérdezést, amely megfelel a felhasználó a mailNickname attribútum externalId attribútumértékkel rendelkező felhasználó számára a webszolgáltatás válasza nem ad vissza azokat a felhasználókat, Azure Active Directory kéri, hogy a szolgáltatás kiépíteni az Azure Active Directoryban egy megfelelő felhasználó.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-331">If the response to a query to the web service for a user with an externalId attribute value that matches the mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that the service provision a user corresponding to the one in Azure Active Directory.</span></span>  <span data-ttu-id="5b7ff-332">Íme egy példa a kérelem:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-332">Here is an example of such a request:</span></span> 
  ````
    POST https://.../scim/Users HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas":
      [
        "urn:ietf:params:scim:schemas:core:2.0:User",
        "urn:ietf:params:scim:schemas:extension:enterprise:2.0User"],
      "externalId":"jyoung",
      "userName":"jyoung",
      "active":true,
      "addresses":null,
      "displayName":"Joy Young",
      "emails": [
        {
          "type":"work",
          "value":"jyoung@Contoso.com",
          "primary":true}],
      "meta": {
        "resourceType":"User"},
       "name":{
        "familyName":"Young",
        "givenName":"Joy"},
      "phoneNumbers":null,
      "preferredLanguage":null,
      "title":null,
      "department":null,
      "manager":null}
  ````
  <span data-ttu-id="5b7ff-333">A közös nyelvi infrastruktúra könyvtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított lefordítja a kérésre azokat a Create metódussal a szolgáltatás-szolgáltató hívása.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-333">The Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call to the Create method of the service’s provider.</span></span>  <span data-ttu-id="5b7ff-334">A Create metódussal az aláírása:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-334">The Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="5b7ff-335">A kérelem egy felhasználó kiépítéséhez az erőforrás argumentum értéke a Microsoft.SystemForCrossDomainIdentityManagement példánya.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-335">In a request to provision a user, the value of the resource argument is an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="5b7ff-336">A Microsoft.SystemForCrossDomainIdentityManagement.Schemas könyvtárban meghatározott Core2EnterpriseUser osztály.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-336">Core2EnterpriseUser class, defined in the Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="5b7ff-337">Ha a felhasználó létrehozásához a kérelem sikeres, majd metódus megvalósítása várhatóan térjen vissza a Microsoft.SystemForCrossDomainIdentityManagement példánya.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-337">If the request to provision the user succeeds, then the implementation of the method is expected to return an instance of the Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="5b7ff-338">Az újonnan kiépített felhasználó egyedi azonosítóját azonosító tulajdonsága értékének a Core2EnterpriseUser osztályt.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-338">Core2EnterpriseUser class, with the value of the Identifier property set to the unique identifier of the newly provisioned user.</span></span>  

3. <span data-ttu-id="5b7ff-339">Egy felhasználó által egy SCIM fronted identitás tárolóban található ismert frissítéséhez, Azure Active Directory folytatja úgy, hogy a felhasználó aktuális állapotának kér a szolgáltatás a kéréshez, többek között:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-339">To update a user known to exist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting the current state of that user from the service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="5b7ff-340">Használatával a közös nyelvi infrastruktúra könyvtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított beépített szolgáltatás a kérelem a lekérése metódus a szolgáltató van lefordítva.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-340">In a service built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, the request is translated into a call to the Retrieve method of the service’s provider.</span></span>  <span data-ttu-id="5b7ff-341">A lekérési metódus aláírása a következő:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-341">Here is the signature of the Retrieve method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource and 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
    // are defined in Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  
    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> 
       Retrieve(
         Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters 
           parameters, 
           string correlationIdentifier);

    public interface 
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceRetrievalParameters:   
        IRetrievalParameters
        {
          Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
            ResourceIdentifier 
              { get; }
    }
    public interface Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier
    {
        string Identifier 
          { get; set; }
        string Microsoft.SystemForCrossDomainIdentityManagement.SchemaIdentifier 
          { get; set; }
    }
  ````
  <span data-ttu-id="5b7ff-342">A példa egy kérelem a felhasználó aktuális állapotának beolvasására, a paraméterek argumentum értéke a megadott objektum tulajdonságainak értékei a következők:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-342">In the example of a request to retrieve the current state of a user, the values of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="5b7ff-343">Azonosító: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="5b7ff-344">SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="5b7ff-345">Ha a hivatkozási attribútum frissíteni kell, majd az Azure Active Directory-e a hivatkozási attribútum identitás tárolójában aktuális értékének fronted a szolgáltatás már meghatározni a szolgáltatás lekérdezi az Azure Active Directoryban ez az attribútum értéke megegyezik.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-345">If a reference attribute is to be updated, then Azure Active Directory queries the service to determine whether or not the current value of the reference attribute in the identity store fronted by the service already matches the value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="5b7ff-346">Felhasználók a, amelyek a jelenlegi érték a ily módon le kell kérdezni attribútum esetén a kezelő attribútum.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-346">For users, the only attribute of which the current value is queried in this way is the manager attribute.</span></span> <span data-ttu-id="5b7ff-347">Íme egy példa egy kérelem annak meghatározásához, hogy a kezelő egy adott felhasználó objektum attribútuma van a megadott érték:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-347">Here is an example of a request to determine whether the manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="5b7ff-348">Az érték a attribútumok lekérdezési paraméter azonosítója, azt jelzi, hogy, hogy ha egy felhasználói objektum, amely eleget tesz a kifejezést a szűrő lekérdezési paraméter értéke, akkor a szolgáltatás várhatóan urn: ietf:params:scim:schemas:core:2.0:User vagy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrás, beleértve az adott erőforrás id attribútum értéke csak válaszolni.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-348">The value of the attributes query parameter, id, signifies that if a user object exists that satisfies the expression provided as the value of the filter query parameter, then the service is expected to respond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only the value of that resource’s id attribute.</span></span>  <span data-ttu-id="5b7ff-349">Értékét a **azonosító** a kérelmező ismert attribútum.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-349">The value of the **id** attribute is known to the requestor.</span></span> <span data-ttu-id="5b7ff-350">A szűrő lekérdezési paraméter; érték szerepel. az azt kérő célja ténylegesen kérelmet a minimális erőforrás megjelenítése felel meg a szűrési kifejezés arra utal, hogy az összes ilyen objektum létezik-e.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-350">It is included in the value of the filter query parameter; the purpose of asking for it is actually to request a minimal representation of a resource that satisfying the filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="5b7ff-351">Ha a szolgáltatás a Microsoft által előírt végrehajtási SCIM szolgáltatások közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd a kérést lefordítását a szolgáltató lekérdezési metódus hívásakor.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-351">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Query method of the service’s provider.</span></span> <span data-ttu-id="5b7ff-352">A paraméterek argumentumnak az értékeként megadott objektum tulajdonságainak értékének a következők:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-352">The value of the properties of the object provided as the value of the parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="5b7ff-353">a paraméterek. AlternateFilters.Count: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="5b7ff-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="5b7ff-354">a paraméterek. AlternateFilters.ElementAt(x). AttributePath: "id"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="5b7ff-355">a paraméterek. AlternateFilters.ElementAt(x). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="5b7ff-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="5b7ff-356">a paraméterek. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="5b7ff-357">a paraméterek. AlternateFilters.ElementAt(y). AttributePath: "manager"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="5b7ff-358">a paraméterek. AlternateFilters.ElementAt(y). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="5b7ff-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="5b7ff-359">a paraméterek. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="5b7ff-360">a paraméterek. RequestedAttributePaths.ElementAt(0): "id"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="5b7ff-361">a paraméterek. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="5b7ff-362">Itt lehet, hogy az index x értékének 0 és lehet, hogy az index y értéke 1, vagy lehet, hogy az x értéknek 1 és y értékének lehet 0, attól függően, hogy a szűrő lekérdezési paraméter kifejezések sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-362">Here, the value of the index x may be 0 and the value of the index y may be 1, or the value of x may be 1 and the value of y may be 0, depending on the order of the expressions of the filter query parameter.</span></span>   

5. <span data-ttu-id="5b7ff-363">Itt látható egy példa egy kérelem az Azure Active Directory egy felhasználó frissítéséhez egy SCIM szolgáltatáshoz:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-363">Here is an example of a request from Azure Active Directory to an SCIM service to update a user:</span></span> 
  ````
    PATCH ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
    Content-type: application/json
    {
      "schemas": 
      [
        "urn:ietf:params:scim:api:messages:2.0:PatchOp"],
      "Operations":
      [
        {
          "op":"Add",
          "path":"manager",
          "value":
            [
              {
                "$ref":"http://.../scim/Users/2819c223-7f76-453a-919d-413861904646",
                "value":"2819c223-7f76-453a-919d-413861904646"}]}]}
  ````
  <span data-ttu-id="5b7ff-364">A Microsoft közös nyelvi infrastruktúra-könyvtárakban SCIM szolgáltatások végrehajtási lefordítja a kérelem azokat a szolgáltató az Update metódus hívásakor.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-364">The Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate the request into a call to the Update method of the service’s provider.</span></span> <span data-ttu-id="5b7ff-365">Az Update metódus aláírása a következő:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-365">Here is the signature of the Update method:</span></span> 
  ````
    // System.Threading.Tasks.Tasks and 
    // System.Collections.Generic.IReadOnlyCollection<T>
    // are defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IPatch, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation, 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationName, 
    // Microsoft.SystemForCrossDomainIdentityManagement.IPath and 
    // Microsoft.SystemForCrossDomainIdentityManagement.OperationValue 
    // are all defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 

    System.Threading.Tasks.Task Update(
      Microsoft.SystemForCrossDomainIdentityManagement.IPatch patch, 
      string correlationIdentifier);

    public interface Microsoft.SystemForCrossDomainIdentityManagement.IPatch
    {
    Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase 
      PatchRequest 
        { get; set; }
    Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier 
      ResourceIdentifier 
        { get; set; }        
    }

    public class PatchRequest2: 
      Microsoft.SystemForCrossDomainIdentityManagement.PatchRequestBase
    {
    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation> 
        Operations
        { get;}

    public void AddOperation(
      Microsoft.SystemForCrossDomainIdentityManagement.PatchOperation operation);
    }

    public class PatchOperation
    {
    public Microsoft.SystemForCrossDomainIdentityManagement.OperationName 
      Name
      { get; set; }

    public Microsoft.SystemForCrossDomainIdentityManagement.IPath 
      Path
      { get; set; }

    public System.Collections.Generic.IReadOnlyCollection
      <Microsoft.SystemForCrossDomainIdentityManagement.OperationValue> Value
      { get; }

    public void AddValue(
      Microsoft.SystemForCrossDomainIdentityManagement.OperationValue value);
    }

    public enum OperationName
    {
      Add,
      Remove,
      Replace
    }

    public interface IPath
    {
      string AttributePath { get; }
      System.Collections.Generic.IReadOnlyCollection<IFilter> SubAttributes { get; }
      Microsoft.SystemForCrossDomainIdentityManagement.IPath ValuePath { get; }
    }

    public class OperationValue
    {
      public string Reference
      { get; set; }

      public string Value
      { get; set; }
    }
  ````
    <span data-ttu-id="5b7ff-366">A példa egy kérelem egy felhasználó frissítéséhez a javítás argumentum értékeként megadott objektumnak a tulajdonságok értékeit:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-366">In the example of a request to update a user, the object provided as the value of the patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="5b7ff-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="5b7ff-368">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="5b7ff-369">(Mint PatchRequest2 PatchRequest). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="5b7ff-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="5b7ff-370">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="5b7ff-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="5b7ff-371">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Path.AttributePath: "manager"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="5b7ff-372">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="5b7ff-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="5b7ff-373">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Hivatkozás: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="5b7ff-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="5b7ff-374">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Érték: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="5b7ff-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="5b7ff-375">Hogy leépíti a felhasználó SCIM szolgáltatása fronted identitás áruházban, az Azure AD egy kérést küld, mint:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-375">To de-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="5b7ff-376">Ha a szolgáltatás a Microsoft által előírt végrehajtási SCIM szolgáltatások közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd a kérést lefordítását a szolgáltató a Delete metódus hívásakor.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-376">If the service was built using the Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then the request is translated into a call to the Delete method of the service’s provider.</span></span>   <span data-ttu-id="5b7ff-377">Ez a módszer az aláírása:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="5b7ff-378">A resourceIdentifier argumentumnak az értékeként megadott objektum leépíti a felhasználó kérést példájában a tulajdonságok értékeit az rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-378">The object provided as the value of the resourceIdentifier argument has these property values in the example of a request to de-provision a user:</span></span> 
  
  * <span data-ttu-id="5b7ff-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="5b7ff-380">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="5b7ff-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="5b7ff-381">Csoport üzembe helyezést és megszüntetést</span><span class="sxs-lookup"><span data-stu-id="5b7ff-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="5b7ff-382">A következő ábra azt mutatja, hogy Azure AcD küld a SCIM szolgáltatásnak csoportnak egy másik identitás tárolására az életciklus kezeléséhez az üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-382">The following illustration shows the messages that Azure AcD sends to a SCIM service to manage the lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="5b7ff-383">Az üzenetek a felhasználók háromféleképpen vonatkozó üzeneteket különböznek:</span><span class="sxs-lookup"><span data-stu-id="5b7ff-383">Those messages differ from the messages pertaining to users in three ways:</span></span> 

* <span data-ttu-id="5b7ff-384">Egy csoport erőforrás sémája http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group azonosítja.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-384">The schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="5b7ff-385">Csoportok beolvasására irányuló kérelmek határozzák meg, hogy a tagok attribútum ki lesznek zárva a bármilyen olyan erőforrás található kérelemre adott válasz.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-385">Requests to retrieve groups stipulate that the members attribute is to be excluded from any resource provided in response to the request.</span></span>  
* <span data-ttu-id="5b7ff-386">Annak meghatározásához, hogy rendelkezik-e a hivatkozási attribútum egy adott értékre kérelmek azok a tagok attribútum kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="5b7ff-386">Requests to determine whether a reference attribute has a certain value are requests about the members attribute.</span></span>  

<span data-ttu-id="5b7ff-387">![][5]
*6. ábra: Az üzembe helyezést és megszüntetést feladatütemezési csoport*</span><span class="sxs-lookup"><span data-stu-id="5b7ff-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="5b7ff-388">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="5b7ff-388">Related articles</span></span>
* [<span data-ttu-id="5b7ff-389">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="5b7ff-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="5b7ff-390">Felhasználói létesítési vagy megszüntetési SaaS-alkalmazásokhoz való automatizálásához</span><span class="sxs-lookup"><span data-stu-id="5b7ff-390">Automate User Provisioning/Deprovisioning to SaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="5b7ff-391">A felhasználók átadása attribútum-leképezésekhez testreszabása</span><span class="sxs-lookup"><span data-stu-id="5b7ff-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="5b7ff-392">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="5b7ff-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="5b7ff-393">Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="5b7ff-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="5b7ff-394">Alkalmazás-kiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="5b7ff-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="5b7ff-395">SaaS-alkalmazások integrációjával kapcsolatos bemutatók felsorolása</span><span class="sxs-lookup"><span data-stu-id="5b7ff-395">List of Tutorials on How to Integrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
