---
title: "Tartományok közötti identitáskezeléshez rendszer aaaUsing automatikusan létesítsen felhasználók és csoportok az Azure Active Directory tooapplications |} Microsoft Docs"
description: "Az Azure Active Directory-felhasználók és csoportok tooany alkalmazás vagy identitás tároló hello felületen meghatározott hello SCIM protokoll-meghatározása a webszolgáltatás által az fronted automatikusan telepíthetik"
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
ms.openlocfilehash: 43045c97e68d0d22db598dcb5ec23481c4e97718
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-system-for-cross-domain-identity-management-tooautomatically-provision-users-and-groups-from-azure-active-directory-tooapplications"></a><span data-ttu-id="91e59-103">Tartományok közötti Identity Management tooautomatically rendelkezés felhasználókhoz és csoportokhoz az Azure Active Directory tooapplications használatával</span><span class="sxs-lookup"><span data-stu-id="91e59-103">Using System for Cross-Domain Identity Management tooautomatically provision users and groups from Azure Active Directory tooapplications</span></span>

## <a name="overview"></a><span data-ttu-id="91e59-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="91e59-104">Overview</span></span>
<span data-ttu-id="91e59-105">Azure Active Directory (Azure AD) automatikusan telepíthetik a felhasználók és csoportok tooany alkalmazás vagy identitás tároló, amely a webszolgáltatás által meghatározott hello hello felülettel fronted van [rendszer tartományok közötti Identity Management (SCIM) 2.0 protokoll specifikációja](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span><span class="sxs-lookup"><span data-stu-id="91e59-105">Azure Active Directory (Azure AD) can automatically provision users and groups tooany application or identity store that is fronted by a web service with hello interface defined in hello [System for Cross-Domain Identity Management (SCIM) 2.0 protocol specification](https://tools.ietf.org/html/draft-ietf-scim-api-19).</span></span> <span data-ttu-id="91e59-106">Az Azure Active Directory küldési kérelmek toocreate, módosítása vagy törlése a kijelölt felhasználók és csoportok toohello webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="91e59-106">Azure Active Directory can send requests toocreate, modify, or delete assigned users and groups toohello web service.</span></span> <span data-ttu-id="91e59-107">hello webszolgáltatás majd is ezeket a kérelmeket, lefordítása hello cél identitás tárolására műveleteket.</span><span class="sxs-lookup"><span data-stu-id="91e59-107">hello web service can then translate those requests into operations on hello target identity store.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="91e59-108">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="91e59-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 



<span data-ttu-id="91e59-109">![][0]
*1. ábra: Azure Active Directory tooan identitás tárolására a webszolgáltatáson keresztül a kiépítés*</span><span class="sxs-lookup"><span data-stu-id="91e59-109">![][0]
*Figure 1: Provisioning from Azure Active Directory tooan identity store via a web service*</span></span>

<span data-ttu-id="91e59-110">Ez a funkció hello "állapotba hozása a saját alkalmazás" lehetőséget az Azure AD tooenable egyszeri bejelentkezést és automatikus kiépítést alkalmazásokat, amelyek adja meg, vagy amelyek fronted SCIM webszolgáltatás által a felhasználói együtt is használható.</span><span class="sxs-lookup"><span data-stu-id="91e59-110">This capability can be used in conjunction with hello “bring your own app” capability in Azure AD tooenable single sign-on and automatic user provisioning for applications that provide or are fronted by a SCIM web service.</span></span>

<span data-ttu-id="91e59-111">Számos két alkalmazási helyzetei SCIM használata az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="91e59-111">There are two use cases for using SCIM in Azure Active Directory:</span></span>

* <span data-ttu-id="91e59-112">**Felhasználók és csoportok tooapplications SCIM támogató kiépítés** alkalmazásokat, amelyek támogatják az SCIM 2.0 és OAuth tulajdonosi jogkivonatok használnak, a hitelesítés az Azure AD konfigurálása nélkül működik.</span><span class="sxs-lookup"><span data-stu-id="91e59-112">**Provisioning users and groups tooapplications that support SCIM** Applications that support SCIM 2.0 and use OAuth bearer tokens for authentication works with Azure AD without configuration.</span></span>
* <span data-ttu-id="91e59-113">**Saját kiépítési megoldás az alkalmazások, amelyek támogatják a többi API-alapú üzembe helyezés** nem SCIM alkalmazások, létrehozhat egy SCIM végpont tootranslate hello Azure AD SCIM végpont, valamint esetleges API hello alkalmazás között a felhasználók átadása.</span><span class="sxs-lookup"><span data-stu-id="91e59-113">**Build your own provisioning solution for applications that support other API-based provisioning** For non-SCIM applications, you can create a SCIM endpoint tootranslate between hello Azure AD SCIM endpoint and any API hello application supports for user provisioning.</span></span> <span data-ttu-id="91e59-114">toohelp egy SCIM végpont fejleszt, nyújtunk a közös nyelvi infrastruktúra (CLI) szalagtárak mintakódok, amelyek bemutatják, hogyan toodo adjon meg egy SCIM végpontot, és SCIM üzenetek fordítása együtt.</span><span class="sxs-lookup"><span data-stu-id="91e59-114">toohelp you develop a SCIM endpoint, we provide Common Language Infrastructure (CLI) libraries along with code samples that show you how toodo provide a SCIM endpoint and translate SCIM messages.</span></span>  

## <a name="provisioning-users-and-groups-tooapplications-that-support-scim"></a><span data-ttu-id="91e59-115">Felhasználók és csoportok tooapplications SCIM támogató kiépítése</span><span class="sxs-lookup"><span data-stu-id="91e59-115">Provisioning users and groups tooapplications that support SCIM</span></span>
<span data-ttu-id="91e59-116">Az Azure AD lehet konfigurált tooautomatically rendelkezés hozzárendelt felhasználók és csoportok tooapplications, amelyek megvalósítják a [tartományok közötti identitáskezeléshez 2 (SCIM) rendszer](https://tools.ietf.org/html/draft-ietf-scim-api-19) webes szolgáltatás, és fogadja el az OAuth tulajdonosi jogkivonatokat a hitelesítéshez .</span><span class="sxs-lookup"><span data-stu-id="91e59-116">Azure AD can be configured tooautomatically provision assigned users and groups tooapplications that implement a [System for Cross-domain Identity Management 2 (SCIM)](https://tools.ietf.org/html/draft-ietf-scim-api-19) web service and accept OAuth bearer tokens for authentication.</span></span> <span data-ttu-id="91e59-117">Belül hello SCIM 2.0 specifikációját alkalmazások követelménynek kell megfelelnie:</span><span class="sxs-lookup"><span data-stu-id="91e59-117">Within hello SCIM 2.0 specification, applications must meet these requirements:</span></span>

* <span data-ttu-id="91e59-118">Támogatja az felhasználói és/vagy csoportok szerint hello SCIM protokoll 3.3 szakasza létrehozását.</span><span class="sxs-lookup"><span data-stu-id="91e59-118">Supports creating users and/or groups, as per section 3.3 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="91e59-119">Támogatja a felhasználók és/vagy csoportok szerint hello SCIM protokoll 3.5.2 szakasza a patch kéréseknek módosítása.</span><span class="sxs-lookup"><span data-stu-id="91e59-119">Supports modifying users and/or groups with patch requests as per section 3.5.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="91e59-120">Támogatja az adott hello SCIM protokoll 3.4.1 szakasza ismert erőforrás beolvasása.</span><span class="sxs-lookup"><span data-stu-id="91e59-120">Supports retrieving a known resource as per section 3.4.1 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="91e59-121">Támogatja a felhasználók és/vagy csoportok szerint hello SCIM protokoll 3.4.2 szakasza lekérdezése.</span><span class="sxs-lookup"><span data-stu-id="91e59-121">Supports querying users and/or groups, as per section 3.4.2 of hello SCIM protocol.</span></span>  <span data-ttu-id="91e59-122">Alapértelmezés szerint externalId rendszer megkérdezi a felhasználókat és csoportokat a rendszer megkérdezi a displayName által.</span><span class="sxs-lookup"><span data-stu-id="91e59-122">By default, users are queried by externalId and groups are queried by displayName.</span></span>  
* <span data-ttu-id="91e59-123">Felhasználói azonosító és a kezelő szakaszában 3.4.2 hello SCIM protokoll szerinti lekérdezése támogatja.</span><span class="sxs-lookup"><span data-stu-id="91e59-123">Supports querying user by ID and by manager as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="91e59-124">Azonosító és a tag hello SCIM protokoll 3.4.2 szakaszában szereplő csoportok lekérdezése támogatja.</span><span class="sxs-lookup"><span data-stu-id="91e59-124">Supports querying groups by ID and by member as per section 3.4.2 of hello SCIM protocol.</span></span>  
* <span data-ttu-id="91e59-125">Hello SCIM protokoll szerinti szakasz 2.1 engedélyezését OAuth tulajdonosi jogkivonatokat fogad el.</span><span class="sxs-lookup"><span data-stu-id="91e59-125">Accepts OAuth bearer tokens for authorization as per section 2.1 of hello SCIM protocol.</span></span>

<span data-ttu-id="91e59-126">Kérje meg az alkalmazás-szolgáltatót, és ezeket a követelményeket is kompatibilisek állapotkimutatások az alkalmazás szolgáltató dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="91e59-126">Check with your application provider, or your application provider's documentation for statements of compatibility with these requirements.</span></span>

### <a name="getting-started"></a><span data-ttu-id="91e59-127">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="91e59-127">Getting started</span></span>
<span data-ttu-id="91e59-128">Ebben a cikkben leírt hello SCIM profil támogató alkalmazások lehet csatlakoztatott tooAzure Active Directory hello Azure AD application gallery a "nem galéria alkalmazás" hello funkciójával.</span><span class="sxs-lookup"><span data-stu-id="91e59-128">Applications that support hello SCIM profile described in this article can be connected tooAzure Active Directory using hello "non-gallery application" feature in hello Azure AD application gallery.</span></span> <span data-ttu-id="91e59-129">Csatlakoztatott, az Azure AD futtatása után 20 percenként ahol hello alkalmazás SCIM végpontja lekéri a szinkronizálási folyamat hozzárendelt felhasználókat és csoportokat, és hoz létre, vagy módosítja őket szerint toohello hozzárendelés részletei.</span><span class="sxs-lookup"><span data-stu-id="91e59-129">Once connected, Azure AD runs a synchronization process every 20 minutes where it queries hello application's SCIM endpoint for assigned users and groups, and creates or modifies them according toohello assignment details.</span></span>

<span data-ttu-id="91e59-130">**SCIM támogató alkalmazás tooconnect:**</span><span class="sxs-lookup"><span data-stu-id="91e59-130">**tooconnect an application that supports SCIM:**</span></span>

1. <span data-ttu-id="91e59-131">Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="91e59-131">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="91e59-132">Keresse meg a túl ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="91e59-132">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="91e59-133">Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikon toocreate app objektum.</span><span class="sxs-lookup"><span data-stu-id="91e59-133">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span>
    
  <span data-ttu-id="91e59-134">![][1]
  *2. ábra: Az Azure AD application gallery*</span><span class="sxs-lookup"><span data-stu-id="91e59-134">![][1]
*Figure 2: Azure AD application gallery*</span></span>
    
4. <span data-ttu-id="91e59-135">Az eredményül kapott üdvözlő képernyőt, válassza ki a hello **kiépítési** lapon hello bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="91e59-135">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="91e59-136">A hello **kiépítési üzemmódban** menü **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="91e59-136">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="91e59-137">![][2]
  *3. ábra: Konfigurálása kiosztás a hello Azure-portálon*</span><span class="sxs-lookup"><span data-stu-id="91e59-137">![][2]
*Figure 3: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="91e59-138">A hello **bérlői URL-cím** mezőbe írja be a hello hello alkalmazás SCIM végpont URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="91e59-138">In hello **Tenant URL** field, enter hello URL of hello application's SCIM endpoint.</span></span> <span data-ttu-id="91e59-139">Példa: https://api.contoso.com/scim/v2/</span><span class="sxs-lookup"><span data-stu-id="91e59-139">Example: https://api.contoso.com/scim/v2/</span></span>
7. <span data-ttu-id="91e59-140">Ha hello SCIM végpont megköveteli az OAuth tulajdonosi jogkivonat nem az Azure AD egy kiállítótól érkező, akkor a Másolás hello szükséges OAuth tulajdonosi jogkivonattal történő választható hello **titkos Token** mező.</span><span class="sxs-lookup"><span data-stu-id="91e59-140">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="91e59-141">Ez a mező üresen marad, ha az Azure AD az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="91e59-141">If this field is left blank, then Azure AD included an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="91e59-142">Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.</span><span class="sxs-lookup"><span data-stu-id="91e59-142">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="91e59-143">Kattintson a hello **kapcsolat tesztelése** gomb toohave Azure Active Directory kísérlet tooconnect toohello SCIM végpont.</span><span class="sxs-lookup"><span data-stu-id="91e59-143">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="91e59-144">Ha hello kísérlet sikertelen, hiba információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="91e59-144">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="91e59-145">Ha hello kísérletek tooconnect toohello alkalmazás sikeres legyen, majd kattintson **mentése** toosave hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="91e59-145">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="91e59-146">A hello **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak.</span><span class="sxs-lookup"><span data-stu-id="91e59-146">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="91e59-147">Minden egyes egy tooreview hello attribútumokat szinkronizált Azure Active Directory tooyour app választja ki.</span><span class="sxs-lookup"><span data-stu-id="91e59-147">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="91e59-148">kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználókat és csoportokat a frissítési műveletek az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="91e59-148">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="91e59-149">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="91e59-149">Select hello Save button toocommit any changes.</span></span>

    >[!NOTE]
    ><span data-ttu-id="91e59-150">Igény szerint letilthatja hello "csoportok" leképezési letiltásával objektumok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="91e59-150">You can optionally disable syncing of group objects by disabling hello "groups" mapping.</span></span> 

11. <span data-ttu-id="91e59-151">A **beállítások**, hello **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="91e59-151">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="91e59-152">Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálva lesz a hello rendelt felhasználók és csoportok **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="91e59-152">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="91e59-153">A konfiguráció befejezése után módosítsa a hello **kiépítési állapot** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="91e59-153">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="91e59-154">Kattintson a **mentése** toostart hello Azure AD létesítési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="91e59-154">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="91e59-155">Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), lehet, hogy tooselect hello **felhasználók és csoportok** lapra, és rendeljen hello felhasználók és/vagy csoportok toosync kívánja.</span><span class="sxs-lookup"><span data-stu-id="91e59-155">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="91e59-156">Miután hello kezdeti szinkronizálás elindult, használhatja a hello **naplók** lapon toomonitor folyamatban, amely mutatja az alkalmazás a szolgáltatás kiépítését hello által végzett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="91e59-156">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="91e59-157">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="91e59-157">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

>[!NOTE]
><span data-ttu-id="91e59-158">hello kezdeti szinkronizálás hosszabb, mint bekövetkező körülbelül 20 percenként, mindaddig, amíg hello szolgáltatás fut. ezt követő szinkronizálások tooperform vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="91e59-158">hello initial sync takes longer tooperform than subsequent syncs, which occur approximately every 20 minutes as long as hello service is running.</span></span> 


## <a name="building-your-own-provisioning-solution-for-any-application"></a><span data-ttu-id="91e59-159">Bármely alkalmazás a saját kiépítési megoldás létrehozása</span><span class="sxs-lookup"><span data-stu-id="91e59-159">Building your own provisioning solution for any application</span></span>
<span data-ttu-id="91e59-160">Hozzon létre egy SCIM webszolgáltatás-bővítmény is eltárolni, amely az Azure Active Directoryval, engedélyezheti a egyetlen bejelentkezést és automatikus felhasználólétesítés szinte bármilyen alkalmazáshoz, amely a kiépítés API REST vagy SOAP felhasználó biztosít.</span><span class="sxs-lookup"><span data-stu-id="91e59-160">By creating a SCIM web service that interfaces with Azure Active Directory, you can enable single sign-on and automatic user provisioning for virtually any application that provides a REST or SOAP user provisioning API.</span></span>

<span data-ttu-id="91e59-161">Itt látható, hogyan működik:</span><span class="sxs-lookup"><span data-stu-id="91e59-161">Here’s how it works:</span></span>

1. <span data-ttu-id="91e59-162">Az Azure AD biztosít a közös nyelvi infrastruktúra szalagtár nevű [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span><span class="sxs-lookup"><span data-stu-id="91e59-162">Azure AD provides a common language infrastructure library named [Microsoft.SystemForCrossDomainIdentityManagement](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/).</span></span> <span data-ttu-id="91e59-163">Rendszerintegrátorok és a fejlesztők a szalagtár toocreate használja, és csatlakozni tudnak az Azure AD tooany alkalmazás identitás tárolására SCIM-alapú webes szolgáltatásvégpont telepítése.</span><span class="sxs-lookup"><span data-stu-id="91e59-163">System integrators and developers can use this library toocreate and deploy a SCIM-based web service endpoint capable of connecting Azure AD tooany application’s identity store.</span></span>
2. <span data-ttu-id="91e59-164">Hozzárendelések hello web service toomap hello szabványos felhasználói séma toohello felhasználói séma és a protokoll hello alkalmazáshoz szükséges valósíthatók meg.</span><span class="sxs-lookup"><span data-stu-id="91e59-164">Mappings are implemented in hello web service toomap hello standardized user schema toohello user schema and protocol required by hello application.</span></span>
3. <span data-ttu-id="91e59-165">hello végponti URL-cím regisztrálva van az Azure AD egy egyéni alkalmazást a hello alkalmazáskatalógusában részeként.</span><span class="sxs-lookup"><span data-stu-id="91e59-165">hello endpoint URL is registered in Azure AD as part of a custom application in hello application gallery.</span></span>
4. <span data-ttu-id="91e59-166">Felhasználók és csoportok vannak hozzárendelve toothis alkalmazás az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="91e59-166">Users and groups are assigned toothis application in Azure AD.</span></span> <span data-ttu-id="91e59-167">Hozzárendelés, hogy a várólista szinkronizált toobe toohello cél alkalmazásba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="91e59-167">Upon assignment, they are put into a queue toobe synchronized toohello target application.</span></span> <span data-ttu-id="91e59-168">hello szinkronizálási folyamat hello várólista kezelése 20 percenként fut.</span><span class="sxs-lookup"><span data-stu-id="91e59-168">hello synchronization process handling hello queue runs every 20 minutes.</span></span>

### <a name="code-samples"></a><span data-ttu-id="91e59-169">Kódminták</span><span class="sxs-lookup"><span data-stu-id="91e59-169">Code Samples</span></span>
<span data-ttu-id="91e59-170">toomake ez feldolgozása könnyebb, [Kódminták](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) feltéve, hogy hozzon létre egy SCIM webszolgáltatási végpontot, és mutassa be, az Automatikus kiépítés.</span><span class="sxs-lookup"><span data-stu-id="91e59-170">toomake this process easier, a set of [code samples](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master) are provided that create a SCIM web service endpoint and demonstrate automatic provisioning.</span></span> <span data-ttu-id="91e59-171">Egy minta megtartja sorait a CSV-felhasználók és csoportok közti szolgáltatóra van.</span><span class="sxs-lookup"><span data-stu-id="91e59-171">One sample is of a provider that maintains a file with rows of comma-separated values representing users and groups.</span></span>  <span data-ttu-id="91e59-172">más hello hello Amazon Web Services identitás és hozzáférés-kezelés szolgáltatás működik szolgáltatóra van.</span><span class="sxs-lookup"><span data-stu-id="91e59-172">hello other is of a provider that operates on hello Amazon Web Services Identity and Access Management service.</span></span>  

<span data-ttu-id="91e59-173">**Előfeltételek**</span><span class="sxs-lookup"><span data-stu-id="91e59-173">**Prerequisites**</span></span>

* <span data-ttu-id="91e59-174">A Visual Studio 2013 vagy újabb verzió</span><span class="sxs-lookup"><span data-stu-id="91e59-174">Visual Studio 2013 or later</span></span>
* [<span data-ttu-id="91e59-175">Azure SDK for .NET</span><span class="sxs-lookup"><span data-stu-id="91e59-175">Azure SDK for .NET</span></span>](https://azure.microsoft.com/downloads/)
* <span data-ttu-id="91e59-176">Windows-számítógép, amely támogatja az ASP.NET-keretrendszer 4.5-ös toobe hello használt hello SCIM végpont.</span><span class="sxs-lookup"><span data-stu-id="91e59-176">Windows machine that supports hello ASP.NET framework 4.5 toobe used as hello SCIM endpoint.</span></span> <span data-ttu-id="91e59-177">Ezen a számítógépen a hello felhőből elérhetőnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="91e59-177">This machine must be accessible from hello cloud</span></span>
* [<span data-ttu-id="91e59-178">Prémium szintű Azure AD egy próba- vagy licencelt verziójával egy Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="91e59-178">An Azure subscription with a trial or licensed version of Azure AD Premium</span></span>](https://azure.microsoft.com/services/active-directory/)
* <span data-ttu-id="91e59-179">hello Amazon AWS igényel könyvtárakat a hello [AWS eszközkészlet a Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span><span class="sxs-lookup"><span data-stu-id="91e59-179">hello Amazon AWS sample requires libraries from hello [AWS Toolkit for Visual Studio](http://docs.aws.amazon.com/AWSToolkitVS/latest/UserGuide/tkv_setup.html).</span></span> <span data-ttu-id="91e59-180">További információkért lásd: hello információs fájl hello minta.</span><span class="sxs-lookup"><span data-stu-id="91e59-180">For more information, see hello README file included with hello sample.</span></span>

### <a name="getting-started"></a><span data-ttu-id="91e59-181">Első lépések</span><span class="sxs-lookup"><span data-stu-id="91e59-181">Getting Started</span></span>
<span data-ttu-id="91e59-182">hello egy SCIM kiépítés elfogadó kérelmeket az Azure AD legegyszerűbb módja tooimplement toobuild, telepítse a hello kódminta, amely hello kiosztott felhasználók tooa vesszővel tagolt (CSV) fájl.</span><span class="sxs-lookup"><span data-stu-id="91e59-182">hello easiest way tooimplement a SCIM endpoint that can accept provisioning requests from Azure AD is toobuild and deploy hello code sample that outputs hello provisioned users tooa comma-separated value (CSV) file.</span></span>

<span data-ttu-id="91e59-183">**a minta SCIM végpont toocreate:**</span><span class="sxs-lookup"><span data-stu-id="91e59-183">**toocreate a sample SCIM endpoint:**</span></span>

1. <span data-ttu-id="91e59-184">Hello kód a minta-csomag letöltése [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span><span class="sxs-lookup"><span data-stu-id="91e59-184">Download hello code sample package at [https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master](https://github.com/Azure/AzureAD-BYOA-Provisioning-Samples/tree/master)</span></span>
2. <span data-ttu-id="91e59-185">Bontsa ki a hello csomagot, és helyezze el a Windows-számítógép C:\AzureAD-BYOA-Provisioning-Samples\ például egy helyen.</span><span class="sxs-lookup"><span data-stu-id="91e59-185">Unzip hello package and place it on your Windows machine at a location such as C:\AzureAD-BYOA-Provisioning-Samples\.</span></span>
3. <span data-ttu-id="91e59-186">Ebben a mappában nyissa meg a Visual Studio hello FileProvisioningAgent megoldás.</span><span class="sxs-lookup"><span data-stu-id="91e59-186">In this folder, launch hello FileProvisioningAgent solution in Visual Studio.</span></span>
4. <span data-ttu-id="91e59-187">Válassza ki **eszközök > Kódtárcsomag-kezelő > Csomagkezelő konzol**, és hajtsa végre a következő parancsok hello FileProvisioningAgent projekt tooresolve hello megoldás hivatkozásainak hello:</span><span class="sxs-lookup"><span data-stu-id="91e59-187">Select **Tools > Library Package Manager > Package Manager Console**, and execute hello following commands for hello FileProvisioningAgent project tooresolve hello solution references:</span></span>
  ```` 
   Install-Package Microsoft.SystemForCrossDomainIdentityManagement
   Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
   Install-Package Microsoft.Owin.Diagnostics
   Install-Package Microsoft.Owin.Host.SystemWeb
  ````
5. <span data-ttu-id="91e59-188">Hello FileProvisioningAgent projekt felépítéséhez.</span><span class="sxs-lookup"><span data-stu-id="91e59-188">Build hello FileProvisioningAgent project.</span></span>
6. <span data-ttu-id="91e59-189">Indítsa el a (rendszergazdaként) a Windows hello parancssori alkalmazás, és a hello **cd** parancs toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** mappa.</span><span class="sxs-lookup"><span data-stu-id="91e59-189">Launch hello Command Prompt application in Windows (as an Administrator), and use hello **cd** command toochange hello directory tooyour **\AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug** folder.</span></span>
7. <span data-ttu-id="91e59-190">Futtassa a következő parancsot, a < ip-cím > lecserélését hello IP-cím vagy tartománynév kiszolgálónevét hello Windows gép hello:</span><span class="sxs-lookup"><span data-stu-id="91e59-190">Run hello following command, replacing <ip-address> with hello IP address or domain name of hello Windows machine:</span></span>
  ````   
   FileAgnt.exe http://<ip-address>:9000 TargetFile.csv
  ````
8. <span data-ttu-id="91e59-191">A Windows **Windows-beállítások > hálózat és Internet beállítások**, jelölje be hello **Windows tűzfal > Speciális beállítások**, és hozzon létre egy **bejövő szabály** , lehetővé teszi a bejövő hozzáférést tooport 9000.</span><span class="sxs-lookup"><span data-stu-id="91e59-191">In Windows under **Windows Settings > Network & Internet Settings**, select hello **Windows Firewall > Advanced Settings**, and create an **Inbound Rule** that allows inbound access tooport 9000.</span></span>
9. <span data-ttu-id="91e59-192">Ha Windows-számítógép hello útválasztó mögött, hello útválasztó igényeinek konfigurált toobe tooperform hálózati hozzáférési fordítási a portjára 9000 között, amely elérhetővé toohello internet és port 9000 hello Windows számítógépen.</span><span class="sxs-lookup"><span data-stu-id="91e59-192">If hello Windows machine is behind a router, hello router needs toobe configured tooperform Network Access Translation between its port 9000 that is exposed toohello internet, and port 9000 on hello Windows machine.</span></span> <span data-ttu-id="91e59-193">Ez az Azure AD toobe képes tooaccess szükséges ehhez a végponthoz hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="91e59-193">This is required for Azure AD toobe able tooaccess this endpoint in hello cloud.</span></span>

<span data-ttu-id="91e59-194">**tooregister hello minta SCIM végpont Azure AD-ben:**</span><span class="sxs-lookup"><span data-stu-id="91e59-194">**tooregister hello sample SCIM endpoint in Azure AD:**</span></span>

1. <span data-ttu-id="91e59-195">Jelentkezzen be a túl[hello Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="91e59-195">Sign in too[hello Azure portal](https://portal.azure.com).</span></span> 
2. <span data-ttu-id="91e59-196">Keresse meg a túl ** Azure Active Directory > Vállalati alkalmazások, és válassza ki **új alkalmazás > minden > nem-gyűjtemény alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="91e59-196">Browse too**Azure Active Directory > Enterprise Applications, and select **New application > All > Non-gallery application**.</span></span>
3. <span data-ttu-id="91e59-197">Adja meg az alkalmazás nevét, és kattintson a **Hozzáadás** ikon toocreate app objektum.</span><span class="sxs-lookup"><span data-stu-id="91e59-197">Enter a name for your application, and click **Add** icon toocreate an app object.</span></span> <span data-ttu-id="91e59-198">létrehozott hello alkalmazásobjektum tervezett toorepresent hello cél app volna egyszeri bejelentkezés, és nem csak hello SCIM végpont végrehajtási tooand kiépítés.</span><span class="sxs-lookup"><span data-stu-id="91e59-198">hello application object created is intended toorepresent hello target app you would be provisioning tooand implementing single sign-on for, and not just hello SCIM endpoint.</span></span>
4. <span data-ttu-id="91e59-199">Az eredményül kapott üdvözlő képernyőt, válassza ki a hello **kiépítési** lapon hello bal oldali oszlopban.</span><span class="sxs-lookup"><span data-stu-id="91e59-199">In hello resulting screen, select hello **Provisioning** tab in hello left column.</span></span>
5. <span data-ttu-id="91e59-200">A hello **kiépítési üzemmódban** menü **automatikus**.</span><span class="sxs-lookup"><span data-stu-id="91e59-200">In hello **Provisioning Mode** menu, select **Automatic**.</span></span>
    
  <span data-ttu-id="91e59-201">![][2]
  *4. ábra: Konfigurálása kiosztás a hello Azure-portálon*</span><span class="sxs-lookup"><span data-stu-id="91e59-201">![][2]
*Figure 4: Configuring provisioning in hello Azure portal*</span></span>
    
6. <span data-ttu-id="91e59-202">A hello **bérlői URL-cím** mezőbe írja be a hello internet közzétett URL-cím és port a SCIM végpont.</span><span class="sxs-lookup"><span data-stu-id="91e59-202">In hello **Tenant URL** field, enter hello internet-exposed URL and port of your SCIM endpoint.</span></span> <span data-ttu-id="91e59-203">Ez lenne valamit, például http://testmachine.contoso.com:9000 vagy http://<ip-address>:9000/, ahol a < ip-cím > hello internet van közzétéve az IP cím.</span><span class="sxs-lookup"><span data-stu-id="91e59-203">This would be something like http://testmachine.contoso.com:9000 or http://<ip-address>:9000/, where <ip-address> is hello internet exposed IP address.</span></span>  
7. <span data-ttu-id="91e59-204">Ha hello SCIM végpont megköveteli az OAuth tulajdonosi jogkivonat nem az Azure AD egy kiállítótól érkező, akkor a Másolás hello szükséges OAuth tulajdonosi jogkivonattal történő választható hello **titkos Token** mező.</span><span class="sxs-lookup"><span data-stu-id="91e59-204">If hello SCIM endpoint requires an OAuth bearer token from an issuer other than Azure AD, then copy hello required OAuth bearer token into hello optional **Secret Token** field.</span></span> <span data-ttu-id="91e59-205">Ez a mező üresen marad, ha az Azure AD tartalmazza az Azure ad-minden egyes kérelemmel kiadott OAuth tulajdonosi jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="91e59-205">If this field is left blank, then Azure AD will include an OAuth bearer token issued from Azure AD with each request.</span></span> <span data-ttu-id="91e59-206">Alkalmazások, az Azure AD használja az identitásszolgáltató azt is ellenőrzi az Azure AD-jogkivonatot ki.</span><span class="sxs-lookup"><span data-stu-id="91e59-206">Apps that use Azure AD as an identity provider can validate this Azure AD -issued token.</span></span>
8. <span data-ttu-id="91e59-207">Kattintson a hello **kapcsolat tesztelése** gomb toohave Azure Active Directory kísérlet tooconnect toohello SCIM végpont.</span><span class="sxs-lookup"><span data-stu-id="91e59-207">Click hello **Test Connection** button toohave Azure Active Directory attempt tooconnect toohello SCIM endpoint.</span></span> <span data-ttu-id="91e59-208">Ha hello kísérlet sikertelen, hiba információk jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="91e59-208">If hello attempts fail, error information is displayed.</span></span>  
9. <span data-ttu-id="91e59-209">Ha hello kísérletek tooconnect toohello alkalmazás sikeres legyen, majd kattintson **mentése** toosave hello rendszergazdai hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="91e59-209">If hello attempts tooconnect toohello application succeed, then click **Save** toosave hello admin credentials.</span></span>
10. <span data-ttu-id="91e59-210">A hello **hozzárendelések** szakaszban, a két választható csoport attribútum-leképezésekhez: egy felhasználói objektum, egy, a csoport objektumainak.</span><span class="sxs-lookup"><span data-stu-id="91e59-210">In hello **Mappings** section, there are two selectable sets of attribute mappings: one for user objects and one for group objects.</span></span> <span data-ttu-id="91e59-211">Minden egyes egy tooreview hello attribútumokat szinkronizált Azure Active Directory tooyour app választja ki.</span><span class="sxs-lookup"><span data-stu-id="91e59-211">Select each one tooreview hello attributes that are synchronized from Azure Active Directory tooyour app.</span></span> <span data-ttu-id="91e59-212">kiválasztott attribútumok hello **egyező** tulajdonságainak használt toomatch hello felhasználókat és csoportokat a frissítési műveletek az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="91e59-212">hello attributes selected as **Matching** properties are used toomatch hello users and groups in your app for update operations.</span></span> <span data-ttu-id="91e59-213">Válassza ki a hello Mentés gombra toocommit módosításokat.</span><span class="sxs-lookup"><span data-stu-id="91e59-213">Select hello Save button toocommit any changes.</span></span>
11. <span data-ttu-id="91e59-214">A **beállítások**, hello **hatókör** mező határozza meg, hogy mely felhasználók és az or csoportok szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="91e59-214">Under **Settings**, hello **Scope** field defines which users and or groups are synchronized.</span></span> <span data-ttu-id="91e59-215">Válassza a "Sync csak hozzárendelt felhasználók és csoportok" (ajánlott) csak szinkronizálva lesz a hello rendelt felhasználók és csoportok **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="91e59-215">Selecting "Sync only assigned users and groups" (recommended) will only sync users and groups assigned in hello **Users and groups** tab.</span></span>
12. <span data-ttu-id="91e59-216">A konfiguráció befejezése után módosítsa a hello **kiépítési állapot** túl**a**.</span><span class="sxs-lookup"><span data-stu-id="91e59-216">Once your configuration is complete, change hello **Provisioning Status** too**On**.</span></span>
13. <span data-ttu-id="91e59-217">Kattintson a **mentése** toostart hello Azure AD létesítési szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="91e59-217">Click **Save** toostart hello Azure AD provisioning service.</span></span> 
14. <span data-ttu-id="91e59-218">Ha csak szinkronizálás hozzárendelve felhasználók és csoportok (ajánlott), lehet, hogy tooselect hello **felhasználók és csoportok** lapra, és rendeljen hello felhasználók és/vagy csoportok toosync kívánja.</span><span class="sxs-lookup"><span data-stu-id="91e59-218">If syncing only assigned users and groups (recommended), be sure tooselect hello **Users and groups** tab and assign hello users and/or groups you wish toosync.</span></span>

<span data-ttu-id="91e59-219">Miután hello kezdeti szinkronizálás elindult, használhatja a hello **naplók** lapon toomonitor folyamatban, amely mutatja az alkalmazás a szolgáltatás kiépítését hello által végzett összes műveletet.</span><span class="sxs-lookup"><span data-stu-id="91e59-219">Once hello initial synchronization has started, you can use hello **Audit logs** tab toomonitor progress, which shows all actions performed by hello provisioning service on your app.</span></span> <span data-ttu-id="91e59-220">Hogyan tooread hello Azure AD-kiépítés naplózza a további információkért lásd: [automatikus felhasználói fiók kiépítése jelentések](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span><span class="sxs-lookup"><span data-stu-id="91e59-220">For more information on how tooread hello Azure AD provisioning logs, see [Reporting on automatic user account provisioning](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-saas-provisioning-reporting).</span></span>

<span data-ttu-id="91e59-221">utolsó lépés hello ellenőrzése során hello minta tooopen hello TargetFile.csv fájlt a számítógépre a Windows hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug mappában.</span><span class="sxs-lookup"><span data-stu-id="91e59-221">hello final step in verifying hello sample is tooopen hello TargetFile.csv file in hello \AzureAD-BYOA-Provisioning-Samples\ProvisioningAgent\bin\Debug folder on your Windows machine.</span></span> <span data-ttu-id="91e59-222">Létesítésének folyamatát kell használnia hello futtatása után ez a fájl hello részletek az összes társított és kiosztása a felhasználók és csoportok jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91e59-222">Once hello provisioning process is run, this file shows hello details of all assigned and provisioned users and groups.</span></span>

### <a name="development-libraries"></a><span data-ttu-id="91e59-223">Fejlesztő függvénytárak</span><span class="sxs-lookup"><span data-stu-id="91e59-223">Development libraries</span></span>
<span data-ttu-id="91e59-224">toodevelop saját webes szolgáltatás, amely megfelel a toohello SCIM specifikációját, először megismerje a megadott Microsoft toohelp felgyorsítani hello platform fejlesztési folyamata a következő könyvtárak hello:</span><span class="sxs-lookup"><span data-stu-id="91e59-224">toodevelop your own web service that conforms toohello SCIM specification, first familiarize yourself with hello following libraries provided by Microsoft toohelp accelerate hello development process:</span></span> 

1. <span data-ttu-id="91e59-225">Közös nyelvi infrastruktúra (CLI) szalagtárak alapján, hogy az infrastrukturális, például a C# nyelv felkínált való használatra.</span><span class="sxs-lookup"><span data-stu-id="91e59-225">Common Language Infrastructure (CLI) libraries are offered for use with languages based on that infrastructure, such as C#.</span></span> <span data-ttu-id="91e59-226">A tárak egyik [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), deklarál illesztőfelület, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, hello a következő ábrán látható: A fejlesztői hello könyvtárak segítségével egy osztály, amely lehet hivatkozni, általános szolgáltatóként felülettel volna megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="91e59-226">One of those libraries, [Microsoft.SystemForCrossDomainIdentityManagement.Service](https://www.nuget.org/packages/Microsoft.SystemForCrossDomainIdentityManagement/), declares an interface, Microsoft.SystemForCrossDomainIdentityManagement.IProvider, shown in hello following illustration:  A developer using hello libraries would implement that interface with a class that may be referred to, generically, as a provider.</span></span> <span data-ttu-id="91e59-227">hello szalagtárak hello fejlesztői toodeploy egy webszolgáltatás, amely megfelel a toohello SCIM meghatározás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="91e59-227">hello libraries enable hello developer toodeploy a web service that conforms toohello SCIM specification.</span></span> <span data-ttu-id="91e59-228">hello webszolgáltatás Internet Information Services, vagy bármilyen végrehajtható közös nyelvi infrastruktúra szerelvény vagy lehet üzemeltetni.</span><span class="sxs-lookup"><span data-stu-id="91e59-228">hello web service can be either hosted within Internet Information Services, or any executable Common Language Infrastructure assembly.</span></span> <span data-ttu-id="91e59-229">Kérelem hívások toohello szolgáltató, módszerek által hello fejlesztői toooperate néhány identitás tárolójában lévő volna programozhatók van lefordítva.</span><span class="sxs-lookup"><span data-stu-id="91e59-229">Request is translated into calls toohello provider’s methods, which would be programmed by hello developer toooperate on some identity store.</span></span>
  
  ![][3]
  
2. <span data-ttu-id="91e59-230">[Express route kezelők](http://expressjs.com/guide/routing.html) érhetők el elemzés node.js kérelem objektumokból felé indított hívások (által meghatározott hello SCIM specification), tooa node.js webes szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="91e59-230">[Express route handlers](http://expressjs.com/guide/routing.html) are available for parsing node.js request objects representing calls (as defined by hello SCIM specification), made tooa node.js web service.</span></span>   

### <a name="building-a-custom-scim-endpoint"></a><span data-ttu-id="91e59-231">Egy egyéni SCIM végpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="91e59-231">Building a Custom SCIM Endpoint</span></span>
<span data-ttu-id="91e59-232">Hello CLI könyvtárak segítségével a fejlesztők a tárak segítségével tárolhatja, bármilyen végrehajtható közös nyelvi infrastruktúra szerelvényen belül, vagy az Internet Information Services belül a szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="91e59-232">Using hello CLI libraries, developers using those libraries can host their services within any executable Common Language Infrastructure assembly, or within Internet Information Services.</span></span> <span data-ttu-id="91e59-233">Itt látható mintakód egy végrehajtható szerelvényben, a következő címen: hello http://localhost:9000 szolgáltatás üzemeltetéséhez:</span><span class="sxs-lookup"><span data-stu-id="91e59-233">Here is sample code for hosting a service within an executable assembly, at hello address http://localhost:9000:</span></span> 

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

<span data-ttu-id="91e59-234">Ez a szolgáltatás egy HTTP címet és a kiszolgáló hitelesítési tanúsítvánnyal kell rendelkeznie, mely hello legfelső szintű hitelesítésszolgáltató hello következő:</span><span class="sxs-lookup"><span data-stu-id="91e59-234">This service must have an HTTP address and server authentication certificate of which hello root certification authority is one of hello following:</span></span> 

* <span data-ttu-id="91e59-235">CNNIC</span><span class="sxs-lookup"><span data-stu-id="91e59-235">CNNIC</span></span>
* <span data-ttu-id="91e59-236">Comodo</span><span class="sxs-lookup"><span data-stu-id="91e59-236">Comodo</span></span>
* <span data-ttu-id="91e59-237">CyberTrust</span><span class="sxs-lookup"><span data-stu-id="91e59-237">CyberTrust</span></span>
* <span data-ttu-id="91e59-238">DigiCert</span><span class="sxs-lookup"><span data-stu-id="91e59-238">DigiCert</span></span>
* <span data-ttu-id="91e59-239">GeoTrust</span><span class="sxs-lookup"><span data-stu-id="91e59-239">GeoTrust</span></span>
* <span data-ttu-id="91e59-240">GlobalSign</span><span class="sxs-lookup"><span data-stu-id="91e59-240">GlobalSign</span></span>
* <span data-ttu-id="91e59-241">Go Daddy</span><span class="sxs-lookup"><span data-stu-id="91e59-241">Go Daddy</span></span>
* <span data-ttu-id="91e59-242">A VeriSign szolgáltatótól</span><span class="sxs-lookup"><span data-stu-id="91e59-242">Verisign</span></span>
* <span data-ttu-id="91e59-243">WoSign</span><span class="sxs-lookup"><span data-stu-id="91e59-243">WoSign</span></span>

<span data-ttu-id="91e59-244">Kiszolgálói hitelesítési tanúsítvány lehet a Windows hello hálózati rendszerhéj-segédprogrammal történő gazdagépen kötött tooa port:</span><span class="sxs-lookup"><span data-stu-id="91e59-244">A server authentication certificate can be bound tooa port on a Windows host using hello network shell utility:</span></span> 

    netsh http add sslcert ipport=0.0.0.0:443 certhash=0000000000003ed9cd0c315bbb6dc1c08da5e6 appid={00112233-4455-6677-8899-AABBCCDDEEFF}  

<span data-ttu-id="91e59-245">Itt hello megadott hello certhash argumentum érték hello tanúsítvány ujjlenyomata hello hello appid argumentumban megadott hello érték pedig tetszőleges globálisan egyedi azonosító.</span><span class="sxs-lookup"><span data-stu-id="91e59-245">Here, hello value provided for hello certhash argument is hello thumbprint of hello certificate, while hello value provided for hello appid argument is an arbitrary globally unique identifier.</span></span>  

<span data-ttu-id="91e59-246">az Internet Information Services toohost hello szolgáltatást, egy fejlesztő lenne build CLA könyvtár kódszerelvényből indítási nevű hello alapértelmezett névtér hello szerelvény osztállyal.</span><span class="sxs-lookup"><span data-stu-id="91e59-246">toohost hello service within Internet Information Services, a developer would build a CLA code library assembly with a class named Startup in hello default namespace of hello assembly.</span></span>  <span data-ttu-id="91e59-247">Íme egy példa ilyen osztály:</span><span class="sxs-lookup"><span data-stu-id="91e59-247">Here is a sample of such a class:</span></span> 

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

### <a name="handling-endpoint-authentication"></a><span data-ttu-id="91e59-248">Kezelési végpont hitelesítés</span><span class="sxs-lookup"><span data-stu-id="91e59-248">Handling endpoint authentication</span></span>
<span data-ttu-id="91e59-249">Az Azure Active Directory kérések tartalmazzák az OAuth 2.0 tulajdonosi jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="91e59-249">Requests from Azure Active Directory include an OAuth 2.0 bearer token.</span></span>   <span data-ttu-id="91e59-250">A fogadó hello szolgáltatáskérés hitelesítenie kell, hogy az Azure Active Directory nevében várt hello Azure Active Directory-bérlőt az Azure Active Directory Graph webszolgáltatás hozzáférés toohello hello kibocsátó.</span><span class="sxs-lookup"><span data-stu-id="91e59-250">Any service receiving hello request should authenticate hello issuer as being Azure Active Directory on behalf of hello expected Azure Active Directory tenant, for access toohello Azure Active Directory Graph web service.</span></span>  <span data-ttu-id="91e59-251">Hello jogkivonat hello kibocsátó azonosít egy iss jogcímet, például "iss": "https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span><span class="sxs-lookup"><span data-stu-id="91e59-251">In hello token, hello issuer is identified by an iss claim, like, "iss":"https://sts.windows.net/cbb1a5ac-f33b-45fa-9bf5-f37db0fed422/".</span></span>  <span data-ttu-id="91e59-252">Ebben a példában, alapszintű címéből hello hello jogcím értékét, mivel a kibocsátó hello https://sts.windows.net, azonosítja az Azure Active Directory, pedig hello relatív címet szegmens, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422 egyedi azonosítójának hello Azure Active Directory-bérlő nevében mely hello token lett kiállítva.</span><span class="sxs-lookup"><span data-stu-id="91e59-252">In this example, hello base address of hello claim value, https://sts.windows.net, identifies Azure Active Directory as hello issuer, while hello relative address segment, cbb1a5ac-f33b-45fa-9bf5-f37db0fed422, is a unique identifier of hello Azure Active Directory tenant on behalf of which hello token was issued.</span></span>  <span data-ttu-id="91e59-253">Ha hello token ki hello Azure Active Directory Graph webes szolgáltatás, majd hello azonosítója, hogy a szolgáltatás elérésével 00000002-0000-0000-c000-000000000000 kell kell hello token és hello értékének jogcímek.</span><span class="sxs-lookup"><span data-stu-id="91e59-253">If hello token was issued for accessing hello Azure Active Directory Graph web service, then hello identifier of that service, 00000002-0000-0000-c000-000000000000, should be in hello value of hello token’s aud claim.</span></span>  

<span data-ttu-id="91e59-254">SCIM szolgáltatás létrehozása a Microsoft által biztosított hello CLA könyvtárak segítségével a fejlesztők hitelesítheti a kérelmeket az Azure Active Directory hello Microsoft.Owin.Security.ActiveDirectory csomag használata a következő lépések végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="91e59-254">Developers using hello CLA libraries provided by Microsoft for building a SCIM service can authenticate requests from Azure Active Directory using hello Microsoft.Owin.Security.ActiveDirectory package by following these steps:</span></span> 

1. <span data-ttu-id="91e59-255">A szolgáltató megvalósíthatja hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior tulajdonság azt egy hello szolgáltatás indulásakor nevű metódus toobe vissza:</span><span class="sxs-lookup"><span data-stu-id="91e59-255">In a provider, implement hello Microsoft.SystemForCrossDomainIdentityManagement.IProvider.StartupBehavior property by having it return a method toobe called whenever hello service is started:</span></span> 

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

2. <span data-ttu-id="91e59-256">A következő kód toothat metódus toohave hello bármely kérelem tooany hitelesítve, szem előtt a megadott tenantot, az Azure AD Graph webszolgáltatás hozzáférés toohello nevében Azure Active Directory által kiadott tokennek hello szolgáltatás végpontok hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="91e59-256">Add hello following code toothat method toohave any request tooany of hello service’s endpoints authenticated as bearing a token issued by Azure Active Directory on behalf of a specified tenant, for access toohello Azure AD Graph web service:</span></span> 

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
        Tenant = "03F9FCBC-EA7B-46C2-8466-F81917F3C15E" // Substitute hello appropriate tenant’s 
                                                      // identifier for this one.  
      };

      applicationBuilder.UseWindowsAzureActiveDirectoryBearerAuthentication(authenticationOptions);
    }
  ````


## <a name="user-and-group-schema"></a><span data-ttu-id="91e59-257">Felhasználó- és séma</span><span class="sxs-lookup"><span data-stu-id="91e59-257">User and group schema</span></span>
<span data-ttu-id="91e59-258">Az Azure Active Directory kétféle típusú erőforrások tooSCIM webszolgáltatások építhető ki.</span><span class="sxs-lookup"><span data-stu-id="91e59-258">Azure Active Directory can provision two types of resources tooSCIM web services.</span></span>  <span data-ttu-id="91e59-259">Ilyen típusú erőforrások a felhasználók és csoportok.</span><span class="sxs-lookup"><span data-stu-id="91e59-259">Those types of resources are users and groups.</span></span>  

<span data-ttu-id="91e59-260">Felhasználói erőforrások hello sémaazonosítót, szerepel a protokoll-meghatározása urn: ietf:params:scim:schemas:extension:enterprise:2.0:User azonosítja: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span><span class="sxs-lookup"><span data-stu-id="91e59-260">User resources are identified by hello schema identifier, urn:ietf:params:scim:schemas:extension:enterprise:2.0:User, which is included in this protocol specification: http://tools.ietf.org/html/draft-ietf-scim-core-schema.</span></span>  <span data-ttu-id="91e59-261">hello alapértelmezett leképezését hello Azure Active Directory toohello attribútumok urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrások a felhasználók lejjebb tekinthetők meg a tábla 1.</span><span class="sxs-lookup"><span data-stu-id="91e59-261">hello default mapping of hello attributes of users in Azure Active Directory toohello attributes of urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resources is provided in table 1, below.</span></span>  

<span data-ttu-id="91e59-262">Erőforrások hello sémaazonosítót, azonosítva http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span><span class="sxs-lookup"><span data-stu-id="91e59-262">Group resources are identified by hello schema identifier, http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  <span data-ttu-id="91e59-263">2. táblázat, alább látható hello alapértelmezett leképezését hello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group erőforrások az Azure Active Directory toohello attribútumok csoportok.</span><span class="sxs-lookup"><span data-stu-id="91e59-263">Table 2, below, shows hello default mapping of hello attributes of groups in Azure Active Directory toohello attributes of http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group resources.</span></span>  

### <a name="table-1-default-user-attribute-mapping"></a><span data-ttu-id="91e59-264">1. táblázat: Alapértelmezett felhasználói címtárattribútum-leképezésben</span><span class="sxs-lookup"><span data-stu-id="91e59-264">Table 1: Default user attribute mapping</span></span>
| <span data-ttu-id="91e59-265">Az Azure Active Directory-felhasználó</span><span class="sxs-lookup"><span data-stu-id="91e59-265">Azure Active Directory user</span></span> | <span data-ttu-id="91e59-266">urn: ietf:params:scim:schemas:extension:enterprise:2.0:User</span><span class="sxs-lookup"><span data-stu-id="91e59-266">urn:ietf:params:scim:schemas:extension:enterprise:2.0:User</span></span> |
| --- | --- |
| <span data-ttu-id="91e59-267">IsSoftDeleted</span><span class="sxs-lookup"><span data-stu-id="91e59-267">IsSoftDeleted</span></span> |<span data-ttu-id="91e59-268">Aktív</span><span class="sxs-lookup"><span data-stu-id="91e59-268">active</span></span> |
| <span data-ttu-id="91e59-269">displayName</span><span class="sxs-lookup"><span data-stu-id="91e59-269">displayName</span></span> |<span data-ttu-id="91e59-270">displayName</span><span class="sxs-lookup"><span data-stu-id="91e59-270">displayName</span></span> |
| <span data-ttu-id="91e59-271">Telefax-TelephoneNumber</span><span class="sxs-lookup"><span data-stu-id="91e59-271">Facsimile-TelephoneNumber</span></span> |<span data-ttu-id="91e59-272">.value phoneNumbers [típus eq "fax"]</span><span class="sxs-lookup"><span data-stu-id="91e59-272">phoneNumbers[type eq "fax"].value</span></span> |
| <span data-ttu-id="91e59-273">givenName</span><span class="sxs-lookup"><span data-stu-id="91e59-273">givenName</span></span> |<span data-ttu-id="91e59-274">name.givenName</span><span class="sxs-lookup"><span data-stu-id="91e59-274">name.givenName</span></span> |
| <span data-ttu-id="91e59-275">Beosztás</span><span class="sxs-lookup"><span data-stu-id="91e59-275">jobTitle</span></span> |<span data-ttu-id="91e59-276">Cím</span><span class="sxs-lookup"><span data-stu-id="91e59-276">title</span></span> |
| <span data-ttu-id="91e59-277">mail</span><span class="sxs-lookup"><span data-stu-id="91e59-277">mail</span></span> |<span data-ttu-id="91e59-278">e-mailek [típus eq "munkahelyi"] .value</span><span class="sxs-lookup"><span data-stu-id="91e59-278">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="91e59-279">mailNickname</span><span class="sxs-lookup"><span data-stu-id="91e59-279">mailNickname</span></span> |<span data-ttu-id="91e59-280">externalId</span><span class="sxs-lookup"><span data-stu-id="91e59-280">externalId</span></span> |
| <span data-ttu-id="91e59-281">Manager</span><span class="sxs-lookup"><span data-stu-id="91e59-281">manager</span></span> |<span data-ttu-id="91e59-282">Manager</span><span class="sxs-lookup"><span data-stu-id="91e59-282">manager</span></span> |
| <span data-ttu-id="91e59-283">Mobileszköz</span><span class="sxs-lookup"><span data-stu-id="91e59-283">mobile</span></span> |<span data-ttu-id="91e59-284">.value phoneNumbers [típus eq "mobileszköz"]</span><span class="sxs-lookup"><span data-stu-id="91e59-284">phoneNumbers[type eq "mobile"].value</span></span> |
| <span data-ttu-id="91e59-285">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="91e59-285">objectId</span></span> |<span data-ttu-id="91e59-286">id</span><span class="sxs-lookup"><span data-stu-id="91e59-286">id</span></span> |
| <span data-ttu-id="91e59-287">Irányítószám</span><span class="sxs-lookup"><span data-stu-id="91e59-287">postalCode</span></span> |<span data-ttu-id="91e59-288">[típus eq "munkahelyi"] címek .postalCode</span><span class="sxs-lookup"><span data-stu-id="91e59-288">addresses[type eq "work"].postalCode</span></span> |
| <span data-ttu-id="91e59-289">proxy-címek</span><span class="sxs-lookup"><span data-stu-id="91e59-289">proxy-Addresses</span></span> |<span data-ttu-id="91e59-290">[Írja be az "egyéb" eq] e-maileket. Érték</span><span class="sxs-lookup"><span data-stu-id="91e59-290">emails[type eq "other"].Value</span></span> |
| <span data-ttu-id="91e59-291">fizikai-kézbesítés-OfficeName</span><span class="sxs-lookup"><span data-stu-id="91e59-291">physical-Delivery-OfficeName</span></span> |<span data-ttu-id="91e59-292">[Írja be az "egyéb" eq] címek. Formázott</span><span class="sxs-lookup"><span data-stu-id="91e59-292">addresses[type eq "other"].Formatted</span></span> |
| <span data-ttu-id="91e59-293">StreetAddress</span><span class="sxs-lookup"><span data-stu-id="91e59-293">streetAddress</span></span> |<span data-ttu-id="91e59-294">[típus eq "munkahelyi"] címek .streetAddress</span><span class="sxs-lookup"><span data-stu-id="91e59-294">addresses[type eq "work"].streetAddress</span></span> |
| <span data-ttu-id="91e59-295">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="91e59-295">surname</span></span> |<span data-ttu-id="91e59-296">name.familyName</span><span class="sxs-lookup"><span data-stu-id="91e59-296">name.familyName</span></span> |
| <span data-ttu-id="91e59-297">Telefonszám</span><span class="sxs-lookup"><span data-stu-id="91e59-297">telephone-Number</span></span> |<span data-ttu-id="91e59-298">.value phoneNumbers [típus eq "munkahelyi"]</span><span class="sxs-lookup"><span data-stu-id="91e59-298">phoneNumbers[type eq "work"].value</span></span> |
| <span data-ttu-id="91e59-299">felhasználó-egyszerű név</span><span class="sxs-lookup"><span data-stu-id="91e59-299">user-PrincipalName</span></span> |<span data-ttu-id="91e59-300">Felhasználónév</span><span class="sxs-lookup"><span data-stu-id="91e59-300">userName</span></span> |

### <a name="table-2-default-group-attribute-mapping"></a><span data-ttu-id="91e59-301">2. táblázat: Alapértelmezett attribútum leképezése</span><span class="sxs-lookup"><span data-stu-id="91e59-301">Table 2: Default group attribute mapping</span></span>
| <span data-ttu-id="91e59-302">Azure Active Directory-csoportok</span><span class="sxs-lookup"><span data-stu-id="91e59-302">Azure Active Directory group</span></span> | <span data-ttu-id="91e59-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span><span class="sxs-lookup"><span data-stu-id="91e59-303">http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group</span></span> |
| --- | --- |
| <span data-ttu-id="91e59-304">displayName</span><span class="sxs-lookup"><span data-stu-id="91e59-304">displayName</span></span> |<span data-ttu-id="91e59-305">externalId</span><span class="sxs-lookup"><span data-stu-id="91e59-305">externalId</span></span> |
| <span data-ttu-id="91e59-306">mail</span><span class="sxs-lookup"><span data-stu-id="91e59-306">mail</span></span> |<span data-ttu-id="91e59-307">e-mailek [típus eq "munkahelyi"] .value</span><span class="sxs-lookup"><span data-stu-id="91e59-307">emails[type eq "work"].value</span></span> |
| <span data-ttu-id="91e59-308">mailNickname</span><span class="sxs-lookup"><span data-stu-id="91e59-308">mailNickname</span></span> |<span data-ttu-id="91e59-309">displayName</span><span class="sxs-lookup"><span data-stu-id="91e59-309">displayName</span></span> |
| <span data-ttu-id="91e59-310">Tagok</span><span class="sxs-lookup"><span data-stu-id="91e59-310">members</span></span> |<span data-ttu-id="91e59-311">Tagok</span><span class="sxs-lookup"><span data-stu-id="91e59-311">members</span></span> |
| <span data-ttu-id="91e59-312">Objektumazonosító</span><span class="sxs-lookup"><span data-stu-id="91e59-312">objectId</span></span> |<span data-ttu-id="91e59-313">id</span><span class="sxs-lookup"><span data-stu-id="91e59-313">id</span></span> |
| <span data-ttu-id="91e59-314">proxyAddresses</span><span class="sxs-lookup"><span data-stu-id="91e59-314">proxyAddresses</span></span> |<span data-ttu-id="91e59-315">[Írja be az "egyéb" eq] e-maileket. Érték</span><span class="sxs-lookup"><span data-stu-id="91e59-315">emails[type eq "other"].Value</span></span> |

## <a name="user-provisioning-and-de-provisioning"></a><span data-ttu-id="91e59-316">Felhasználói üzembe helyezést és megszüntetést</span><span class="sxs-lookup"><span data-stu-id="91e59-316">User provisioning and de-provisioning</span></span>
<span data-ttu-id="91e59-317">a következő ábra azt mutatja be, hogy Azure Active Directory tooa SCIM szolgáltatás toomanage hello életciklusát a felhasználó elküldi egy másik identitás tárolására köszönőüzenetei hello.</span><span class="sxs-lookup"><span data-stu-id="91e59-317">hello following illustration shows hello messages that Azure Active Directory sends tooa SCIM service toomanage hello lifecycle of a user in another identity store.</span></span> <span data-ttu-id="91e59-318">hello ábrán is látható, hogyan hello CLI szalagtárak készletével megvalósított SCIM szolgáltatás által biztosított Microsoft rendszerbeli kiépítésének e szolgáltatások lefordítása hívások toohello módszerek a szolgáltató ezeket a kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="91e59-318">hello diagram also shows how a SCIM service implemented using hello CLI libraries provided by Microsoft for building such services translate those requests into calls toohello methods of a provider.</span></span>  

<span data-ttu-id="91e59-319">![][4]
*5. ábra: A felhasználók átadása, és megszüntetést feladatütemezési*</span><span class="sxs-lookup"><span data-stu-id="91e59-319">![][4]
*Figure 5: User provisioning and de-provisioning sequence*</span></span>

1. <span data-ttu-id="91e59-320">Az Azure Active Directory lekérdezésekkel hello szolgáltatást az Azure AD-ben hello mailNickname attribútum értékét a felhasználó megfelelő externalId attribútum értéke.</span><span class="sxs-lookup"><span data-stu-id="91e59-320">Azure Active Directory queries hello service for a user with an externalId attribute value matching hello mailNickname attribute value of a user in Azure AD.</span></span> <span data-ttu-id="91e59-321">Ebben a példában, amelynek jyoung egy olyan felhasználó, az Azure Active Directoryban egy mailNickname mintát például Hypertext Transfer Protocol (HTTP) kérelmet hello lekérdezési kifejezése:</span><span class="sxs-lookup"><span data-stu-id="91e59-321">hello query is expressed as a Hypertext Transfer Protocol (HTTP) request such as this example, wherein jyoung is a sample of a mailNickname of a user in Azure Active Directory:</span></span> 
  ````
    GET https://.../scim/Users?filter=externalId eq jyoung HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="91e59-322">Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató lekérdezési metódust.</span><span class="sxs-lookup"><span data-stu-id="91e59-322">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span>  <span data-ttu-id="91e59-323">Ez a metódus aláírása hello:</span><span class="sxs-lookup"><span data-stu-id="91e59-323">Here is hello signature of that method:</span></span> 
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
  <span data-ttu-id="91e59-324">Íme hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters felület hello definíciója:</span><span class="sxs-lookup"><span data-stu-id="91e59-324">Here is hello definition of hello Microsoft.SystemForCrossDomainIdentityManagement.IQueryParameters interface:</span></span> 
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
  <span data-ttu-id="91e59-325">A következő példa egy felhasználó egy adott értékre hello externalId attribútum lekérdezés hello toohello lekérdezés módszer átadott hello argumentumok értékek a következők:</span><span class="sxs-lookup"><span data-stu-id="91e59-325">In hello following sample of a query for a user with a given value for hello externalId attribute, values of hello arguments passed toohello Query method are:</span></span> 
  * <span data-ttu-id="91e59-326">a paraméterek. AlternateFilters.Count: 1</span><span class="sxs-lookup"><span data-stu-id="91e59-326">parameters.AlternateFilters.Count: 1</span></span>
  * <span data-ttu-id="91e59-327">a paraméterek. AlternateFilters.ElementAt(0). AttributePath: "externalId"</span><span class="sxs-lookup"><span data-stu-id="91e59-327">parameters.AlternateFilters.ElementAt(0).AttributePath: "externalId"</span></span>
  * <span data-ttu-id="91e59-328">a paraméterek. AlternateFilters.ElementAt(0). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="91e59-328">parameters.AlternateFilters.ElementAt(0).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="91e59-329">a paraméterek. AlternateFilter.ElementAt(0). ComparisonValue: "jyoung"</span><span class="sxs-lookup"><span data-stu-id="91e59-329">parameters.AlternateFilter.ElementAt(0).ComparisonValue: "jyoung"</span></span>
  * <span data-ttu-id="91e59-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin. Kérelemazonosító"]</span><span class="sxs-lookup"><span data-stu-id="91e59-330">correlationIdentifier: System.Net.Http.HttpRequestMessage.GetOwinEnvironment["owin.RequestId"]</span></span> 

2. <span data-ttu-id="91e59-331">Ha hello válasz tooa lekérdezés toohello webes szolgáltatás, amely megfelel a felhasználó hello mailNickname attribútum értéke externalId attribútumértékkel rendelkező felhasználó nem ad vissza azokat a felhasználókat, Azure Active Directory kéri, hogy hello szolgáltatás kiépítése a felhasználó megfelelő toohello egy Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="91e59-331">If hello response tooa query toohello web service for a user with an externalId attribute value that matches hello mailNickname attribute value of a user does not return any users, then Azure Active Directory requests that hello service provision a user corresponding toohello one in Azure Active Directory.</span></span>  <span data-ttu-id="91e59-332">Íme egy példa a kérelem:</span><span class="sxs-lookup"><span data-stu-id="91e59-332">Here is an example of such a request:</span></span> 
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
  <span data-ttu-id="91e59-333">hello közös nyelvi infrastruktúra szalagtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított lefordítja a kérésre egy hívás toohello Create metódussal hello szolgáltatás szolgáltató be.</span><span class="sxs-lookup"><span data-stu-id="91e59-333">hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services would translate that request into a call toohello Create method of hello service’s provider.</span></span>  <span data-ttu-id="91e59-334">Create metódussal hello az aláírása:</span><span class="sxs-lookup"><span data-stu-id="91e59-334">hello Create method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.Resource is defined in 
    // Microsoft.SystemForCrossDomainIdentityManagement.Schemas.  

    System.Threading.Tasks.Task<Microsoft.SystemForCrossDomainIdentityManagement.Resource> Create(
      Microsoft.SystemForCrossDomainIdentityManagement.Resource resource, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="91e59-335">A kérelem tooprovision egy felhasználó hello hello erőforrás argumentum értéke hello Microsoft.SystemForCrossDomainIdentityManagement példánya.</span><span class="sxs-lookup"><span data-stu-id="91e59-335">In a request tooprovision a user, hello value of hello resource argument is an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="91e59-336">Hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas könyvtárban meghatározott Core2EnterpriseUser osztály.</span><span class="sxs-lookup"><span data-stu-id="91e59-336">Core2EnterpriseUser class, defined in hello Microsoft.SystemForCrossDomainIdentityManagement.Schemas library.</span></span>  <span data-ttu-id="91e59-337">Hello kérelem tooprovision hello felhasználó sikeres, majd hello hello metódus megvalósítása esetén várható tooreturn hello Microsoft.SystemForCrossDomainIdentityManagement példánya.</span><span class="sxs-lookup"><span data-stu-id="91e59-337">If hello request tooprovision hello user succeeds, then hello implementation of hello method is expected tooreturn an instance of hello Microsoft.SystemForCrossDomainIdentityManagement.</span></span> <span data-ttu-id="91e59-338">Core2EnterpriseUser osztályra, hello hello azonosítója tulajdonság értékének beállítása toohello hello újonnan kiosztott felhasználó egyedi azonosítója.</span><span class="sxs-lookup"><span data-stu-id="91e59-338">Core2EnterpriseUser class, with hello value of hello Identifier property set toohello unique identifier of hello newly provisioned user.</span></span>  

3. <span data-ttu-id="91e59-339">a felhasználó által egy SCIM, úgy, hogy a felhasználó aktuális állapotának hello kér hello szolgáltatás például kérelmet tartalmazó Azure Active Directory folytatódik fronted identitás tárolóban tooexist ismert tooupdate:</span><span class="sxs-lookup"><span data-stu-id="91e59-339">tooupdate a user known tooexist in an identity store fronted by an SCIM, Azure Active Directory proceeds by requesting hello current state of that user from hello service with a request such as:</span></span> 
  ````
    GET ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="91e59-340">Hello közös nyelvi infrastruktúra szalagtárak SCIM szolgáltatások végrehajtásához a Microsoft által biztosított használatával készített szolgáltatásban hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltatója lekérése metódusában.</span><span class="sxs-lookup"><span data-stu-id="91e59-340">In a service built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, hello request is translated into a call toohello Retrieve method of hello service’s provider.</span></span>  <span data-ttu-id="91e59-341">Hello lekérése metódus aláírása hello itt található:</span><span class="sxs-lookup"><span data-stu-id="91e59-341">Here is hello signature of hello Retrieve method:</span></span> 
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
  <span data-ttu-id="91e59-342">Egy kérelem tooretrieve hello aktuális állapotát egy felhasználó hello példában hello tulajdonságok hello paraméterek argumentumnak hello értékeként megadott hello objektum hello értékei a következők:</span><span class="sxs-lookup"><span data-stu-id="91e59-342">In hello example of a request tooretrieve hello current state of a user, hello values of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="91e59-343">Azonosító: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="91e59-343">Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="91e59-344">SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="91e59-344">SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

4. <span data-ttu-id="91e59-345">Ha egy hivatkozás attribútum toobe frissítve, majd a Azure Active Directory lekérdezések hello szolgáltatás toodetermine attól hello hello hivatkozási attribútum hello identitás tárolójában aktuális értéke fronted által hello szolgáltatást már megegyezik-e hello adott attribútum az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="91e59-345">If a reference attribute is toobe updated, then Azure Active Directory queries hello service toodetermine whether or not hello current value of hello reference attribute in hello identity store fronted by hello service already matches hello value of that attribute in Azure Active Directory.</span></span> <span data-ttu-id="91e59-346">A felhasználók számára hello attribútum, mely hello a jelenlegi érték a ily módon le kell kérdezni hello manager attribútum.</span><span class="sxs-lookup"><span data-stu-id="91e59-346">For users, hello only attribute of which hello current value is queried in this way is hello manager attribute.</span></span> <span data-ttu-id="91e59-347">Íme egy példa egy kérelem toodetermine e hello kezelő egy adott felhasználó objektum attribútuma van a megadott érték:</span><span class="sxs-lookup"><span data-stu-id="91e59-347">Here is an example of a request toodetermine whether hello manager attribute of a particular user object currently has a certain value:</span></span> 
  ````
    GET ~/scim/Users?filter=id eq 54D382A4-2050-4C03-94D1-E769F1D15682 and manager eq 2819c223-7f76-453a-919d-413861904646&attributes=id HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="91e59-348">érték hello hello attribútumok lekérdezési paraméter azonosítója, jelzi, hogy ha létezik olyan felhasználói objektum, amely megfelel a megadott hello szűrő lekérdezési paraméter értékeként hello hello kifejezést, majd hello szolgáltatás urn: ietf:params:scim:schemas a várt toorespond: Core: 2.0:User vagy urn: ietf:params:scim:schemas:extension:enterprise:2.0:User erőforrás, beleértve az adott erőforrás id attribútum csak hello értékével.</span><span class="sxs-lookup"><span data-stu-id="91e59-348">hello value of hello attributes query parameter, id, signifies that if a user object exists that satisfies hello expression provided as hello value of hello filter query parameter, then hello service is expected toorespond with a urn:ietf:params:scim:schemas:core:2.0:User or urn:ietf:params:scim:schemas:extension:enterprise:2.0:User resource, including only hello value of that resource’s id attribute.</span></span>  <span data-ttu-id="91e59-349">hello értékének hello **azonosító** toohello kérelmező ismert attribútum.</span><span class="sxs-lookup"><span data-stu-id="91e59-349">hello value of hello **id** attribute is known toohello requestor.</span></span> <span data-ttu-id="91e59-350">Hello szűrő lekérdezésparaméter; hello érték szerepel. hello kérdezi azt célja ténylegesen toorequest egy erőforrást, hogy létezik-e ilyen objektum jelezhetik hello szűrőkifejezés felel meg a minimális megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="91e59-350">It is included in hello value of hello filter query parameter; hello purpose of asking for it is actually toorequest a minimal representation of a resource that satisfying hello filter expression as an indication of whether or not any such object exists.</span></span>   

  <span data-ttu-id="91e59-351">Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató lekérdezési metódust.</span><span class="sxs-lookup"><span data-stu-id="91e59-351">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Query method of hello service’s provider.</span></span> <span data-ttu-id="91e59-352">hello paraméterek argumentumnak hello értékeként megadott hello objektum tulajdonságainak hello hello értékének a következők:</span><span class="sxs-lookup"><span data-stu-id="91e59-352">hello value of hello properties of hello object provided as hello value of hello parameters argument are as follows:</span></span> 
  
  * <span data-ttu-id="91e59-353">a paraméterek. AlternateFilters.Count: 2. régiója</span><span class="sxs-lookup"><span data-stu-id="91e59-353">parameters.AlternateFilters.Count: 2</span></span>
  * <span data-ttu-id="91e59-354">a paraméterek. AlternateFilters.ElementAt(x). AttributePath: "id"</span><span class="sxs-lookup"><span data-stu-id="91e59-354">parameters.AlternateFilters.ElementAt(x).AttributePath: "id"</span></span>
  * <span data-ttu-id="91e59-355">a paraméterek. AlternateFilters.ElementAt(x). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="91e59-355">parameters.AlternateFilters.ElementAt(x).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="91e59-356">a paraméterek. AlternateFilter.ElementAt(x). ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="91e59-356">parameters.AlternateFilter.ElementAt(x).ComparisonValue: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="91e59-357">a paraméterek. AlternateFilters.ElementAt(y). AttributePath: "manager"</span><span class="sxs-lookup"><span data-stu-id="91e59-357">parameters.AlternateFilters.ElementAt(y).AttributePath: "manager"</span></span>
  * <span data-ttu-id="91e59-358">a paraméterek. AlternateFilters.ElementAt(y). ÖsszehasonlítóOperátor: ComparisonOperator.Equals</span><span class="sxs-lookup"><span data-stu-id="91e59-358">parameters.AlternateFilters.ElementAt(y).ComparisonOperator: ComparisonOperator.Equals</span></span>
  * <span data-ttu-id="91e59-359">a paraméterek. AlternateFilter.ElementAt(y). ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span><span class="sxs-lookup"><span data-stu-id="91e59-359">parameters.AlternateFilter.ElementAt(y).ComparisonValue: "2819c223-7f76-453a-919d-413861904646"</span></span>
  * <span data-ttu-id="91e59-360">a paraméterek. RequestedAttributePaths.ElementAt(0): "id"</span><span class="sxs-lookup"><span data-stu-id="91e59-360">parameters.RequestedAttributePaths.ElementAt(0): "id"</span></span>
  * <span data-ttu-id="91e59-361">a paraméterek. SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="91e59-361">parameters.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

  <span data-ttu-id="91e59-362">Itt hello index x hello értéke lehet 0 és hello index y hello értéke lehet 1, vagy x hello értéke lehet 1 és hello y érték 0, attól függően, hogy hello hello szűrő lekérdezési paraméter hello kifejezések sorrendjét.</span><span class="sxs-lookup"><span data-stu-id="91e59-362">Here, hello value of hello index x may be 0 and hello value of hello index y may be 1, or hello value of x may be 1 and hello value of y may be 0, depending on hello order of hello expressions of hello filter query parameter.</span></span>   

5. <span data-ttu-id="91e59-363">Íme egy példa egy kérelem az Azure Active Directory tooan SCIM szolgáltatás tooupdate egy felhasználó:</span><span class="sxs-lookup"><span data-stu-id="91e59-363">Here is an example of a request from Azure Active Directory tooan SCIM service tooupdate a user:</span></span> 
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
  <span data-ttu-id="91e59-364">hello Microsoft közös nyelvi infrastruktúra-könyvtárakban SCIM szolgáltatások végrehajtási lefordítja hello kérelem be egy hívás toohello frissítési módszer hello szolgáltatás szolgáltató.</span><span class="sxs-lookup"><span data-stu-id="91e59-364">hello Microsoft Common Language Infrastructure libraries for implementing SCIM services would translate hello request into a call toohello Update method of hello service’s provider.</span></span> <span data-ttu-id="91e59-365">Hello aláírása hello frissítési módszer a következő:</span><span class="sxs-lookup"><span data-stu-id="91e59-365">Here is hello signature of hello Update method:</span></span> 
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
    <span data-ttu-id="91e59-366">A kérelem egy felhasználó tooupdate hello példában hello hello javítás argumentumnak hello értékeként megadott objektumnak a tulajdonságok értékeit:</span><span class="sxs-lookup"><span data-stu-id="91e59-366">In hello example of a request tooupdate a user, hello object provided as hello value of hello patch argument has these property values:</span></span> 
  
  * <span data-ttu-id="91e59-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="91e59-367">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="91e59-368">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="91e59-368">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>
  * <span data-ttu-id="91e59-369">(Mint PatchRequest2 PatchRequest). Operations.Count: 1</span><span class="sxs-lookup"><span data-stu-id="91e59-369">(PatchRequest as PatchRequest2).Operations.Count: 1</span></span>
  * <span data-ttu-id="91e59-370">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). OperationName: OperationName.Add</span><span class="sxs-lookup"><span data-stu-id="91e59-370">(PatchRequest as PatchRequest2).Operations.ElementAt(0).OperationName: OperationName.Add</span></span>
  * <span data-ttu-id="91e59-371">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Path.AttributePath: "manager"</span><span class="sxs-lookup"><span data-stu-id="91e59-371">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Path.AttributePath: "manager"</span></span>
  * <span data-ttu-id="91e59-372">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.Count: 1</span><span class="sxs-lookup"><span data-stu-id="91e59-372">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.Count: 1</span></span>
  * <span data-ttu-id="91e59-373">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Hivatkozás: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="91e59-373">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Reference: http://.../scim/Users/2819c223-7f76-453a-919d-413861904646</span></span>
  * <span data-ttu-id="91e59-374">(Mint PatchRequest2 PatchRequest). Operations.ElementAt(0). Value.ElementAt(0). Érték: 2819c223-7f76-453a-919d-413861904646</span><span class="sxs-lookup"><span data-stu-id="91e59-374">(PatchRequest as PatchRequest2).Operations.ElementAt(0).Value.ElementAt(0).Value: 2819c223-7f76-453a-919d-413861904646</span></span>

6. <span data-ttu-id="91e59-375">toode-provision a felhasználó identitása áruházban fronted SCIM szolgáltatása, az Azure AD egy kérést küld, mint:</span><span class="sxs-lookup"><span data-stu-id="91e59-375">toode-provision a user from an identity store fronted by an SCIM service, Azure AD sends a request such as:</span></span> 
  ````
    DELETE ~/scim/Users/54D382A4-2050-4C03-94D1-E769F1D15682 HTTP/1.1
    Authorization: Bearer ...
  ````
  <span data-ttu-id="91e59-376">Ha hello szolgáltatás SCIM szolgáltatások végrehajtásához a Microsoft által biztosított hello közös nyelvi infrastruktúra könyvtárak segítségével lett létrehozva, majd hello kérelem lefordítását egy hívás toohello hello szolgáltatás szolgáltató Delete metódust.</span><span class="sxs-lookup"><span data-stu-id="91e59-376">If hello service was built using hello Common Language Infrastructure libraries provided by Microsoft for implementing SCIM services, then hello request is translated into a call toohello Delete method of hello service’s provider.</span></span>   <span data-ttu-id="91e59-377">Ez a módszer az aláírása:</span><span class="sxs-lookup"><span data-stu-id="91e59-377">That method has this signature:</span></span> 
  ````
    // System.Threading.Tasks.Tasks is defined in mscorlib.dll.  
    // Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier, 
    // is defined in Microsoft.SystemForCrossDomainIdentityManagement.Protocol. 
    System.Threading.Tasks.Task Delete(
      Microsoft.SystemForCrossDomainIdentityManagement.IResourceIdentifier  
        resourceIdentifier, 
      string correlationIdentifier);
  ````
  <span data-ttu-id="91e59-378">hello hello resourceIdentifier argumentumnak hello értékeként megadott objektumnak a tulajdonságok értékeit az hello példa egy kérelem toode-rendelkezés a felhasználó:</span><span class="sxs-lookup"><span data-stu-id="91e59-378">hello object provided as hello value of hello resourceIdentifier argument has these property values in hello example of a request toode-provision a user:</span></span> 
  
  * <span data-ttu-id="91e59-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span><span class="sxs-lookup"><span data-stu-id="91e59-379">ResourceIdentifier.Identifier: "54D382A4-2050-4C03-94D1-E769F1D15682"</span></span>
  * <span data-ttu-id="91e59-380">ResourceIdentifier.SchemaIdentifier: "urn: ietf:params:scim:schemas:extension:enterprise:2.0:User"</span><span class="sxs-lookup"><span data-stu-id="91e59-380">ResourceIdentifier.SchemaIdentifier: "urn:ietf:params:scim:schemas:extension:enterprise:2.0:User"</span></span>

## <a name="group-provisioning-and-de-provisioning"></a><span data-ttu-id="91e59-381">Csoport üzembe helyezést és megszüntetést</span><span class="sxs-lookup"><span data-stu-id="91e59-381">Group provisioning and de-provisioning</span></span>
<span data-ttu-id="91e59-382">a következő ábra azt mutatja be, hogy Azure AcD tooa SCIM szolgáltatás toomanage hello életciklus csoport elküldi egy másik identitás tárolására köszönőüzenetei hello.</span><span class="sxs-lookup"><span data-stu-id="91e59-382">hello following illustration shows hello messages that Azure AcD sends tooa SCIM service toomanage hello lifecycle of a group in another identity store.</span></span>  <span data-ttu-id="91e59-383">Az üzenetek háromféleképpen toousers vonatkozó köszönőüzenetei különböznek:</span><span class="sxs-lookup"><span data-stu-id="91e59-383">Those messages differ from hello messages pertaining toousers in three ways:</span></span> 

* <span data-ttu-id="91e59-384">egy csoport erőforrás sémája hello http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group azonosítja.</span><span class="sxs-lookup"><span data-stu-id="91e59-384">hello schema of a group resource is identified as http://schemas.microsoft.com/2006/11/ResourceManagement/ADSCIM/Group.</span></span>  
* <span data-ttu-id="91e59-385">Tooretrieve csoportok határozzák meg, hogy hello tagok attribútum kérelmek bármely válasz toohello kérelemben megadott erőforrás kizárását toobe érték.</span><span class="sxs-lookup"><span data-stu-id="91e59-385">Requests tooretrieve groups stipulate that hello members attribute is toobe excluded from any resource provided in response toohello request.</span></span>  
* <span data-ttu-id="91e59-386">Kérelmek toodetermine, hogy rendelkezik-e a hivatkozási attribútum egy adott értékre hello tagok attribútum vonatkozó kérelmek.</span><span class="sxs-lookup"><span data-stu-id="91e59-386">Requests toodetermine whether a reference attribute has a certain value are requests about hello members attribute.</span></span>  

<span data-ttu-id="91e59-387">![][5]
*6. ábra: Az üzembe helyezést és megszüntetést feladatütemezési csoport*</span><span class="sxs-lookup"><span data-stu-id="91e59-387">![][5]
*Figure 6: Group provisioning and de-provisioning sequence*</span></span>

## <a name="related-articles"></a><span data-ttu-id="91e59-388">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="91e59-388">Related articles</span></span>
* [<span data-ttu-id="91e59-389">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="91e59-389">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="91e59-390">Felhasználói kiépítési/megszüntetés tooSaaS alkalmazások automatizálása</span><span class="sxs-lookup"><span data-stu-id="91e59-390">Automate User Provisioning/Deprovisioning tooSaaS Apps</span></span>](active-directory-saas-app-provisioning.md)
* [<span data-ttu-id="91e59-391">A felhasználók átadása attribútum-leképezésekhez testreszabása</span><span class="sxs-lookup"><span data-stu-id="91e59-391">Customizing Attribute Mappings for User Provisioning</span></span>](active-directory-saas-customizing-attribute-mappings.md)
* [<span data-ttu-id="91e59-392">Attribútum-leképezésekhez kifejezések írása</span><span class="sxs-lookup"><span data-stu-id="91e59-392">Writing Expressions for Attribute Mappings</span></span>](active-directory-saas-writing-expressions-for-attribute-mappings.md)
* [<span data-ttu-id="91e59-393">Helyezése Hatókörszűrőkkel felhasználói történő üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="91e59-393">Scoping Filters for User Provisioning</span></span>](active-directory-saas-scoping-filters.md)
* [<span data-ttu-id="91e59-394">Alkalmazás-kiépítési értesítések</span><span class="sxs-lookup"><span data-stu-id="91e59-394">Account Provisioning Notifications</span></span>](active-directory-saas-account-provisioning-notifications.md)
* [<span data-ttu-id="91e59-395">Hogyan kapcsolatos bemutatók felsorolása tooIntegrate SaaS-alkalmazásokhoz</span><span class="sxs-lookup"><span data-stu-id="91e59-395">List of Tutorials on How tooIntegrate SaaS Apps</span></span>](active-directory-saas-tutorial-list.md)

<!--Image references-->
[0]: ./media/active-directory-scim-provisioning/scim-figure-1.PNG
[1]: ./media/active-directory-scim-provisioning/scim-figure-2a.PNG
[2]: ./media/active-directory-scim-provisioning/scim-figure-2b.PNG
[3]: ./media/active-directory-scim-provisioning/scim-figure-3.PNG
[4]: ./media/active-directory-scim-provisioning/scim-figure-4.PNG
[5]: ./media/active-directory-scim-provisioning/scim-figure-5.PNG
