---
title: "aaaActive Directory összevonási szolgáltatások kezelése és testreszabása az Azure AD Connect |} Microsoft Docs"
description: "AD FS felügyelet az Azure AD Connect és felhasználói AD FS bejelentkezési élmény és az Azure AD Connect PowerShell testreszabása."
keywords: "Az AD FS-, ADFS, az AD FS felügyeleti AAD-csatlakozás, csatlakozás, bejelentkezés, az AD FS testreszabás javítani bizalmi kapcsolat, O365, összevonási, függő entitás"
services: active-directory
documentationcenter: 
author: anandyadavmsft
manager: femila
editor: 
ms.assetid: 2593b6c6-dc3f-46ef-8e02-a8e2dc4e9fb9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 361a2bfd6d7a6993dbe773d6ea039ad1afc6346a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-and-customize-active-directory-federation-services-by-using-azure-ad-connect"></a><span data-ttu-id="65ffc-104">Kezelése és testreszabása az Active Directory összevonási szolgáltatások az Azure AD Connect használatával</span><span class="sxs-lookup"><span data-stu-id="65ffc-104">Manage and customize Active Directory Federation Services by using Azure AD Connect</span></span>
<span data-ttu-id="65ffc-105">Ez a cikk ismerteti, hogyan toomanage és testre szabhatja az Active Directory összevonási szolgáltatások (AD FS) Azure Active Directory (Azure AD) Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="65ffc-105">This article describes how toomanage and customize Active Directory Federation Services (AD FS) by using Azure Active Directory (Azure AD) Connect.</span></span> <span data-ttu-id="65ffc-106">Egyéb gyakori AD FS feladatokat, hogy szükség lehet toodo egy AD FS-farm teljes konfiguráció is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="65ffc-106">It also includes other common AD FS tasks that you might need toodo for a complete configuration of an AD FS farm.</span></span>

| <span data-ttu-id="65ffc-107">Témakör</span><span class="sxs-lookup"><span data-stu-id="65ffc-107">Topic</span></span> | <span data-ttu-id="65ffc-108">Ez vonatkozik</span><span class="sxs-lookup"><span data-stu-id="65ffc-108">What it covers</span></span> |
|:--- |:--- |
| <span data-ttu-id="65ffc-109">**Az AD FS kezelése**</span><span class="sxs-lookup"><span data-stu-id="65ffc-109">**Manage AD FS**</span></span> | |
| [<span data-ttu-id="65ffc-110">Hello megbízhatóság javítása</span><span class="sxs-lookup"><span data-stu-id="65ffc-110">Repair hello trust</span></span>](#repairthetrust) |<span data-ttu-id="65ffc-111">Hogyan toorepair hello összevonási megbízhatóság, és az Office 365.</span><span class="sxs-lookup"><span data-stu-id="65ffc-111">How toorepair hello federation trust with Office 365.</span></span> |
| [<span data-ttu-id="65ffc-112">Az Azure AD használatával másodlagos bejelentkezési Azonosítóval összevonni</span><span class="sxs-lookup"><span data-stu-id="65ffc-112">Federate with Azure AD using alternate login ID </span></span>](#alternateid) | <span data-ttu-id="65ffc-113">Másodlagos bejelentkezési azonosítóval összevonás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="65ffc-113">Configure federation using alternate login ID</span></span>  |
| [<span data-ttu-id="65ffc-114">Az AD FS-kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65ffc-114">Add an AD FS server</span></span>](#addadfsserver) |<span data-ttu-id="65ffc-115">Hogyan tooexpand az AD FS farm kerül egy további AD FS-kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="65ffc-115">How tooexpand an AD FS farm with an additional AD FS server.</span></span> |
| [<span data-ttu-id="65ffc-116">AD FS webalkalmazás-Proxy kiszolgáló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65ffc-116">Add an AD FS Web Application Proxy server</span></span>](#addwapserver) |<span data-ttu-id="65ffc-117">Hogyan tooexpand az AD FS farm kerül egy további webalkalmazás-proxykiszolgálóként (WAP) kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="65ffc-117">How tooexpand an AD FS farm with an additional Web Application Proxy (WAP) server.</span></span> |
| [<span data-ttu-id="65ffc-118">Összevont tartomány hozzáadása</span><span class="sxs-lookup"><span data-stu-id="65ffc-118">Add a federated domain</span></span>](#addfeddomain) |<span data-ttu-id="65ffc-119">Hogyan tooadd egy összevont tartományt.</span><span class="sxs-lookup"><span data-stu-id="65ffc-119">How tooadd a federated domain.</span></span> |
| [<span data-ttu-id="65ffc-120">Frissítés hello SSL-tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="65ffc-120">Update hello SSL certificate</span></span>](active-directory-aadconnectfed-ssl-update.md)| <span data-ttu-id="65ffc-121">Hogyan tooupdate hello SSL tanúsítvány egy AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="65ffc-121">How tooupdate hello SSL certificate for an AD FS farm.</span></span> |
| <span data-ttu-id="65ffc-122">**Az AD FS testreszabása**</span><span class="sxs-lookup"><span data-stu-id="65ffc-122">**Customize AD FS**</span></span> | |
| [<span data-ttu-id="65ffc-123">Adjon hozzá egy egyéni vállalati emblémát vagy ábra</span><span class="sxs-lookup"><span data-stu-id="65ffc-123">Add a custom company logo or illustration</span></span>](#customlogo) |<span data-ttu-id="65ffc-124">Hogyan toocustomize az AD FS-Bejelentkezés lapon a vállalati embléma és ábra.</span><span class="sxs-lookup"><span data-stu-id="65ffc-124">How toocustomize an AD FS sign-in page with a company logo and illustration.</span></span> |
| [<span data-ttu-id="65ffc-125">Adjon meg egy bejelentkezési leírást</span><span class="sxs-lookup"><span data-stu-id="65ffc-125">Add a sign-in description</span></span>](#addsignindescription) |<span data-ttu-id="65ffc-126">A bejelentkezés tooadd hogyan lap leírása.</span><span class="sxs-lookup"><span data-stu-id="65ffc-126">How tooadd a sign-in page description.</span></span> |
| [<span data-ttu-id="65ffc-127">AD FS jogcím szabályok módosítása</span><span class="sxs-lookup"><span data-stu-id="65ffc-127">Modify AD FS claim rules</span></span>](#modclaims) |<span data-ttu-id="65ffc-128">Hogyan toomodify AD FS összevonási több, különböző esetekre jogcímek.</span><span class="sxs-lookup"><span data-stu-id="65ffc-128">How toomodify AD FS claims for various federation scenarios.</span></span> |

## <a name="manage-ad-fs"></a><span data-ttu-id="65ffc-129">Az AD FS kezelése</span><span class="sxs-lookup"><span data-stu-id="65ffc-129">Manage AD FS</span></span>
<span data-ttu-id="65ffc-130">Különböző AD FS-sel kapcsolatos feladat hello Azure AD Connect varázsló segítségével elvégezhető az Azure AD Connect minimális felhasználói beavatkozás.</span><span class="sxs-lookup"><span data-stu-id="65ffc-130">You can perform various AD FS-related tasks in Azure AD Connect with minimal user intervention by using hello Azure AD Connect wizard.</span></span> <span data-ttu-id="65ffc-131">Miután befejezte a futó hello varázsló az Azure AD Connect telepítése, hello varázsló futtatásával újra tooperform további feladatokat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-131">After you've finished installing Azure AD Connect by running hello wizard, you can run hello wizard again tooperform additional tasks.</span></span>

## <span data-ttu-id="65ffc-132">Hello megbízhatóság javítása<a name=repairthetrust></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-132">Repair hello trust <a name=repairthetrust></a></span></span>
<span data-ttu-id="65ffc-133">Az Azure AD Connect toocheck hello aktuális állapotának hello AD FS és az Azure AD-megbízhatóság, és megfelelő műveleteket toorepair hello megbízhatósági igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="65ffc-133">You can use Azure AD Connect toocheck hello current health of hello AD FS and Azure AD trust and take appropriate actions toorepair hello trust.</span></span> <span data-ttu-id="65ffc-134">Kövesse ezeket a lépéseket toorepair az Azure AD és az AD FS megbízható.</span><span class="sxs-lookup"><span data-stu-id="65ffc-134">Follow these steps toorepair your Azure AD and AD FS trust.</span></span>

1. <span data-ttu-id="65ffc-135">Válassza ki **javítási aad-ben és az AD FS megbízható** hello további feladatok listája.</span><span class="sxs-lookup"><span data-stu-id="65ffc-135">Select **Repair AAD and ADFS Trust** from hello list of additional tasks.</span></span>
   <span data-ttu-id="65ffc-136">![Javítsa az aad-ben és az AD FS megbízható](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span><span class="sxs-lookup"><span data-stu-id="65ffc-136">![Repair AAD and ADFS Trust](media/active-directory-aadconnect-federation-management/RepairADTrust1.PNG)</span></span>

2. <span data-ttu-id="65ffc-137">A hello **tooAzure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-137">On hello **Connect tooAzure AD** page, provide your global administrator credentials for Azure AD, and click **Next**.</span></span>
   <span data-ttu-id="65ffc-138">![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span><span class="sxs-lookup"><span data-stu-id="65ffc-138">![Connect tooAzure AD](media/active-directory-aadconnect-federation-management/RepairADTrust2.PNG)</span></span>

3. <span data-ttu-id="65ffc-139">A hello **távelérési hitelesítő adatainak** lapján adja meg hello tartományi rendszergazda hello hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="65ffc-139">On hello **Remote access credentials** page, enter hello credentials for hello domain administrator.</span></span>

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/RepairADTrust3.PNG)

    <span data-ttu-id="65ffc-141">Miután rákattintott **következő**, az Azure AD Connect tanúsítványállapot keres, és az esetleges problémákat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="65ffc-141">After you click **Next**, Azure AD Connect checks for certificate health and shows any issues.</span></span>

    ![A tanúsítványok állapota](media/active-directory-aadconnect-federation-management/RepairADTrust4.PNG)

    <span data-ttu-id="65ffc-143">Hello **készen tooconfigure** lapra mutat be hello listája, amely lesz műveletek végre toorepair hello megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="65ffc-143">hello **Ready tooconfigure** page shows hello list of actions that will be performed toorepair hello trust.</span></span>

    ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/RepairADTrust5.PNG)

4. <span data-ttu-id="65ffc-145">Kattintson a **telepítése** toorepair hello megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="65ffc-145">Click **Install** toorepair hello trust.</span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-146">Az Azure AD Connect is csak javítása vagy a törvény önaláírt tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-146">Azure AD Connect can only repair or act on certificates that are self-signed.</span></span> <span data-ttu-id="65ffc-147">Az Azure AD Connect harmadik féltől származó tanúsítványok nem lehet kijavítani.</span><span class="sxs-lookup"><span data-stu-id="65ffc-147">Azure AD Connect can't repair third-party certificates.</span></span>

## <span data-ttu-id="65ffc-148">Az Azure AD használatával AlternateID összevonni<a name=alternateid></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-148">Federate with Azure AD using AlternateID <a name=alternateid></a></span></span>
<span data-ttu-id="65ffc-149">Javasoljuk, hogy hello a helyszíni egyszerű Name(UPN) és egyszerű felhasználónév tartják hello felhő hello azonos.</span><span class="sxs-lookup"><span data-stu-id="65ffc-149">It is recommended that hello on-premises User Principal Name(UPN) and hello cloud User Principal Name are kept hello same.</span></span> <span data-ttu-id="65ffc-150">Ha hello a helyszíni egyszerű Felhasználónévvel (pl. egy nem átirányítható tartományt használja.</span><span class="sxs-lookup"><span data-stu-id="65ffc-150">If hello on-premises UPN uses a non-routable domain (ex.</span></span> <span data-ttu-id="65ffc-151">Contoso.local), vagy nem módosítható miatt toolocal alkalmazásfüggőségek, azt javasoljuk másodlagos bejelentkezési azonosító beállítása</span><span class="sxs-lookup"><span data-stu-id="65ffc-151">Contoso.local) or cannot be changed due toolocal application dependencies, we recommend setting up alternate login ID.</span></span> <span data-ttu-id="65ffc-152">Másodlagos bejelentkezési Azonosítóval is lehetővé teszi a bejelentkezés tapasztalhat, ha nem az egyszerű Felhasználónevük, például a mail attribútum felhasználók bejelentkezhetnek tooconfigure.</span><span class="sxs-lookup"><span data-stu-id="65ffc-152">Alternate login ID allows you tooconfigure a sign-in experience where users can sign in with an attribute other than their UPN, such as mail.</span></span> <span data-ttu-id="65ffc-153">hello választás az egyszerű felhasználónév az Azure AD Connect alapértelmezett toohello userPrincipalName attribútum az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="65ffc-153">hello choice for User Principal Name in Azure AD Connect defaults toohello userPrincipalName attribute in Active Directory.</span></span> <span data-ttu-id="65ffc-154">Ha az attribútum válasszon az egyszerű felhasználónév, és az AD FS segítségével összevonja, majd az Azure AD Connect AD FS beállítják másodlagos bejelentkezési azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="65ffc-154">If you choose any other attribute for User Principal Name and are federating using AD FS, then Azure AD Connect will configure AD FS for alternate login ID.</span></span> <span data-ttu-id="65ffc-155">Egy másik attribútum kiválasztása az egyszerű felhasználónév példát alább találja:</span><span class="sxs-lookup"><span data-stu-id="65ffc-155">An example of choosing a different attribute for User Principal Name is shown below:</span></span>

![Alternatív ID attribútum kiválasztása](media/active-directory-aadconnect-federation-management/attributeselection.png)

<span data-ttu-id="65ffc-157">Másodlagos bejelentkezési azonosító konfigurálása az AD FS két fő lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="65ffc-157">Configuring alternate login ID for AD FS consists of two main steps:</span></span>
1. <span data-ttu-id="65ffc-158">**Kiállítási jogcímek megfelelő halmazát hello konfigurálása**: hello kiállítási jogcímszabályok függő entitás hello Azure AD megbízik áll a másik azonosító hello felhasználó hello módosított toouse kijelölt hello UserPrincipalName attribútum.</span><span class="sxs-lookup"><span data-stu-id="65ffc-158">**Configure hello right set of issuance claims**: hello issuance claim rules in hello Azure AD relying party trust are modified toouse hello selected UserPrincipalName attribute as hello alternate ID of hello user.</span></span>
2. <span data-ttu-id="65ffc-159">**Engedélyezze a másodlagos bejelentkezési Azonosítóval is hello AD FS konfiguráció**: hello AD FS konfigurációs frissül, hogy az AD FS hello alternatív azonosítójával hello megfelelő erdőkben található felhasználókat is kereshet.</span><span class="sxs-lookup"><span data-stu-id="65ffc-159">**Enable alternate login ID in hello AD FS configuration**: hello AD FS configuration is updated so that AD FS can look up users in hello appropriate forests using hello alternate ID.</span></span> <span data-ttu-id="65ffc-160">Ez a konfiguráció az AD FS Windows Server 2012 R2 (az KB2919355) vagy újabb rendszeren támogatott.</span><span class="sxs-lookup"><span data-stu-id="65ffc-160">This configuration is supported for AD FS on Windows Server 2012 R2 (with KB2919355) or later.</span></span> <span data-ttu-id="65ffc-161">Ha hello AD FS-kiszolgáló 2012 R2, a hello hello meglétének ellenőrzése az Azure AD Connect KB szükséges.</span><span class="sxs-lookup"><span data-stu-id="65ffc-161">If hello AD FS servers are 2012 R2, Azure AD Connect checks for hello presence of hello required KB.</span></span> <span data-ttu-id="65ffc-162">Ha a hello KB nem észlel, egy figyelmeztetés jelenik meg konfiguráció befejezése után alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="65ffc-162">If hello KB is not detected, a warning will be displayed after configuration completes, as shown below:</span></span>

    ![A hiányzó KB 2012R2 a figyelmeztetés](media/active-directory-aadconnect-federation-management/kbwarning.png)

    <span data-ttu-id="65ffc-164">toorectify hello konfigurációs hiányzó KB esetén telepítse a szükséges hello [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) , és javítsa a hello megbízhatósági használatával [javítása aad-ben és az AD FS-megbízhatóság](#repairthetrust).</span><span class="sxs-lookup"><span data-stu-id="65ffc-164">toorectify hello configuration in case of missing KB, install hello required [KB2919355](http://go.microsoft.com/fwlink/?LinkID=396590) and then repair hello trust using [Repair AAD and AD FS Trust](#repairthetrust).</span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-165">További információ a alternateID és lépéseket toomanually konfigurálása, olvassa el [másodlagos bejelentkezési azonosító beállítása](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span><span class="sxs-lookup"><span data-stu-id="65ffc-165">For more information on alternateID and steps toomanually configure, read [Configuring Alternate Login ID](https://technet.microsoft.com/windows-server-docs/identity/ad-fs/operations/configuring-alternate-login-id)</span></span>

## <span data-ttu-id="65ffc-166">Az AD FS-kiszolgáló hozzáadása<a name=addadfsserver></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-166">Add an AD FS server <a name=addadfsserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-167">tooadd az AD FS-kiszolgáló, az Azure AD Connect hello PFX-tanúsítványokat igényel.</span><span class="sxs-lookup"><span data-stu-id="65ffc-167">tooadd an AD FS server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="65ffc-168">Ezért a művelet végrehajtása, csak ha hello AD FS-farm konfigurálta az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="65ffc-168">Therefore, you can perform this operation only if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="65ffc-169">Válassza ki **további összevonási kiszolgáló üzembe helyezése**, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-169">Select **Deploy an additional Federation Server**, and click **Next**.</span></span>

   ![További összevonási kiszolgáló](media/active-directory-aadconnect-federation-management/AddNewADFSServer1.PNG)

2. <span data-ttu-id="65ffc-171">A hello **tooAzure AD Connect** lapon adja meg az Azure AD globális rendszergazdai hitelesítő adataival, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-171">On hello **Connect tooAzure AD** page, enter your global administrator credentials for Azure AD, and click **Next**.</span></span>

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/AddNewADFSServer2.PNG)

3. <span data-ttu-id="65ffc-173">Hello tartományi rendszergazdai hitelesítő adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="65ffc-173">Provide hello domain administrator credentials.</span></span>

   ![Tartományi rendszergazda hitelesítő adatai](media/active-directory-aadconnect-federation-management/AddNewADFSServer3.PNG)

4. <span data-ttu-id="65ffc-175">Az Azure AD Connect hello PFX-fájl az új AD FS-farm konfigurálása az Azure AD Connect közben megadott hello jelszót kér.</span><span class="sxs-lookup"><span data-stu-id="65ffc-175">Azure AD Connect asks for hello password of hello PFX file that you provided while configuring your new AD FS farm with Azure AD Connect.</span></span> <span data-ttu-id="65ffc-176">Kattintson a **jelszó megadása** tooprovide hello jelszó hello PFX-fájljának.</span><span class="sxs-lookup"><span data-stu-id="65ffc-176">Click **Enter Password** tooprovide hello password for hello PFX file.</span></span>

   ![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/AddNewADFSServer4.PNG)

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/AddNewADFSServer5.PNG)

5. <span data-ttu-id="65ffc-179">A hello **AD FS-kiszolgálók** lapján adja meg a hello kiszolgáló neve vagy IP-cím toobe hozzáadott toohello AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="65ffc-179">On hello **AD FS Servers** page, enter hello server name or IP address toobe added toohello AD FS farm.</span></span>

   ![AD FS-kiszolgálók](media/active-directory-aadconnect-federation-management/AddNewADFSServer6.PNG)

6. <span data-ttu-id="65ffc-181">Kattintson a **következő**, és halad át a végső hello **konfigurálása** lap.</span><span class="sxs-lookup"><span data-stu-id="65ffc-181">Click **Next**, and go through hello final **Configure** page.</span></span> <span data-ttu-id="65ffc-182">Miután az Azure AD Connect befejezte a kiszolgálók toohello AD FS-farm hello hozzáadása, az aktiválási hello beállítás tooverify hello kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-182">After Azure AD Connect has finished adding hello servers toohello AD FS farm, you will be given hello option tooverify hello connectivity.</span></span>

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/AddNewADFSServer7.PNG)

    ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/AddNewADFSServer8.PNG)

## <span data-ttu-id="65ffc-185">AD FS WAP-kiszolgáló hozzáadása<a name=addwapserver></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-185">Add an AD FS WAP server <a name=addwapserver></a></span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-186">a WAP-kiszolgáló tooadd, az Azure AD Connect hello PFX-tanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="65ffc-186">tooadd a WAP server, Azure AD Connect requires hello PFX certificate.</span></span> <span data-ttu-id="65ffc-187">Ezért csak végezheti el a művelet Ha hello AD FS-farm konfigurálta az Azure AD Connect használatával.</span><span class="sxs-lookup"><span data-stu-id="65ffc-187">Therefore, you can only perform this operation if you configured hello AD FS farm by using Azure AD Connect.</span></span>

1. <span data-ttu-id="65ffc-188">Válassza ki **webalkalmazás-Proxy telepítése** hello rendelkezésre álló feladatok közül.</span><span class="sxs-lookup"><span data-stu-id="65ffc-188">Select **Deploy Web Application Proxy** from hello list of available tasks.</span></span>

   ![Webalkalmazás-Proxy telepítéséhez](media/active-directory-aadconnect-federation-management/WapServer1.PNG)

2. <span data-ttu-id="65ffc-190">Adja meg a hello Azure globális rendszergazdai hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-190">Provide hello Azure global administrator credentials.</span></span>

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/wapserver2.PNG)

3. <span data-ttu-id="65ffc-192">A hello **adja meg az SSL-tanúsítvány** lapon, az Azure AD Connect hello AD FS-farm konfigurálása során megadott hello PFX-fájl hello jelszót adjon meg.</span><span class="sxs-lookup"><span data-stu-id="65ffc-192">On hello **Specify SSL certificate** page, provide hello password for hello PFX file that you provided when you configured hello AD FS farm with Azure AD Connect.</span></span>
   <span data-ttu-id="65ffc-193">![Tanúsítvány jelszava](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span><span class="sxs-lookup"><span data-stu-id="65ffc-193">![Certificate password](media/active-directory-aadconnect-federation-management/WapServer3.PNG)</span></span>

    ![Adja meg az SSL-tanúsítvány](media/active-directory-aadconnect-federation-management/WapServer4.PNG)

4. <span data-ttu-id="65ffc-195">Hello server toobe fel, mert a WAP-kiszolgáló hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="65ffc-195">Add hello server toobe added as a WAP server.</span></span> <span data-ttu-id="65ffc-196">Hello WAP-kiszolgáló nem feltétlenül illesztett toohello tartományban, mert hello varázsló hozzáadni kívánt toohello kiszolgáló rendszergazdai hitelesítő adatokat kér.</span><span class="sxs-lookup"><span data-stu-id="65ffc-196">Because hello WAP server might not be joined toohello domain, hello wizard asks for administrative credentials toohello server being added.</span></span>

   ![A felügyeleti kiszolgáló hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer5.PNG)

5. <span data-ttu-id="65ffc-198">A hello **Proxy megbízhatósági hitelesítő adatok** lapján adja meg a rendszergazdai hitelesítő adatokkal tooconfigure hello proxykiszolgáló megbízhatóság és a hozzáférés hello elsődleges kiszolgáló hello AD FS-farm.</span><span class="sxs-lookup"><span data-stu-id="65ffc-198">On hello **Proxy trust credentials** page, provide administrative credentials tooconfigure hello proxy trust and access hello primary server in hello AD FS farm.</span></span>

   ![Proxy megbízhatósági hitelesítő adatai](media/active-directory-aadconnect-federation-management/WapServer6.PNG)

6. <span data-ttu-id="65ffc-200">A hello **készen tooconfigure** lap, a hello varázsló végrehajtott műveletek hello listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="65ffc-200">On hello **Ready tooconfigure** page, hello wizard shows hello list of actions that will be performed.</span></span>

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/WapServer7.PNG)

7. <span data-ttu-id="65ffc-202">Kattintson a **telepítése** toofinish hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="65ffc-202">Click **Install** toofinish hello configuration.</span></span> <span data-ttu-id="65ffc-203">Hello konfiguráció befejezése után hello varázsló ad meg a beállítás tooverify hello hello kapcsolat toohello kiszolgálók.</span><span class="sxs-lookup"><span data-stu-id="65ffc-203">After hello configuration is complete, hello wizard gives you hello option tooverify hello connectivity toohello servers.</span></span> <span data-ttu-id="65ffc-204">Kattintson a **ellenőrizze** toocheck kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-204">Click **Verify** toocheck connectivity.</span></span>

   ![A telepítés befejeződött](media/active-directory-aadconnect-federation-management/WapServer8.PNG)

## <span data-ttu-id="65ffc-206">Összevont tartomány hozzáadása<a name=addfeddomain></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-206">Add a federated domain <a name=addfeddomain></a></span></span>

<span data-ttu-id="65ffc-207">A tartomány toobe összevont Azure AD-val az Azure AD Connect használatával könnyen tooadd.</span><span class="sxs-lookup"><span data-stu-id="65ffc-207">It's easy tooadd a domain toobe federated with Azure AD by using Azure AD Connect.</span></span> <span data-ttu-id="65ffc-208">Az Azure AD Connect hello tartományt összevonásra hozzáadja, és módosítja a hello jogcím szabályok toocorrectly hello kibocsátó tükrözik, és az Azure AD összevont több tartomány esetén.</span><span class="sxs-lookup"><span data-stu-id="65ffc-208">Azure AD Connect adds hello domain for federation and modifies hello claim rules toocorrectly reflect hello issuer when you have multiple domains federated with Azure AD.</span></span>

1. <span data-ttu-id="65ffc-209">tooadd összevont tartományt, jelölje be hello feladat **adnia egy további Azure AD-tartomány**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-209">tooadd a federated domain, select hello task **Add an additional Azure AD domain**.</span></span>

   ![További Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain1.PNG)

2. <span data-ttu-id="65ffc-211">Hello hello varázsló következő lapján adja meg az Azure AD hello globális rendszergazda hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="65ffc-211">On hello next page of hello wizard, provide hello global administrator credentials for Azure AD.</span></span>

   ![TooAzure AD Connect](media/active-directory-aadconnect-federation-management/AdditionalDomain2.PNG)

3. <span data-ttu-id="65ffc-213">A hello **távelérési hitelesítő adatainak** lapján hello tartományi rendszergazdai hitelesítő adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="65ffc-213">On hello **Remote access credentials** page, provide hello domain administrator credentials.</span></span>

   ![Távoli hozzáférési hitelesítő adatok](media/active-directory-aadconnect-federation-management/additionaldomain3.PNG)

4. <span data-ttu-id="65ffc-215">A következő lapon hello a hello varázsló lehet összevonást végezni a helyszíni címtár az Azure AD-tartományok listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="65ffc-215">On hello next page, hello wizard provides a list of Azure AD domains that you can federate your on-premises directory with.</span></span> <span data-ttu-id="65ffc-216">Válassza ki a hello tartomány hello listából.</span><span class="sxs-lookup"><span data-stu-id="65ffc-216">Choose hello domain from hello list.</span></span>

   ![Az Azure AD-tartomány](media/active-directory-aadconnect-federation-management/AdditionalDomain4.PNG)

    <span data-ttu-id="65ffc-218">Hello tartomány kiválasztása után hello varázsló segítségével, akkor megfelelő információkkal kapcsolatos további műveleteket varázsló hello fogad, majd hello hello konfigurációs hatását.</span><span class="sxs-lookup"><span data-stu-id="65ffc-218">After you choose hello domain, hello wizard provides you with appropriate information about further actions that hello wizard will take and hello impact of hello configuration.</span></span> <span data-ttu-id="65ffc-219">Bizonyos esetekben egy tartományhoz, amely nincs még ellenőrzése az Azure AD-hello varázsló nyújt információkat toohelp választásakor ellenőriznie hello tartomány.</span><span class="sxs-lookup"><span data-stu-id="65ffc-219">In some cases, if you select a domain that isn't yet verified in Azure AD, hello wizard provides you with information toohelp you verify hello domain.</span></span> <span data-ttu-id="65ffc-220">Lásd: [adja hozzá az egyéni tartomány nevét tooAzure Active Directory](../active-directory-add-domain.md) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="65ffc-220">See [Add your custom domain name tooAzure Active Directory](../active-directory-add-domain.md) for more details.</span></span>

5. <span data-ttu-id="65ffc-221">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="65ffc-221">Click **Next**.</span></span> <span data-ttu-id="65ffc-222">Hello **készen tooconfigure** lap, amely az Azure AD Connect hajtja végre műveletek hello listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="65ffc-222">hello **Ready tooconfigure** page shows hello list of actions that Azure AD Connect will perform.</span></span> <span data-ttu-id="65ffc-223">Kattintson a **telepítése** toofinish hello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="65ffc-223">Click **Install** toofinish hello configuration.</span></span>

   ![Készen áll a tooconfigure](media/active-directory-aadconnect-federation-management/AdditionalDomain5.PNG)

> [!NOTE]
> <span data-ttu-id="65ffc-225">Hello felhasználóit hozzá nem tud toologin tooAzure AD összevont tartományt kell szinkronizálni.</span><span class="sxs-lookup"><span data-stu-id="65ffc-225">Users from hello added federated domain must be synchronized before they will be able toologin tooAzure AD.</span></span>

## <a name="ad-fs-customization"></a><span data-ttu-id="65ffc-226">AD FS Testreszabás</span><span class="sxs-lookup"><span data-stu-id="65ffc-226">AD FS customization</span></span>
<span data-ttu-id="65ffc-227">hello következő szakaszok részletesen bemutatják hello gyakori feladatokat, hogy tooperform előfordulhat, ha a az AD FS bejelentkezési oldal testreszabása.</span><span class="sxs-lookup"><span data-stu-id="65ffc-227">hello following sections provide details about some of hello common tasks that you might have tooperform when you customize your AD FS sign-in page.</span></span>

## <span data-ttu-id="65ffc-228">Adjon hozzá egy egyéni vállalati emblémát vagy ábra<a name=customlogo></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-228">Add a custom company logo or illustration <a name=customlogo></a></span></span>
<span data-ttu-id="65ffc-229">toochange hello embléma hello vállalat hello megjelenő **bejelentkezési** lapján hello a következő Windows PowerShell-parancsmagját és szintaxisát használja.</span><span class="sxs-lookup"><span data-stu-id="65ffc-229">toochange hello logo of hello company that's displayed on hello **Sign-in** page, use hello following Windows PowerShell cmdlet and syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-230">ajánlott a dimenziók a hello embléma 260 x 35 pixel, legfeljebb 10 KB-os fájlméretben 96 dpi hello.</span><span class="sxs-lookup"><span data-stu-id="65ffc-230">hello recommended dimensions for hello logo are 260 x 35 @ 96 dpi with a file size no greater than 10 KB.</span></span>

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [!NOTE]
> <span data-ttu-id="65ffc-231">Hello *TargetName* paraméter megadása kötelező.</span><span class="sxs-lookup"><span data-stu-id="65ffc-231">hello *TargetName* parameter is required.</span></span> <span data-ttu-id="65ffc-232">hello alapértelmezett témájának az AD FS alapértelmezett neve.</span><span class="sxs-lookup"><span data-stu-id="65ffc-232">hello default theme that's released with AD FS is named Default.</span></span>

## <span data-ttu-id="65ffc-233">Adjon meg egy bejelentkezési leírást<a name=addsignindescription></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-233">Add a sign-in description <a name=addsignindescription></a></span></span>
<span data-ttu-id="65ffc-234">a bejelentkezési oldal leírás toohello tooadd **bejelentkezési oldal**, hello a következő Windows PowerShell-parancsmagját és szintaxisát használja.</span><span class="sxs-lookup"><span data-stu-id="65ffc-234">tooadd a sign-in page description toohello **Sign-in page**, use hello following Windows PowerShell cmdlet and syntax.</span></span>

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in tooContoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

## <span data-ttu-id="65ffc-235">AD FS jogcím szabályok módosítása<a name=modclaims></a></span><span class="sxs-lookup"><span data-stu-id="65ffc-235">Modify AD FS claim rules <a name=modclaims></a></span></span>
<span data-ttu-id="65ffc-236">Az AD FS támogatja a gazdag jogcímszabályokat nyelv használható toocreate egyéni jogcímszabályokat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-236">AD FS supports a rich claim language that you can use toocreate custom claim rules.</span></span> <span data-ttu-id="65ffc-237">További információkért lásd: [hello Jogcímszabály nyelvének szerepe hello](https://technet.microsoft.com/library/dd807118.aspx).</span><span class="sxs-lookup"><span data-stu-id="65ffc-237">For more information, see [hello Role of hello Claim Rule Language](https://technet.microsoft.com/library/dd807118.aspx).</span></span>

<span data-ttu-id="65ffc-238">hello következő részek a hogyan írhat egyéni szabályok a lehetséges, hogy tooAzure AD és az AD FS összevonási vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="65ffc-238">hello following sections describe how you can write custom rules for some scenarios that relate tooAzure AD and AD FS federation.</span></span>

### <a name="immutable-id-conditional-on-a-value-being-present-in-hello-attribute"></a><span data-ttu-id="65ffc-239">Feltételes a hello attribútum szerepel egy érték nem módosítható azonosítója</span><span class="sxs-lookup"><span data-stu-id="65ffc-239">Immutable ID conditional on a value being present in hello attribute</span></span>
<span data-ttu-id="65ffc-240">Az Azure AD Connect lehetővé teszi, hogy az attribútum toobe használt objektumok esetén egy forráshorgonyt szinkronizálva tooAzure AD meg.</span><span class="sxs-lookup"><span data-stu-id="65ffc-240">Azure AD Connect lets you specify an attribute toobe used as a source anchor when objects are synced tooAzure AD.</span></span> <span data-ttu-id="65ffc-241">Ha hello hello egyéni attribútum értéke nem üres, érdemes tooissue egy nem módosítható Azonosítót jogcímet.</span><span class="sxs-lookup"><span data-stu-id="65ffc-241">If hello value in hello custom attribute is not empty, you might want tooissue an immutable ID claim.</span></span>

<span data-ttu-id="65ffc-242">Például kiválaszthatja **ms-ds-consistencyguid** hello attribútumaként hello forráshorgony és probléma **ImmutableID** , **ms-ds-consistencyguid** az eset hello attribútum értéke rajta.</span><span class="sxs-lookup"><span data-stu-id="65ffc-242">For example, you might select **ms-ds-consistencyguid** as hello attribute for hello source anchor and issue **ImmutableID** as **ms-ds-consistencyguid** in case hello attribute has a value against it.</span></span> <span data-ttu-id="65ffc-243">Ha nincs érték hello attribútum ellen, **objectGuid** , hello nem módosítható azonosítót.</span><span class="sxs-lookup"><span data-stu-id="65ffc-243">If there's no value against hello attribute, issue **objectGuid** as hello immutable ID.</span></span> <span data-ttu-id="65ffc-244">Hogyan hozhat létre egyéni jogcímszabályok hello készlete, hello a következő szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="65ffc-244">You can construct hello set of custom claim rules as described in hello following section.</span></span>

<span data-ttu-id="65ffc-245">**1. szabály: Lekérdezés attribútumok**</span><span class="sxs-lookup"><span data-stu-id="65ffc-245">**Rule 1: Query attributes**</span></span>

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

<span data-ttu-id="65ffc-246">Ebben a szabályban hello értékének lekérdezésére **ms-ds-consistencyguid** és **objectGuid** hello felhasználóhoz az Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="65ffc-246">In this rule, you're querying hello values of **ms-ds-consistencyguid** and **objectGuid** for hello user from Active Directory.</span></span> <span data-ttu-id="65ffc-247">Hello tároló neve tooan megfelelő tároló nevének módosítása a az AD FS üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="65ffc-247">Change hello store name tooan appropriate store name in your AD FS deployment.</span></span> <span data-ttu-id="65ffc-248">Meghatározott is módosítani a hello jogcím típusa tooa megfelelő jogcím típusát az összevonási **objectGuid** és **ms-ds-consistencyguid**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-248">Also change hello claims type tooa proper claims type for your federation, as defined for **objectGuid** and **ms-ds-consistencyguid**.</span></span>

<span data-ttu-id="65ffc-249">Továbbá használatával **hozzáadása** és nem **probléma**, elkerülheti a kimenő problémát hello entitás hozzáadása, és köztes értékként hello értékeket is használhat.</span><span class="sxs-lookup"><span data-stu-id="65ffc-249">Also, by using **add** and not **issue**, you avoid adding an outgoing issue for hello entity, and can use hello values as intermediate values.</span></span> <span data-ttu-id="65ffc-250">Egy újabb szabály hello jogcímet állít ki, melyik érték toouse, nem módosítható azonosítót. hello létrehozása után</span><span class="sxs-lookup"><span data-stu-id="65ffc-250">You will issue hello claim in a later rule after you establish which value toouse as hello immutable ID.</span></span>

<span data-ttu-id="65ffc-251">**2. szabály: Ellenőrizze, hogy létezik-e az ms-ds-consistencyguid hello felhasználó**</span><span class="sxs-lookup"><span data-stu-id="65ffc-251">**Rule 2: Check if ms-ds-consistencyguid exists for hello user**</span></span>

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

<span data-ttu-id="65ffc-252">Ez a szabály meghatározása nevű ideiglenes jelzőt **idflag** , amely túl van-e állítva**useguid** esetén nem **ms-ds-consistencyguid** hello felhasználó feltöltve.</span><span class="sxs-lookup"><span data-stu-id="65ffc-252">This rule defines a temporary flag called **idflag** that is set too**useguid** if there's no **ms-ds-consistencyguid** populated for hello user.</span></span> <span data-ttu-id="65ffc-253">hello logika ez mögött hello tényt, hogy az AD FS nem engedélyezi üres jogcímeket.</span><span class="sxs-lookup"><span data-stu-id="65ffc-253">hello logic behind this is hello fact that AD FS doesn't allow empty claims.</span></span> <span data-ttu-id="65ffc-254">Ezért amikor hozzáadja jogcímek http://contoso.com/ws/2016/02/identity/claims/objectguid és http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid Rule 1, hogy végül egy **msdsconsistencyguid** jogcímet csak akkor, ha hello érték hello felhasználó fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="65ffc-254">So when you add claims http://contoso.com/ws/2016/02/identity/claims/objectguid and http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid in Rule 1, you end up with an **msdsconsistencyguid** claim only if hello value is populated for hello user.</span></span> <span data-ttu-id="65ffc-255">Ha azt nem üres, az AD FS látja, hogy egy üres érték fog szerepelni, elutasítja azokat azonnal.</span><span class="sxs-lookup"><span data-stu-id="65ffc-255">If it isn't populated, AD FS sees that it will have an empty value and drops it immediately.</span></span> <span data-ttu-id="65ffc-256">Minden objektumot kell **objectGuid**, így az igény mindig van 1. szabály végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="65ffc-256">All objects will have **objectGuid**, so that claim will always be there after Rule 1 is executed.</span></span>

<span data-ttu-id="65ffc-257">**3. szabály: Ki az ms-ds-consistencyguid ID nem módosítható, ha telepítve**</span><span class="sxs-lookup"><span data-stu-id="65ffc-257">**Rule 3: Issue ms-ds-consistencyguid as immutable ID if it's present**</span></span>

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconsistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

<span data-ttu-id="65ffc-258">Ez az egy implicit **Exist** ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="65ffc-258">This is an implicit **Exist** check.</span></span> <span data-ttu-id="65ffc-259">Ha hello értéket hello jogcímet, hogyan adhat ki, hogy hello nem módosítható, azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="65ffc-259">If hello value for hello claim exists, then issue that as hello immutable ID.</span></span> <span data-ttu-id="65ffc-260">hello előző példa hello **nameidentifier** jogcímek.</span><span class="sxs-lookup"><span data-stu-id="65ffc-260">hello previous example uses hello **nameidentifier** claim.</span></span> <span data-ttu-id="65ffc-261">Összekapcsolta toochange a toohello megfelelő jogcímtípus hello nem módosítható a(z) a környezetben.</span><span class="sxs-lookup"><span data-stu-id="65ffc-261">You'll have toochange this toohello appropriate claim type for hello immutable ID in your environment.</span></span>

<span data-ttu-id="65ffc-262">**4. szabály: Ki objectGuid ID nem módosítható, ha az ms-ds-consistencyGuid nincs jelen**</span><span class="sxs-lookup"><span data-stu-id="65ffc-262">**Rule 4: Issue objectGuid as immutable ID if ms-ds-consistencyGuid is not present**</span></span>

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

<span data-ttu-id="65ffc-263">A szabály, akkor még egyszerűen ellenőrzése hello ideiglenes jelző **idflag**.</span><span class="sxs-lookup"><span data-stu-id="65ffc-263">In this rule, you're simply checking hello temporary flag **idflag**.</span></span> <span data-ttu-id="65ffc-264">Úgy dönt, hogy tooissue hello igénylés alapú annak értékét.</span><span class="sxs-lookup"><span data-stu-id="65ffc-264">You decide whether tooissue hello claim based on its value.</span></span>

> [!NOTE]
> <span data-ttu-id="65ffc-265">Ezek a szabályok sorrendjének hello fontos.</span><span class="sxs-lookup"><span data-stu-id="65ffc-265">hello sequence of these rules is important.</span></span>

### <a name="sso-with-a-subdomain-upn"></a><span data-ttu-id="65ffc-266">Egyszerű felhasználónév altartomány egyszeri bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="65ffc-266">SSO with a subdomain UPN</span></span>
<span data-ttu-id="65ffc-267">Összevont Azure AD Connect használatával, a több tartomány toobe is hozzáadhat [hozzá egy új összevont tartományt](active-directory-aadconnect-federation-management.md#addfeddomain).</span><span class="sxs-lookup"><span data-stu-id="65ffc-267">You can add more than one domain toobe federated by using Azure AD Connect, as described in [Add a new federated domain](active-directory-aadconnect-federation-management.md#addfeddomain).</span></span> <span data-ttu-id="65ffc-268">Módosítania kell hello felhasználó egyszerű felhasználónév (UPN) jogcím, hogy hello kibocsátó azonosítója megegyezik toohello gyökértartományt, valamint nem hello altartomány, mert hello összevont gyökértartomány is magában foglalja a hello gyermek.</span><span class="sxs-lookup"><span data-stu-id="65ffc-268">You must modify hello user principal name (UPN) claim so that hello issuer ID corresponds toohello root domain and not hello subdomain, because hello federated root domain also covers hello child.</span></span>

<span data-ttu-id="65ffc-269">Alapértelmezés szerint hello jogcímszabály azonosítója be van állítva az kibocsátó:</span><span class="sxs-lookup"><span data-stu-id="65ffc-269">By default, hello claim rule for issuer ID is set as:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![Alapértelmezett kibocsátó azonosító jogcím](media/active-directory-aadconnect-federation-management/issuer_id_default.png)

<span data-ttu-id="65ffc-271">hello alapértelmezett szabály egyszerűen hello egyszerű Felhasználónévi utótagot vesz igénybe, és használja hello kibocsátó azonosító jogcímek.</span><span class="sxs-lookup"><span data-stu-id="65ffc-271">hello default rule simply takes hello UPN suffix and uses it in hello issuer ID claim.</span></span> <span data-ttu-id="65ffc-272">Például John sub.contoso.com felhasználóként, és contoso.com össze van vonva az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="65ffc-272">For example, John is a user in sub.contoso.com, and contoso.com is federated with Azure AD.</span></span> <span data-ttu-id="65ffc-273">John megadja john@sub.contoso.com , a felhasználónév hello tooAzure AD bejelentkezés során.</span><span class="sxs-lookup"><span data-stu-id="65ffc-273">John enters john@sub.contoso.com as hello username while signing in tooAzure AD.</span></span> <span data-ttu-id="65ffc-274">hello alapértelmezett kibocsátó azonosító jogcímszabály az AD FS kezeli a következő módon hello:</span><span class="sxs-lookup"><span data-stu-id="65ffc-274">hello default issuer ID claim rule in AD FS handles it in hello following manner:</span></span>

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

<span data-ttu-id="65ffc-275">**A jogcím értéke:** http://sub.contoso.com/adfs/services/trust/</span><span class="sxs-lookup"><span data-stu-id="65ffc-275">**Claim value:**  http://sub.contoso.com/adfs/services/trust/</span></span>

<span data-ttu-id="65ffc-276">toohave csak hello gyökértartomány hello kiállító jogcím értéke, hello jogcím szabály toomatch hello következő módosítása:</span><span class="sxs-lookup"><span data-stu-id="65ffc-276">toohave only hello root domain in hello issuer claim value, change hello claim rule toomatch hello following:</span></span>

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a><span data-ttu-id="65ffc-277">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="65ffc-277">Next steps</span></span>
<span data-ttu-id="65ffc-278">További információ [felhasználói bejelentkezés lehetőségei](active-directory-aadconnect-user-signin.md).</span><span class="sxs-lookup"><span data-stu-id="65ffc-278">Learn more about [user sign-in options](active-directory-aadconnect-user-signin.md).</span></span>
