---
title: "és az Azure AD Connect-szinkronizálás jelszó-szinkronizálás aaaTroubleshoot |} Microsoft Docs"
description: "Ez a cikk nyújt információt tootroubleshoot jelszó-szinkronizálási hibák."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a><span data-ttu-id="08359-103">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="08359-103">Troubleshoot password synchronization with Azure AD Connect sync</span></span>
<span data-ttu-id="08359-104">Ez a témakör a hogyan tootroubleshoot állít ki a jelszó-szinkronizálás lépéseit ismerteti.</span><span class="sxs-lookup"><span data-stu-id="08359-104">This topic provides steps for how tootroubleshoot issues with password synchronization.</span></span> <span data-ttu-id="08359-105">Jelszavak nem szinkronizál a várt módon, ha azok a felhasználók egy része vagy az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="08359-105">If passwords are not synchronizing as expected, it can be either for a subset of users or for all users.</span></span> <span data-ttu-id="08359-106">Az Azure Active Directory (Azure AD) telepítési csatlakozás verziója 1.1.524.0, vagy később, már létezik egy diagnosztikai parancsmag használható tootroubleshoot jelszó-szinkronizálás problémákat:</span><span class="sxs-lookup"><span data-stu-id="08359-106">For Azure Active Directory (Azure AD) Connect deployment with version 1.1.524.0 or later, there is now a diagnostic cmdlet that you can use tootroubleshoot password synchronization issues:</span></span>

* <span data-ttu-id="08359-107">Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a toohello [nem jelszavai szinkronizálva vannak: hello diagnosztikai parancsmaggal hibaelhárításához](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-107">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

* <span data-ttu-id="08359-108">Ha az egyes objektumok problémát, tekintse meg a toohello [több objektum van nem jelszó-szinkronizálás: hello diagnosztikai parancsmaggal hibaelhárításához](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-108">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

<span data-ttu-id="08359-109">Az Azure AD Connect telepítési korábbi verzióival:</span><span class="sxs-lookup"><span data-stu-id="08359-109">For older versions of Azure AD Connect deployment:</span></span>

* <span data-ttu-id="08359-110">Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a toohello [nem jelszavai szinkronizálva vannak: hibaelhárítási manuális](#no-passwords-are-synchronized-manual-troubleshooting-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-110">If you have an issue where no passwords are synchronized, refer toohello [No passwords are synchronized: manual troubleshooting steps](#no-passwords-are-synchronized-manual-troubleshooting-steps) section.</span></span>

* <span data-ttu-id="08359-111">Ha az egyes objektumok problémát, tekintse meg a toohello [több objektum van nem jelszó-szinkronizálás: hibaelhárítási manuális](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-111">If you have an issue with individual objects, refer toohello [One object is not synchronizing passwords: manual troubleshooting steps](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) section.</span></span>

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="08359-112">Nincs jelszavai szinkronizálva vannak: hello diagnosztikai parancsmaggal hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="08359-112">No passwords are synchronized: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="08359-113">Használhatja a hello `Invoke-ADSyncDiagnostics` parancsmag toofigure, miért nem jelszavak szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="08359-113">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toofigure out why no passwords are synchronized.</span></span>

> [!NOTE]
> <span data-ttu-id="08359-114">Hello `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="08359-114">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="08359-115">Hello diagnosztika parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="08359-115">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="08359-116">ahol nincs jelszavai szinkronizálva vannak az tootroubleshoot problémák:</span><span class="sxs-lookup"><span data-stu-id="08359-116">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="08359-117">Nyisson meg egy új Windows PowerShell-munkamenetet az Azure AD Connect kiszolgálón hello **Futtatás rendszergazdaként** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="08359-117">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="08359-118">Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="08359-118">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="08359-119">Futtassa az `Import-Module ADSyncDiagnostics` parancsot.</span><span class="sxs-lookup"><span data-stu-id="08359-119">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="08359-120">Futtassa az `Invoke-ADSyncDiagnostics -PasswordSync` parancsot.</span><span class="sxs-lookup"><span data-stu-id="08359-120">Run `Invoke-ADSyncDiagnostics -PasswordSync`.</span></span>

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="08359-121">Hello eredmények hello parancsmag ismertetése</span><span class="sxs-lookup"><span data-stu-id="08359-121">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="08359-122">hello diagnosztikai parancsmag a következő ellenőrzések hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="08359-122">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="08359-123">Ellenőrzi, hogy hello jelszó-szinkronizálási szolgáltatás engedélyezve van az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="08359-123">Validates that hello password synchronization feature is enabled for your Azure AD tenant.</span></span>

* <span data-ttu-id="08359-124">Ellenőrzi, hogy hello Azure AD Connect kiszolgáló nem átmeneti módban van.</span><span class="sxs-lookup"><span data-stu-id="08359-124">Validates that hello Azure AD Connect server is not in staging mode.</span></span>

* <span data-ttu-id="08359-125">Az egyes meglévő helyszíni Active Directory connector (amely felel meg a meglévő Active Directory-erdő tooan):</span><span class="sxs-lookup"><span data-stu-id="08359-125">For each existing on-premises Active Directory connector (which corresponds tooan existing Active Directory forest):</span></span>

   * <span data-ttu-id="08359-126">Ellenőrzi, hogy hello jelszó-szinkronizálás szolgáltatás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="08359-126">Validates that hello password synchronization feature is enabled.</span></span>
   
   * <span data-ttu-id="08359-127">Jelszó szinkronizálási szívverés események hello keres Windows-alkalmazás eseményt naplózza.</span><span class="sxs-lookup"><span data-stu-id="08359-127">Searches for password synchronization heartbeat events in hello Windows Application Event logs.</span></span>

   * <span data-ttu-id="08359-128">Minden egyes hello a helyszíni Active Directory-összekötőt az Active Directory-tartomány:</span><span class="sxs-lookup"><span data-stu-id="08359-128">For each Active Directory domain under hello on-premises Active Directory connector:</span></span>

      * <span data-ttu-id="08359-129">Ellenőrzi, hogy hello tartománya hello Azure AD Connect-kiszolgáló elérhető.</span><span class="sxs-lookup"><span data-stu-id="08359-129">Validates that hello domain is reachable from hello Azure AD Connect server.</span></span>

      * <span data-ttu-id="08359-130">Ellenőrzi, hogy hello a helyszíni Active Directory-összekötő által használt hello Active Directory tartományi szolgáltatások (AD DS) fiókok hello megfelelő felhasználónév, jelszó és a jelszó-szinkronizáláshoz szükséges engedélyekkel rendelkezik-e.</span><span class="sxs-lookup"><span data-stu-id="08359-130">Validates that hello Active Directory Domain Services (AD DS) accounts used by hello on-premises Active Directory connector has hello correct username, password, and permissions required for password synchronization.</span></span>

<span data-ttu-id="08359-131">a következő diagram hello hello parancsmag egy egyetlen tartományból, a helyszíni Active Directory-topológia hello eredményét mutatja be:</span><span class="sxs-lookup"><span data-stu-id="08359-131">hello following diagram illustrates hello results of hello cmdlet for a single-domain, on-premises Active Directory topology:</span></span>

![Diagnosztikai kimenetet a jelszó-szinkronizáláshoz](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

<span data-ttu-id="08359-133">Ez a szakasz többi hello ismerteti adott hello parancsmag által visszaadott eredmények és a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="08359-133">hello rest of this section describes specific results that are returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="password-synchronization-feature-isnt-enabled"></a><span data-ttu-id="08359-134">Jelszó-szinkronizálási szolgáltatás nincs engedélyezve</span><span class="sxs-lookup"><span data-stu-id="08359-134">Password synchronization feature isn't enabled</span></span>
<span data-ttu-id="08359-135">Ha még nem engedélyezte a jelszó-szinkronizálás hello Azure AD Connect varázsló segítségével, a következő hiba hello ad vissza:</span><span class="sxs-lookup"><span data-stu-id="08359-135">If you haven't enabled password synchronization by using hello Azure AD Connect wizard, hello following error is returned:</span></span>

![A jelszó-szinkronizálás nincs engedélyezve](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a><span data-ttu-id="08359-137">Az Azure AD Connect-kiszolgáló átmeneti módban van</span><span class="sxs-lookup"><span data-stu-id="08359-137">Azure AD Connect server is in staging mode</span></span>
<span data-ttu-id="08359-138">Ha hello Azure AD Connect-kiszolgáló átmeneti módban, a jelszó-szinkronizálás ideiglenesen le van tiltva, és hello következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="08359-138">If hello Azure AD Connect server is in staging mode, password synchronization is temporarily disabled, and hello following error is returned:</span></span>

![Az Azure AD Connect-kiszolgáló átmeneti módban van](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a><span data-ttu-id="08359-140">Jelszó szinkronizálási szívverés események</span><span class="sxs-lookup"><span data-stu-id="08359-140">No password synchronization heartbeat events</span></span>
<span data-ttu-id="08359-141">Minden egyes a helyszíni Active Directory-összekötőt a saját jelszó-szinkronizálás csatorna rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="08359-141">Each on-premises Active Directory connector has its own password synchronization channel.</span></span> <span data-ttu-id="08359-142">Ha nincs minden szinkronizált jelszó módosítások toobe hello jelszó-szinkronizálás csatorna jön létre, a szívverés (eseményazonosító 654) jön létre 30 percenként az alkalmazások eseménynaplójában keresse meg a Windows hello.</span><span class="sxs-lookup"><span data-stu-id="08359-142">When hello password synchronization channel is established and there aren't any password changes toobe synchronized, a heartbeat event (EventId 654) is generated once every 30 minutes under hello Windows Application Event Log.</span></span> <span data-ttu-id="08359-143">Az egyes a helyszíni Active Directory-összekötő hello parancsmag rákeres a hello kapcsolódó szívverés eseményekre vonatkozóan elmúlt három óra.</span><span class="sxs-lookup"><span data-stu-id="08359-143">For each on-premises Active Directory connector, hello cmdlet searches for corresponding heartbeat events in hello past three hours.</span></span> <span data-ttu-id="08359-144">Ha nincs szívverés esemény található, hello következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="08359-144">If no heartbeat event is found, hello following error is returned:</span></span>

![Nincs jelszó-szinkronizálás szív járulhatnak esemény](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a><span data-ttu-id="08359-146">AD DS fiók nem rendelkezik megfelelő engedélyekkel</span><span class="sxs-lookup"><span data-stu-id="08359-146">AD DS account does not have correct permissions</span></span>
<span data-ttu-id="08359-147">Ha az Active Directory tartományi szolgáltatások hello hello által használt fiók a helyszíni Active Directory connector toosynchronize jelszókivonatait nem rendelkezik megfelelő engedélyekkel hello hello következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="08359-147">If hello AD DS account that's used by hello on-premises Active Directory connector toosynchronize password hashes does not have hello appropriate permissions, hello following error is returned:</span></span>

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a><span data-ttu-id="08359-149">Active Directory tartományi szolgáltatások fiók helytelen felhasználónév vagy jelszó</span><span class="sxs-lookup"><span data-stu-id="08359-149">Incorrect AD DS account username or password</span></span>
<span data-ttu-id="08359-150">Ha hello Active Directory tartományi Szolgáltatásokban a fiókot használják hello a helyszíni Active Directory connector toosynchronize jelszókivonatait egy helytelen felhasználónév vagy jelszó, a hello következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="08359-150">If hello AD DS account used by hello on-premises Active Directory connector toosynchronize password hashes has an incorrect username or password, hello following error is returned:</span></span>

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a><span data-ttu-id="08359-152">Több objektum van nem jelszó-szinkronizálás: hello diagnosztikai parancsmaggal hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="08359-152">One object is not synchronizing passwords: troubleshoot by using hello diagnostic cmdlet</span></span>
<span data-ttu-id="08359-153">Használhatja a hello `Invoke-ADSyncDiagnostics` parancsmag toodetermine miért egy objektum nem szinkronizál jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="08359-153">You can use hello `Invoke-ADSyncDiagnostics` cmdlet toodetermine why one object is not synchronizing passwords.</span></span>

> [!NOTE]
> <span data-ttu-id="08359-154">Hello `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="08359-154">hello `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-hello-diagnostics-cmdlet"></a><span data-ttu-id="08359-155">Hello diagnosztika parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="08359-155">Run hello diagnostics cmdlet</span></span>
<span data-ttu-id="08359-156">ahol nincs jelszavai szinkronizálva vannak az tootroubleshoot problémák:</span><span class="sxs-lookup"><span data-stu-id="08359-156">tootroubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="08359-157">Nyisson meg egy új Windows PowerShell-munkamenetet az Azure AD Connect kiszolgálón hello **Futtatás rendszergazdaként** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="08359-157">Open a new Windows PowerShell session on your Azure AD Connect server with hello **Run as Administrator** option.</span></span>

2. <span data-ttu-id="08359-158">Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="08359-158">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="08359-159">Futtassa az `Import-Module ADSyncDiagnostics` parancsot.</span><span class="sxs-lookup"><span data-stu-id="08359-159">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="08359-160">Futtassa a következő parancsmag hello:</span><span class="sxs-lookup"><span data-stu-id="08359-160">Run hello following cmdlet:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   <span data-ttu-id="08359-161">Példa:</span><span class="sxs-lookup"><span data-stu-id="08359-161">For example:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a><span data-ttu-id="08359-162">Hello eredmények hello parancsmag ismertetése</span><span class="sxs-lookup"><span data-stu-id="08359-162">Understand hello results of hello cmdlet</span></span>
<span data-ttu-id="08359-163">hello diagnosztikai parancsmag a következő ellenőrzések hello hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="08359-163">hello diagnostic cmdlet performs hello following checks:</span></span>

* <span data-ttu-id="08359-164">Megvizsgálja a hello Active Directory kapcsolódási térbe, Metaverse és az Azure Active Directory-objektum hello hello állapotának AD kapcsolódási térbe.</span><span class="sxs-lookup"><span data-stu-id="08359-164">Examines hello state of hello Active Directory object in hello Active Directory connector space, Metaverse, and Azure AD connector space.</span></span>

* <span data-ttu-id="08359-165">Ellenőrzi, hogy vannak-e a jelszó-szinkronizálás szinkronizálási szabályait engedélyezve van, és toohello Active Directory-objektumot alkalmazza.</span><span class="sxs-lookup"><span data-stu-id="08359-165">Validates that there are synchronization rules with password synchronization enabled and applied toohello Active Directory object.</span></span>

* <span data-ttu-id="08359-166">Próbálja meg hello utolsó kísérlet toosynchronize hello jelszó hello objektum tooretrieve és megjelenítési hello eredményeit.</span><span class="sxs-lookup"><span data-stu-id="08359-166">Attempts tooretrieve and display hello results of hello last attempt toosynchronize hello password for hello object.</span></span>

<span data-ttu-id="08359-167">a következő diagram hello hello parancsmag hello eredményét mutatja be, egyetlen objektumhoz jelszó-szinkronizálás hibaelhárítása során:</span><span class="sxs-lookup"><span data-stu-id="08359-167">hello following diagram illustrates hello results of hello cmdlet when troubleshooting password synchronization for a single object:</span></span>

![A jelszó-szinkronizáláshoz - egyetlen objektumhoz diagnosztikai kimenetet](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

<span data-ttu-id="08359-169">Ez a szakasz többi hello ismerteti adott hello parancsmag által visszaadott eredmények és a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="08359-169">hello rest of this section describes specific results returned by hello cmdlet and corresponding issues.</span></span>

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a><span data-ttu-id="08359-170">hello Active Directory-objektum nem AD exportált tooAzure</span><span class="sxs-lookup"><span data-stu-id="08359-170">hello Active Directory object isn't exported tooAzure AD</span></span>
<span data-ttu-id="08359-171">A jelszó-szinkronizálás a helyszíni Active Directory-fiók sikertelen lesz, mivel nincs hello Azure AD-bérlő a megfelelő objektum.</span><span class="sxs-lookup"><span data-stu-id="08359-171">Password synchronization for this on-premises Active Directory account fails because there is no corresponding object in hello Azure AD tenant.</span></span> <span data-ttu-id="08359-172">a visszaadott hiba a következő hello:</span><span class="sxs-lookup"><span data-stu-id="08359-172">hello following error is returned:</span></span>

![Az Azure AD-objektum hiányzik.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a><span data-ttu-id="08359-174">Felhasználó rendelkezik-e egy ideiglenes jelszót</span><span class="sxs-lookup"><span data-stu-id="08359-174">User has a temporary password</span></span>
<span data-ttu-id="08359-175">Jelenleg az Azure AD Connect nem támogatja ideiglenes jelszó-szinkronizálás Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="08359-175">Currently, Azure AD Connect does not support synchronizing temporary passwords with Azure AD.</span></span> <span data-ttu-id="08359-176">A jelszó akkor tekinthető toobe ideiglenes Ha hello **jelszó módosítása a következő bejelentkezéskor** hello a helyszíni Active Directory-felhasználó a beállítás.</span><span class="sxs-lookup"><span data-stu-id="08359-176">A password is considered toobe temporary if hello **Change password at next logon** option is set on hello on-premises Active Directory user.</span></span> <span data-ttu-id="08359-177">a visszaadott hiba a következő hello:</span><span class="sxs-lookup"><span data-stu-id="08359-177">hello following error is returned:</span></span>

![Ideiglenes jelszót ne exportálja.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a><span data-ttu-id="08359-179">Utolsó kísérlet toosynchronize jelszó eredményei nem érhetők el</span><span class="sxs-lookup"><span data-stu-id="08359-179">Results of last attempt toosynchronize password aren't available</span></span>
<span data-ttu-id="08359-180">Alapértelmezés szerint az Azure AD Connect szinkronizálási bejelentkezési kísérletek hét napja hello eredményeit tárolja.</span><span class="sxs-lookup"><span data-stu-id="08359-180">By default, Azure AD Connect stores hello results of password synchronization attempts for seven days.</span></span> <span data-ttu-id="08359-181">Ha nincsenek kiválasztott hello Active Directory-objektum nem járt eredménnyel, figyelmeztetés a következő hello adja vissza:</span><span class="sxs-lookup"><span data-stu-id="08359-181">If there are no results available for hello selected Active Directory object, hello following warning is returned:</span></span>

![Egyetlen objektum – a Nincs szinkronizálás az előző diagnosztikai kimenete](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a><span data-ttu-id="08359-183">Nincs jelszavai szinkronizálva vannak: manuális hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="08359-183">No passwords are synchronized: manual troubleshooting steps</span></span>
<span data-ttu-id="08359-184">Kövesse a lépéseket toodetermine, miért nem jelszavai szinkronizálva vannak az:</span><span class="sxs-lookup"><span data-stu-id="08359-184">Follow these steps toodetermine why no passwords are synchronized:</span></span>

1. <span data-ttu-id="08359-185">A Connect-kiszolgálója hello [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode)?</span><span class="sxs-lookup"><span data-stu-id="08359-185">Is hello Connect server in [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)?</span></span> <span data-ttu-id="08359-186">Átmeneti módban nem szinkronizálja a jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="08359-186">A server in staging mode does not synchronize any passwords.</span></span>

2. <span data-ttu-id="08359-187">Hello hello parancsprogrammal [hello jelszó-szinkronizálási beállítások állapotának lekérése](#get-the-status-of-password-sync-settings) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-187">Run hello script in hello [Get hello status of password sync settings](#get-the-status-of-password-sync-settings) section.</span></span> <span data-ttu-id="08359-188">Ez lehetővé teszi az hello jelszó-szinkronizálás konfigurációs áttekintését.</span><span class="sxs-lookup"><span data-stu-id="08359-188">It gives you an overview of hello password sync configuration.</span></span>  

    ![PowerShell parancsfájl kimenete, jelszó-szinkronizálási beállítások](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. <span data-ttu-id="08359-190">Ha hello szolgáltatás nincs engedélyezve az Azure ad-ben, vagy ha hello szinkronizálási csatorna állapota nincs engedélyezve, futtassa a hello Connect telepítővarázslóját.</span><span class="sxs-lookup"><span data-stu-id="08359-190">If hello feature is not enabled in Azure AD or if hello sync channel status is not enabled, run hello Connect installation wizard.</span></span> <span data-ttu-id="08359-191">Válassza ki **testre szabhatja a szinkronizálási beállítások**, és törölje a jelszó-szinkronizálást. Ez a változás ideiglenesen letiltja hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="08359-191">Select **Customize synchronization options**, and unselect password sync. This change temporarily disables hello feature.</span></span> <span data-ttu-id="08359-192">Majd futtassa újból hello varázslót, és engedélyezze újra a jelszó-szinkronizálást. Hello parancsprogrammal újra, hogy a konfigurációs hello tooverify helyességéről.</span><span class="sxs-lookup"><span data-stu-id="08359-192">Then run hello wizard again and re-enable password sync. Run hello script again tooverify that hello configuration is correct.</span></span>

4. <span data-ttu-id="08359-193">Keressen hello eseménynaplóban a hibákat.</span><span class="sxs-lookup"><span data-stu-id="08359-193">Look in hello event log for errors.</span></span> <span data-ttu-id="08359-194">Keresse meg a következő események, amelyek azt jelentené, hogy egy probléma hello:</span><span class="sxs-lookup"><span data-stu-id="08359-194">Look for hello following events, which would indicate a problem:</span></span>
    * <span data-ttu-id="08359-195">Forrás: "A címtár-szinkronizálás" azonosító: 0, 611, 652, ha ezek az események 655 kapcsolat hibát.</span><span class="sxs-lookup"><span data-stu-id="08359-195">Source: "Directory synchronization" ID: 0, 611, 652, 655 If you see these events, you have a connectivity problem.</span></span> <span data-ttu-id="08359-196">Eseménynapló-üzenet hello probléma esetében erdő információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="08359-196">hello event log message contains forest information where you have a problem.</span></span> <span data-ttu-id="08359-197">További információkért lásd: [csatlakozási probléma](#connectivity problem).</span><span class="sxs-lookup"><span data-stu-id="08359-197">For more information, see [Connectivity problem](#connectivity problem).</span></span>

5. <span data-ttu-id="08359-198">Ha nem szívverés, vagy ha nincs más, futtassa [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="08359-198">If you see no heartbeat or if nothing else worked, run [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span> <span data-ttu-id="08359-199">Hello parancsfájl csak egyszer futtatnia.</span><span class="sxs-lookup"><span data-stu-id="08359-199">Run hello script only once.</span></span>

6. <span data-ttu-id="08359-200">Lásd: hello [egy olyan objektum, amely nem szinkronizál jelszavak hibaelhárítása](#one-object-is-not-synchronizing-passwords) szakasz.</span><span class="sxs-lookup"><span data-stu-id="08359-200">See hello [Troubleshoot one object that is not synchronizing passwords](#one-object-is-not-synchronizing-passwords) section.</span></span>

### <a name="connectivity-problems"></a><span data-ttu-id="08359-201">Kapcsolódási problémák</span><span class="sxs-lookup"><span data-stu-id="08359-201">Connectivity problems</span></span>

<span data-ttu-id="08359-202">Az Azure AD-kapcsolat van?</span><span class="sxs-lookup"><span data-stu-id="08359-202">Do you have connectivity with Azure AD?</span></span>

<span data-ttu-id="08359-203">Hello fiók rendelkezik szükséges engedélyek tooread hello jelszókivonatait minden tartományban?</span><span class="sxs-lookup"><span data-stu-id="08359-203">Does hello account have required permissions tooread hello password hashes in all domains?</span></span> <span data-ttu-id="08359-204">Ha a csatlakozás a gyorsbeállítások használatával telepítette, hello engedélyek már kell megfelelő.</span><span class="sxs-lookup"><span data-stu-id="08359-204">If you installed Connect by using Express settings, hello permissions should already be correct.</span></span> 

<span data-ttu-id="08359-205">Ha egyéni telepítési, hello engedélyek beállítása manuálisan hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="08359-205">If you used custom installation, set hello permissions manually by doing hello following:</span></span>
    
1. <span data-ttu-id="08359-206">hello Active Directory-összekötő kezdő által használt toofind hello fiók **Synchronization Service Managert**.</span><span class="sxs-lookup"><span data-stu-id="08359-206">toofind hello account used by hello Active Directory connector, start **Synchronization Service Manager**.</span></span> 
 
2. <span data-ttu-id="08359-207">Nyissa meg túl**összekötők**, majd keresse meg a hello a helyszíni Active Directory-erdő hibaelhárítást.</span><span class="sxs-lookup"><span data-stu-id="08359-207">Go too**Connectors**, and then search for hello on-premises Active Directory forest you are troubleshooting.</span></span> 
 
3. <span data-ttu-id="08359-208">Válassza ki a hello összekötő, és kattintson a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="08359-208">Select hello connector, and then click **Properties**.</span></span> 
 
4. <span data-ttu-id="08359-209">Nyissa meg túl**tooActive Directory-erdő csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="08359-209">Go too**Connect tooActive Directory Forest**.</span></span>  
    
    ![Active Directory-összekötő által használt fiók](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    <span data-ttu-id="08359-211">Megjegyzés: hello felhasználónév és a hello tartomány hello fiók.</span><span class="sxs-lookup"><span data-stu-id="08359-211">Note hello username and hello domain where hello account is located.</span></span>
    
5. <span data-ttu-id="08359-212">Start **Active Directory – felhasználók és számítógépek**, és ellenőrizze, hogy korábban található hello fiók jogosult hello kövesse állítsa be megfelelően az erdőben lévő összes tartományban hello gyökérmappájában:</span><span class="sxs-lookup"><span data-stu-id="08359-212">Start **Active Directory Users and Computers**, and then verify that hello account you found earlier has hello follow permissions set at hello root of all domains in your forest:</span></span>
    * <span data-ttu-id="08359-213">Változások replikálása</span><span class="sxs-lookup"><span data-stu-id="08359-213">Replicate Directory Changes</span></span>
    * <span data-ttu-id="08359-214">Replikálása Directory összes módosítása</span><span class="sxs-lookup"><span data-stu-id="08359-214">Replicate Directory Changes All</span></span>

6. <span data-ttu-id="08359-215">Hello Azure AD Connect elérhető-e tartományvezérlők vannak?</span><span class="sxs-lookup"><span data-stu-id="08359-215">Are hello domain controllers reachable by Azure AD Connect?</span></span> <span data-ttu-id="08359-216">Ha hello Connect-kiszolgáló nem tud csatlakozni a tooall tartományvezérlők, konfigurálja **csak az elsődleges tartományvezérlőt használja**.</span><span class="sxs-lookup"><span data-stu-id="08359-216">If hello Connect server cannot connect tooall domain controllers, configure **Only use preferred domain controller**.</span></span>  
    
    ![Az Active Directory-összekötő által használt tartományvezérlő](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. <span data-ttu-id="08359-218">Lépjen vissza túl**Synchronization Service Managert** és **címtárpartíciók konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="08359-218">Go back too**Synchronization Service Manager** and **Configure Directory Partition**.</span></span> 
 
8. <span data-ttu-id="08359-219">Válassza ki a tartományt a **válassza ki az a címtárpartíciókat**, jelölje be hello **csak használja az elsődleges tartományvezérlő** jelölőnégyzetet, majd kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="08359-219">Select your domain in **Select directory partitions**, select hello **Only use preferred domain controllers** check box, and then click **Configure**.</span></span> 

9. <span data-ttu-id="08359-220">Hello listában adja meg a jelszó-szinkronizálás használata ajánlott a Connect hello tartományvezérlők. hello ugyanazt a listát történő importálásának és exportálásának is szolgál.</span><span class="sxs-lookup"><span data-stu-id="08359-220">In hello list, enter hello domain controllers that Connect should use for password sync. hello same list is used for import and export as well.</span></span> <span data-ttu-id="08359-221">Hajtsa végre ezeket a lépéseket minden tartományban.</span><span class="sxs-lookup"><span data-stu-id="08359-221">Do these steps for all your domains.</span></span>

10. <span data-ttu-id="08359-222">Ha hello parancsfájl bemutatja, hogy van-e nem szívverés, futtassa az hello parancsfájl [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="08359-222">If hello script shows that there is no heartbeat, run hello script in [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span>

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a><span data-ttu-id="08359-223">Több objektum van nem jelszó-szinkronizálás: manuális hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="08359-223">One object is not synchronizing passwords: manual troubleshooting steps</span></span>
<span data-ttu-id="08359-224">Jelszó-szinkronizálás problémák könnyen elháríthatja az hello egy objektumot állapotának megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="08359-224">You can easily troubleshoot password synchronization issues by reviewing hello status of an object.</span></span>

1. <span data-ttu-id="08359-225">A **Active Directory – felhasználók és számítógépek**, hello felhasználói keressen, és ellenőrizze, hogy hello **kell változtatni a jelszót a következő bejelentkezéskor** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="08359-225">In **Active Directory Users and Computers**, search for hello user, and then verify that hello **User must change password at next logon** check box is cleared.</span></span>  

    ![Active Directory hatékony jelszavak](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    <span data-ttu-id="08359-227">Ha hello jelölőnégyzet be van jelölve, kérje meg a hello felhasználói toosign és hello jelszó módosítása.</span><span class="sxs-lookup"><span data-stu-id="08359-227">If hello check box is selected, ask hello user toosign in and change hello password.</span></span> <span data-ttu-id="08359-228">Ideiglenes jelszavak nincsenek szinkronizálva az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="08359-228">Temporary passwords are not synchronized with Azure AD.</span></span>

2. <span data-ttu-id="08359-229">Ha hello jelszó helyes-e az Active Directoryban, hajtsa végre a szinkronizálási motor hello hello felhasználói.</span><span class="sxs-lookup"><span data-stu-id="08359-229">If hello password looks correct in Active Directory, follow hello user in hello sync engine.</span></span> <span data-ttu-id="08359-230">A helyszíni Active Directory tooAzure AD a következő hello felhasználó láthatja, hogy van-e a leíró hiba hello objektumon.</span><span class="sxs-lookup"><span data-stu-id="08359-230">By following hello user from on-premises Active Directory tooAzure AD, you can see whether there is a descriptive error on hello object.</span></span>

    <span data-ttu-id="08359-231">a.</span><span class="sxs-lookup"><span data-stu-id="08359-231">a.</span></span> <span data-ttu-id="08359-232">Indítsa el a hello [Synchronization Service Managert](active-directory-aadconnectsync-service-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="08359-232">Start hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span></span>

    <span data-ttu-id="08359-233">b.</span><span class="sxs-lookup"><span data-stu-id="08359-233">b.</span></span> <span data-ttu-id="08359-234">Kattintson a **összekötők**.</span><span class="sxs-lookup"><span data-stu-id="08359-234">Click **Connectors**.</span></span>

    <span data-ttu-id="08359-235">c.</span><span class="sxs-lookup"><span data-stu-id="08359-235">c.</span></span> <span data-ttu-id="08359-236">Jelölje be hello **Active Directory-összekötő** ahol hello felhasználó tartozik.</span><span class="sxs-lookup"><span data-stu-id="08359-236">Select hello **Active Directory Connector** where hello user is located.</span></span>

    <span data-ttu-id="08359-237">d.</span><span class="sxs-lookup"><span data-stu-id="08359-237">d.</span></span> <span data-ttu-id="08359-238">Válassza ki **Összekötőtér keresési**.</span><span class="sxs-lookup"><span data-stu-id="08359-238">Select **Search Connector Space**.</span></span>

    <span data-ttu-id="08359-239">e.</span><span class="sxs-lookup"><span data-stu-id="08359-239">e.</span></span> <span data-ttu-id="08359-240">A hello **hatókör** mezőben válassza **megkülönböztető név vagy a horgony**, majd adja meg a teljes DN hello hibaelhárítási hello felhasználó.</span><span class="sxs-lookup"><span data-stu-id="08359-240">In hello **Scope** box, select **DN or Anchor**, and then enter hello full DN of hello user you are troubleshooting.</span></span>

    ![A kapcsolódási térbe megkülönböztető névvel rendelkező felhasználó keresése](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    <span data-ttu-id="08359-242">f.</span><span class="sxs-lookup"><span data-stu-id="08359-242">f.</span></span> <span data-ttu-id="08359-243">Keresse meg a hello felhasználói keres, és kattintson a **tulajdonságok** toosee összes hello attribútumok.</span><span class="sxs-lookup"><span data-stu-id="08359-243">Locate hello user you are looking for, and then click **Properties** toosee all hello attributes.</span></span> <span data-ttu-id="08359-244">Ha hello felhasználó nincs hello keresési eredményt, ellenőrizze a [szűrési szabályok](active-directory-aadconnectsync-configure-filtering.md) és győződjön meg arról, hogy [alkalmaz és a módosítások ellenőrzéséhez](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) hello felhasználói tooappear a csatlakozás a.</span><span class="sxs-lookup"><span data-stu-id="08359-244">If hello user is not in hello search result, verify your [filtering rules](active-directory-aadconnectsync-configure-filtering.md) and make sure that you run [Apply and verify changes](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) for hello user tooappear in Connect.</span></span>

    <span data-ttu-id="08359-245">g.</span><span class="sxs-lookup"><span data-stu-id="08359-245">g.</span></span> <span data-ttu-id="08359-246">toosee hello jelszó szinkronizálási részletek hello hello objektumának múlt héten kattintson **napló**.</span><span class="sxs-lookup"><span data-stu-id="08359-246">toosee hello password sync details of hello object for hello past week, click **Log**.</span></span>  

    ![Napló részletes adatai](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    <span data-ttu-id="08359-248">Ha hello objektum napló üres, az Azure AD Connect lett nem tooread hello Jelszókivonat az Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="08359-248">If hello object log is empty, Azure AD Connect has been unable tooread hello password hash from Active Directory.</span></span> <span data-ttu-id="08359-249">Folytassa a [kapcsolódási hibák](#connectivity-errors).</span><span class="sxs-lookup"><span data-stu-id="08359-249">Continue your troubleshooting with [Connectivity Errors](#connectivity-errors).</span></span> <span data-ttu-id="08359-250">Ha látja, mint bármely más érték **sikeres**, tekintse meg a táblázat toohello [jelszó-szinkronizálás napló](#password-sync-log).</span><span class="sxs-lookup"><span data-stu-id="08359-250">If you see any other value than **success**, refer toohello table in [Password sync log](#password-sync-log).</span></span>

    <span data-ttu-id="08359-251">h.</span><span class="sxs-lookup"><span data-stu-id="08359-251">h.</span></span> <span data-ttu-id="08359-252">Jelölje be hello **Leszármaztatás** lapot, és győződjön meg arról, hogy legalább egy szinkronizálási szabály a hello **PasswordSync** oszlop **igaz**.</span><span class="sxs-lookup"><span data-stu-id="08359-252">Select hello **lineage** tab, and make sure that at least one sync rule in hello **PasswordSync** column is **True**.</span></span> <span data-ttu-id="08359-253">Hello alapértelmezés szerint hello neve hello szinkronizálási szabály: **a az AD - felhasználó AccountEnabled**.</span><span class="sxs-lookup"><span data-stu-id="08359-253">In hello default configuration, hello name of hello sync rule is **In from AD - User AccountEnabled**.</span></span>  

    ![Leszármaztatás felhasználókkal kapcsolatos információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    <span data-ttu-id="08359-255">i.</span><span class="sxs-lookup"><span data-stu-id="08359-255">i.</span></span> <span data-ttu-id="08359-256">Kattintson a **Metaverzum-objektum tulajdonságai** toodisplay felhasználói attribútumok listáját.</span><span class="sxs-lookup"><span data-stu-id="08359-256">Click **Metaverse Object Properties** toodisplay a list of user attributes.</span></span>  

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    <span data-ttu-id="08359-258">Győződjön meg arról, hogy nincs **cloudFiltered** attribútumnak jelen.</span><span class="sxs-lookup"><span data-stu-id="08359-258">Verify that there is no **cloudFiltered** attribute present.</span></span> <span data-ttu-id="08359-259">Győződjön meg arról, hogy hello tartomány tulajdonságai (domainFQDN és domainNetBios) hello várt értékek.</span><span class="sxs-lookup"><span data-stu-id="08359-259">Make sure that hello domain attributes (domainFQDN and domainNetBios) have hello expected values.</span></span>

    <span data-ttu-id="08359-260">j.</span><span class="sxs-lookup"><span data-stu-id="08359-260">j.</span></span> <span data-ttu-id="08359-261">Kattintson a hello **összekötők** fülre. Győződjön meg arról, hogy megjelenik-e összekötők tooboth a helyszíni Active Directory és az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08359-261">Click hello **Connectors** tab. Make sure that you see connectors tooboth on-premises Active Directory and Azure AD.</span></span>

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    <span data-ttu-id="08359-263">k.</span><span class="sxs-lookup"><span data-stu-id="08359-263">k.</span></span> <span data-ttu-id="08359-264">Az Azure AD jelképező válassza hello sorban kattintson **tulajdonságok**, majd kattintson a hello **Leszármaztatás** külön-külön hello összekötő terület objektum meg kell adni a hello egy kimenő forgalomra vonatkozó szabály **PasswordSync** oszlopkészlet túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="08359-264">Select hello row that represents Azure AD, click **Properties**, and then click hello **Lineage** tab. hello connector space object should have an outbound rule in hello **PasswordSync** column set too**True**.</span></span> <span data-ttu-id="08359-265">Hello alapértelmezés szerint hello neve hello szinkronizálási szabály: **tooAAD - felhasználók csatlakozásra kimenő**.</span><span class="sxs-lookup"><span data-stu-id="08359-265">In hello default configuration, hello name of hello sync rule is **Out tooAAD - User Join**.</span></span>  

    ![Összekötő Összekötőtér-objektum tulajdonságai párbeszédpanel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a><span data-ttu-id="08359-267">Jelszó-szinkronizálás napló</span><span class="sxs-lookup"><span data-stu-id="08359-267">Password sync log</span></span>
<span data-ttu-id="08359-268">hello állapot oszlop hello a következő értékeket veheti fel:</span><span class="sxs-lookup"><span data-stu-id="08359-268">hello status column can have hello following values:</span></span>

| <span data-ttu-id="08359-269">status</span><span class="sxs-lookup"><span data-stu-id="08359-269">Status</span></span> | <span data-ttu-id="08359-270">Leírás</span><span class="sxs-lookup"><span data-stu-id="08359-270">Description</span></span> |
| --- | --- |
| <span data-ttu-id="08359-271">Sikeres</span><span class="sxs-lookup"><span data-stu-id="08359-271">Success</span></span> |<span data-ttu-id="08359-272">Jelszó szinkronizálása sikerült.</span><span class="sxs-lookup"><span data-stu-id="08359-272">Password has been successfully synchronized.</span></span> |
| <span data-ttu-id="08359-273">FilteredByTarget</span><span class="sxs-lookup"><span data-stu-id="08359-273">FilteredByTarget</span></span> |<span data-ttu-id="08359-274">Jelszó beállítása túl**kell változtatni a jelszót a következő bejelentkezéskor**.</span><span class="sxs-lookup"><span data-stu-id="08359-274">Password is set too**User must change password at next logon**.</span></span> <span data-ttu-id="08359-275">Jelszó nem lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="08359-275">Password has not been synchronized.</span></span> |
| <span data-ttu-id="08359-276">NoTargetConnection</span><span class="sxs-lookup"><span data-stu-id="08359-276">NoTargetConnection</span></span> |<span data-ttu-id="08359-277">Nincs objektum hello metaverse vagy a kapcsolódási térbe hello Azure AD.</span><span class="sxs-lookup"><span data-stu-id="08359-277">No object in hello metaverse or in hello Azure AD connector space.</span></span> |
| <span data-ttu-id="08359-278">SourceConnectorNotPresent</span><span class="sxs-lookup"><span data-stu-id="08359-278">SourceConnectorNotPresent</span></span> |<span data-ttu-id="08359-279">Nem található a hello a helyszíni Active Directory kapcsolódási térbe objektum.</span><span class="sxs-lookup"><span data-stu-id="08359-279">No object found in hello on-premises Active Directory connector space.</span></span> |
| <span data-ttu-id="08359-280">TargetNotExportedToDirectory</span><span class="sxs-lookup"><span data-stu-id="08359-280">TargetNotExportedToDirectory</span></span> |<span data-ttu-id="08359-281">hello Azure AD kapcsolódási térbe hello objektum még nem lett exportálva.</span><span class="sxs-lookup"><span data-stu-id="08359-281">hello object in hello Azure AD connector space has not yet been exported.</span></span> |
| <span data-ttu-id="08359-282">MigratedCheckDetailsForMoreInfo</span><span class="sxs-lookup"><span data-stu-id="08359-282">MigratedCheckDetailsForMoreInfo</span></span> |<span data-ttu-id="08359-283">Naplóbejegyzés build 1.0.9125.0 előtt készült, és megjelenik-e a korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="08359-283">Log entry was created before build 1.0.9125.0 and is shown in its legacy state.</span></span> |
| <span data-ttu-id="08359-284">Hiba</span><span class="sxs-lookup"><span data-stu-id="08359-284">Error</span></span> |<span data-ttu-id="08359-285">Szolgáltatás ismeretlen hibával tért vissza.</span><span class="sxs-lookup"><span data-stu-id="08359-285">Service returned an unknown error.</span></span> |
| <span data-ttu-id="08359-286">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="08359-286">Unknown</span></span> |<span data-ttu-id="08359-287">Hiba történt a jelszó-kivonatok köteg tooprocess tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="08359-287">An error occurred while trying tooprocess a batch of password hashes.</span></span>  |
| <span data-ttu-id="08359-288">MissingAttribute</span><span class="sxs-lookup"><span data-stu-id="08359-288">MissingAttribute</span></span> |<span data-ttu-id="08359-289">Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="08359-289">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services are not available.</span></span> |
| <span data-ttu-id="08359-290">RetryRequestedByTarget</span><span class="sxs-lookup"><span data-stu-id="08359-290">RetryRequestedByTarget</span></span> |<span data-ttu-id="08359-291">Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt korábban nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="08359-291">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services were not available previously.</span></span> <span data-ttu-id="08359-292">Egy kísérlet tooresynchronize hello felhasználói Jelszókivonat történik.</span><span class="sxs-lookup"><span data-stu-id="08359-292">An attempt tooresynchronize hello user's password hash is made.</span></span> |

## <a name="scripts-toohelp-troubleshooting"></a><span data-ttu-id="08359-293">Parancsfájlok toohelp hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="08359-293">Scripts toohelp troubleshooting</span></span>

### <a name="get-hello-status-of-password-sync-settings"></a><span data-ttu-id="08359-294">A jelszó-szinkronizálási beállítások hello állapotának beolvasása</span><span class="sxs-lookup"><span data-stu-id="08359-294">Get hello status of password sync settings</span></span>
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a><span data-ttu-id="08359-295">A teljes szinkronizálás az összes jelszavak eseményindító</span><span class="sxs-lookup"><span data-stu-id="08359-295">Trigger a full sync of all passwords</span></span>
> [!NOTE]
> <span data-ttu-id="08359-296">Ez a parancsfájl csak egyszer futtatnia.</span><span class="sxs-lookup"><span data-stu-id="08359-296">Run this script only once.</span></span> <span data-ttu-id="08359-297">Toorun azt egynél többször van szükség, ha valami mással hello probléma.</span><span class="sxs-lookup"><span data-stu-id="08359-297">If you need toorun it more than once, something else is hello problem.</span></span> <span data-ttu-id="08359-298">tootroubleshoot hello probléma, forduljon a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="08359-298">tootroubleshoot hello problem, contact Microsoft support.</span></span>

<span data-ttu-id="08359-299">A következő parancsfájl hello segítségével a teljes szinkronizálás az összes jelszavak indíthat el:</span><span class="sxs-lookup"><span data-stu-id="08359-299">You can trigger a full sync of all passwords by using hello following script:</span></span>

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a><span data-ttu-id="08359-300">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="08359-300">Next steps</span></span>
* [<span data-ttu-id="08359-301">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="08359-301">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md)
* [<span data-ttu-id="08359-302">Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="08359-302">Azure AD Connect Sync: Customizing synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="08359-303">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="08359-303">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
