---
title: "A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás hibaelhárítása |} Microsoft Docs"
description: "Ez a cikk ismerteti a jelszó-szinkronizálás problémák elhárításáról."
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
ms.openlocfilehash: 33fa6a8867764975a57b8727e7705529d1d7506a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a><span data-ttu-id="6ebc7-103">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-103">Troubleshoot password synchronization with Azure AD Connect sync</span></span>
<span data-ttu-id="6ebc7-104">Ez a témakör a jelszó-szinkronizálás a probléma megoldásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-104">This topic provides steps for how to troubleshoot issues with password synchronization.</span></span> <span data-ttu-id="6ebc7-105">Jelszavak nem szinkronizál a várt módon, ha azok a felhasználók egy része vagy az összes felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-105">If passwords are not synchronizing as expected, it can be either for a subset of users or for all users.</span></span> <span data-ttu-id="6ebc7-106">Az Azure Active Directory (Azure AD) telepítési csatlakozás verziója 1.1.524.0, vagy később, már létezik egy diagnosztikai parancsmag, amely a jelszó-szinkronizálás probléma megoldásához használhatja:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-106">For Azure Active Directory (Azure AD) Connect deployment with version 1.1.524.0 or later, there is now a diagnostic cmdlet that you can use to troubleshoot password synchronization issues:</span></span>

* <span data-ttu-id="6ebc7-107">Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a [nem jelszavai szinkronizálva vannak: a diagnosztikai parancsmaggal hibaelhárításához](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-107">If you have an issue where no passwords are synchronized, refer to the [No passwords are synchronized: troubleshoot by using the diagnostic cmdlet](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

* <span data-ttu-id="6ebc7-108">Ha az egyes objektumok problémát, tekintse meg a [több objektum van nem jelszó-szinkronizálás: a diagnosztikai parancsmaggal hibaelhárításához](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-108">If you have an issue with individual objects, refer to the [One object is not synchronizing passwords: troubleshoot by using the diagnostic cmdlet](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) section.</span></span>

<span data-ttu-id="6ebc7-109">Az Azure AD Connect telepítési korábbi verzióival:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-109">For older versions of Azure AD Connect deployment:</span></span>

* <span data-ttu-id="6ebc7-110">Ha a probléma lehet ahol nincs jelszavak szinkronizálódnak, tekintse meg a a [nem jelszavai szinkronizálva vannak: hibaelhárítási manuális](#no-passwords-are-synchronized-manual-troubleshooting-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-110">If you have an issue where no passwords are synchronized, refer to the [No passwords are synchronized: manual troubleshooting steps](#no-passwords-are-synchronized-manual-troubleshooting-steps) section.</span></span>

* <span data-ttu-id="6ebc7-111">Ha egyéni objektumokat problémát, tekintse meg a [több objektum van nem jelszó-szinkronizálás: hibaelhárítási manuális](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-111">If you have an issue with individual objects, refer to the [One object is not synchronizing passwords: manual troubleshooting steps](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) section.</span></span>

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet"></a><span data-ttu-id="6ebc7-112">Nincs jelszavai szinkronizálva vannak: a diagnosztikai parancsmaggal hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="6ebc7-112">No passwords are synchronized: troubleshoot by using the diagnostic cmdlet</span></span>
<span data-ttu-id="6ebc7-113">Használhatja a `Invoke-ADSyncDiagnostics` parancsmag, hogy megtudja, miért nem jelszavak szinkronizálódnak.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-113">You can use the `Invoke-ADSyncDiagnostics` cmdlet to figure out why no passwords are synchronized.</span></span>

> [!NOTE]
> <span data-ttu-id="6ebc7-114">A `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-114">The `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-the-diagnostics-cmdlet"></a><span data-ttu-id="6ebc7-115">A diagnosztika parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-115">Run the diagnostics cmdlet</span></span>
<span data-ttu-id="6ebc7-116">Ahol nincs jelszavai szinkronizálva vannak-e problémák hibaelhárításához:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-116">To troubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="6ebc7-117">Nyisson meg egy új Windows PowerShell-munkamenetet a Azure AD Connect-kiszolgáló és az a **Futtatás rendszergazdaként** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-117">Open a new Windows PowerShell session on your Azure AD Connect server with the **Run as Administrator** option.</span></span>

2. <span data-ttu-id="6ebc7-118">Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-118">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="6ebc7-119">Futtassa az `Import-Module ADSyncDiagnostics` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-119">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="6ebc7-120">Futtassa az `Invoke-ADSyncDiagnostics -PasswordSync` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-120">Run `Invoke-ADSyncDiagnostics -PasswordSync`.</span></span>

### <a name="understand-the-results-of-the-cmdlet"></a><span data-ttu-id="6ebc7-121">A parancsmag eredményei ismertetése</span><span class="sxs-lookup"><span data-stu-id="6ebc7-121">Understand the results of the cmdlet</span></span>
<span data-ttu-id="6ebc7-122">A diagnosztikai parancsmag a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-122">The diagnostic cmdlet performs the following checks:</span></span>

* <span data-ttu-id="6ebc7-123">Ellenőrzi, hogy a jelszó-szinkronizálási szolgáltatás engedélyezve van az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-123">Validates that the password synchronization feature is enabled for your Azure AD tenant.</span></span>

* <span data-ttu-id="6ebc7-124">Ellenőrzi, hogy az Azure AD Connect-kiszolgáló nem átmeneti módban.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-124">Validates that the Azure AD Connect server is not in staging mode.</span></span>

* <span data-ttu-id="6ebc7-125">Minden meglévő helyszíni Active Directory Connector (amely a meglévő Active Directory-erdőben felel meg):</span><span class="sxs-lookup"><span data-stu-id="6ebc7-125">For each existing on-premises Active Directory connector (which corresponds to an existing Active Directory forest):</span></span>

   * <span data-ttu-id="6ebc7-126">Ellenőrzi, hogy engedélyezve van-e a jelszó-szinkronizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-126">Validates that the password synchronization feature is enabled.</span></span>
   
   * <span data-ttu-id="6ebc7-127">Megkeresi a jelszó szinkronizálása szívverés eseményeket a Windows-alkalmazás eseménynaplóiban lesz naplózva.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-127">Searches for password synchronization heartbeat events in the Windows Application Event logs.</span></span>

   * <span data-ttu-id="6ebc7-128">Minden egyes a helyszíni Active Directory-összekötőt az Active Directory-tartomány:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-128">For each Active Directory domain under the on-premises Active Directory connector:</span></span>

      * <span data-ttu-id="6ebc7-129">Ellenőrzi, hogy a tartomány az Azure AD Connect-kiszolgáló elérhető-e.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-129">Validates that the domain is reachable from the Azure AD Connect server.</span></span>

      * <span data-ttu-id="6ebc7-130">Ellenőrzi, hogy az Active Directory tartományi szolgáltatások (AD DS) fiókok, amelyet a helyszíni Active Directory-összekötő rendelkezik-e a megfelelő felhasználónév, jelszó és a jelszó-szinkronizáláshoz szükséges engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-130">Validates that the Active Directory Domain Services (AD DS) accounts used by the on-premises Active Directory connector has the correct username, password, and permissions required for password synchronization.</span></span>

<span data-ttu-id="6ebc7-131">A következő ábra szemlélteti a parancsmag eredményei azoknak az olyan egyetlen tartományból, a helyszíni Active Directory-topológiát:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-131">The following diagram illustrates the results of the cmdlet for a single-domain, on-premises Active Directory topology:</span></span>

![Diagnosztikai kimenetet a jelszó-szinkronizáláshoz](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

<span data-ttu-id="6ebc7-133">Ez a szakasz a többi adott, a parancsmag és a megfelelő problémák által visszaadott eredmények ismerteti.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-133">The rest of this section describes specific results that are returned by the cmdlet and corresponding issues.</span></span>

#### <a name="password-synchronization-feature-isnt-enabled"></a><span data-ttu-id="6ebc7-134">Jelszó-szinkronizálási szolgáltatás nincs engedélyezve</span><span class="sxs-lookup"><span data-stu-id="6ebc7-134">Password synchronization feature isn't enabled</span></span>
<span data-ttu-id="6ebc7-135">Ha még nem engedélyezte a jelszó-szinkronizálás az Azure AD Connect varázsló segítségével, a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-135">If you haven't enabled password synchronization by using the Azure AD Connect wizard, the following error is returned:</span></span>

![A jelszó-szinkronizálás nincs engedélyezve](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a><span data-ttu-id="6ebc7-137">Az Azure AD Connect-kiszolgáló átmeneti módban van</span><span class="sxs-lookup"><span data-stu-id="6ebc7-137">Azure AD Connect server is in staging mode</span></span>
<span data-ttu-id="6ebc7-138">Ha az Azure AD Connect-kiszolgáló az átmeneti környezetű üzemmód, jelszó-szinkronizálás ideiglenesen le van tiltva, és a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-138">If the Azure AD Connect server is in staging mode, password synchronization is temporarily disabled, and the following error is returned:</span></span>

![Az Azure AD Connect-kiszolgáló átmeneti módban van](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a><span data-ttu-id="6ebc7-140">Jelszó szinkronizálási szívverés események</span><span class="sxs-lookup"><span data-stu-id="6ebc7-140">No password synchronization heartbeat events</span></span>
<span data-ttu-id="6ebc7-141">Minden egyes a helyszíni Active Directory-összekötőt a saját jelszó-szinkronizálás csatorna rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-141">Each on-premises Active Directory connector has its own password synchronization channel.</span></span> <span data-ttu-id="6ebc7-142">Ha a jelszó-szinkronizálás csatorna jön létre, és nincs a szinkronizálandó jelszó módosításokat, a szívverés (eseményazonosító 654) jön létre 30 percenként alatt a Windows alkalmazási eseménynaplóba.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-142">When the password synchronization channel is established and there aren't any password changes to be synchronized, a heartbeat event (EventId 654) is generated once every 30 minutes under the Windows Application Event Log.</span></span> <span data-ttu-id="6ebc7-143">Az egyes a helyszíni Active Directory-összekötőt, a parancsmag rákeres a megfelelő szívverés-események az elmúlt három órában.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-143">For each on-premises Active Directory connector, the cmdlet searches for corresponding heartbeat events in the past three hours.</span></span> <span data-ttu-id="6ebc7-144">Ha nincs szívverés esemény található, a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-144">If no heartbeat event is found, the following error is returned:</span></span>

![Nincs jelszó-szinkronizálás szív járulhatnak esemény](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a><span data-ttu-id="6ebc7-146">AD DS fiók nem rendelkezik megfelelő engedélyekkel</span><span class="sxs-lookup"><span data-stu-id="6ebc7-146">AD DS account does not have correct permissions</span></span>
<span data-ttu-id="6ebc7-147">Ha a jelszó-kivonatok szinkronizálását a helyi Active Directory-összekötő által használt Active Directory tartományi szolgáltatások fiók nem rendelkezik megfelelő engedélyekkel, a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-147">If the AD DS account that's used by the on-premises Active Directory connector to synchronize password hashes does not have the appropriate permissions, the following error is returned:</span></span>

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a><span data-ttu-id="6ebc7-149">Active Directory tartományi szolgáltatások fiók helytelen felhasználónév vagy jelszó</span><span class="sxs-lookup"><span data-stu-id="6ebc7-149">Incorrect AD DS account username or password</span></span>
<span data-ttu-id="6ebc7-150">Ha az Active Directory tartományi szolgáltatások jelszókivonatait szinkronizálása a helyszíni Active Directory-összekötő által használt fióknak egy helytelen felhasználónév vagy jelszó, a következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-150">If the AD DS account used by the on-premises Active Directory connector to synchronize password hashes has an incorrect username or password, the following error is returned:</span></span>

![Helytelen hitelesítő adatok](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet"></a><span data-ttu-id="6ebc7-152">Több objektum van nem jelszó-szinkronizálás: a diagnosztikai parancsmaggal hibaelhárításához</span><span class="sxs-lookup"><span data-stu-id="6ebc7-152">One object is not synchronizing passwords: troubleshoot by using the diagnostic cmdlet</span></span>
<span data-ttu-id="6ebc7-153">Használhatja a `Invoke-ADSyncDiagnostics` parancsmag használatával határozza meg, miért egy objektum nem szinkronizál jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-153">You can use the `Invoke-ADSyncDiagnostics` cmdlet to determine why one object is not synchronizing passwords.</span></span>

> [!NOTE]
> <span data-ttu-id="6ebc7-154">A `Invoke-ADSyncDiagnostics` parancsmag verziójához rendelkezésre álló csak az Azure AD Connect 1.1.524.0 vagy újabb verzió.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-154">The `Invoke-ADSyncDiagnostics` cmdlet is available only for Azure AD Connect version 1.1.524.0 or later.</span></span>

### <a name="run-the-diagnostics-cmdlet"></a><span data-ttu-id="6ebc7-155">A diagnosztika parancsmag futtatása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-155">Run the diagnostics cmdlet</span></span>
<span data-ttu-id="6ebc7-156">Ahol nincs jelszavai szinkronizálva vannak-e problémák hibaelhárításához:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-156">To troubleshoot issues where no passwords are synchronized:</span></span>

1. <span data-ttu-id="6ebc7-157">Nyisson meg egy új Windows PowerShell-munkamenetet a Azure AD Connect-kiszolgáló és az a **Futtatás rendszergazdaként** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-157">Open a new Windows PowerShell session on your Azure AD Connect server with the **Run as Administrator** option.</span></span>

2. <span data-ttu-id="6ebc7-158">Futtatás `Set-ExecutionPolicy RemoteSigned` vagy `Set-ExecutionPolicy Unrestricted`.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-158">Run `Set-ExecutionPolicy RemoteSigned` or `Set-ExecutionPolicy Unrestricted`.</span></span>

3. <span data-ttu-id="6ebc7-159">Futtassa az `Import-Module ADSyncDiagnostics` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-159">Run `Import-Module ADSyncDiagnostics`.</span></span>

4. <span data-ttu-id="6ebc7-160">Futtassa a következő parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-160">Run the following cmdlet:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   <span data-ttu-id="6ebc7-161">Példa:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-161">For example:</span></span>
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-the-results-of-the-cmdlet"></a><span data-ttu-id="6ebc7-162">A parancsmag eredményei ismertetése</span><span class="sxs-lookup"><span data-stu-id="6ebc7-162">Understand the results of the cmdlet</span></span>
<span data-ttu-id="6ebc7-163">A diagnosztikai parancsmag a következő műveleteket hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-163">The diagnostic cmdlet performs the following checks:</span></span>

* <span data-ttu-id="6ebc7-164">Az Active Directory connector, Metaverse, és Azure Active Directory-objektum állapotának megvizsgálja AD kapcsolódási térbe.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-164">Examines the state of the Active Directory object in the Active Directory connector space, Metaverse, and Azure AD connector space.</span></span>

* <span data-ttu-id="6ebc7-165">Ellenőrzi, hogy vannak-e a jelszó-szinkronizálás engedélyezve van, és az Active Directory-objektum alkalmazott szinkronizálási szabályait.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-165">Validates that there are synchronization rules with password synchronization enabled and applied to the Active Directory object.</span></span>

* <span data-ttu-id="6ebc7-166">Megkísérli lekérni, és a jelszó az objektum utolsó kísérlet eredményeit jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-166">Attempts to retrieve and display the results of the last attempt to synchronize the password for the object.</span></span>

<span data-ttu-id="6ebc7-167">A következő ábra szemlélteti a parancsmag eredményei egy adott objektum jelszó-szinkronizálás hibaelhárítása során:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-167">The following diagram illustrates the results of the cmdlet when troubleshooting password synchronization for a single object:</span></span>

![A jelszó-szinkronizáláshoz - egyetlen objektumhoz diagnosztikai kimenetet](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

<span data-ttu-id="6ebc7-169">A többi Ez a szakasz ismerteti a parancsmag és a megfelelő problémák által adott eredmények.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-169">The rest of this section describes specific results returned by the cmdlet and corresponding issues.</span></span>

#### <a name="the-active-directory-object-isnt-exported-to-azure-ad"></a><span data-ttu-id="6ebc7-170">Az Active Directory-objektum nem az Azure AD exportálva</span><span class="sxs-lookup"><span data-stu-id="6ebc7-170">The Active Directory object isn't exported to Azure AD</span></span>
<span data-ttu-id="6ebc7-171">A jelszó-szinkronizálás a helyszíni Active Directory-fiók sikertelen lesz, mivel nincs a megfelelő objektum az az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-171">Password synchronization for this on-premises Active Directory account fails because there is no corresponding object in the Azure AD tenant.</span></span> <span data-ttu-id="6ebc7-172">A következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-172">The following error is returned:</span></span>

![Az Azure AD-objektum hiányzik.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a><span data-ttu-id="6ebc7-174">Felhasználó rendelkezik-e egy ideiglenes jelszót</span><span class="sxs-lookup"><span data-stu-id="6ebc7-174">User has a temporary password</span></span>
<span data-ttu-id="6ebc7-175">Jelenleg az Azure AD Connect nem támogatja ideiglenes jelszó-szinkronizálás Azure AD-val.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-175">Currently, Azure AD Connect does not support synchronizing temporary passwords with Azure AD.</span></span> <span data-ttu-id="6ebc7-176">A jelszó ideiglenes tekinthető Ha a **jelszó módosítása a következő bejelentkezéskor** beállítás be van állítva a helyszíni Active Directory felhasználóra.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-176">A password is considered to be temporary if the **Change password at next logon** option is set on the on-premises Active Directory user.</span></span> <span data-ttu-id="6ebc7-177">A következő hibaüzenet:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-177">The following error is returned:</span></span>

![Ideiglenes jelszót ne exportálja.](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-to-synchronize-password-arent-available"></a><span data-ttu-id="6ebc7-179">Jelszó tett legutóbbi kísérlet eredményeit nem érhetők el</span><span class="sxs-lookup"><span data-stu-id="6ebc7-179">Results of last attempt to synchronize password aren't available</span></span>
<span data-ttu-id="6ebc7-180">Alapértelmezés szerint az Azure AD Connect szinkronizálási bejelentkezési kísérletek hét napja eredményeit tárolja.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-180">By default, Azure AD Connect stores the results of password synchronization attempts for seven days.</span></span> <span data-ttu-id="6ebc7-181">Ha nem érhető el a kiválasztott Active Directory-objektum, akkor a következő figyelmeztetést adott vissza:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-181">If there are no results available for the selected Active Directory object, the following warning is returned:</span></span>

![Egyetlen objektum – a Nincs szinkronizálás az előző diagnosztikai kimenete](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a><span data-ttu-id="6ebc7-183">Nincs jelszavai szinkronizálva vannak: manuális hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="6ebc7-183">No passwords are synchronized: manual troubleshooting steps</span></span>
<span data-ttu-id="6ebc7-184">Határozza meg, miért nem jelszavai szinkronizálva vannak az alábbi lépéseket követve:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-184">Follow these steps to determine why no passwords are synchronized:</span></span>

1. <span data-ttu-id="6ebc7-185">A Connect-kiszolgáló a [átmeneti módban](active-directory-aadconnectsync-operations.md#staging-mode)?</span><span class="sxs-lookup"><span data-stu-id="6ebc7-185">Is the Connect server in [staging mode](active-directory-aadconnectsync-operations.md#staging-mode)?</span></span> <span data-ttu-id="6ebc7-186">Átmeneti módban nem szinkronizálja a jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-186">A server in staging mode does not synchronize any passwords.</span></span>

2. <span data-ttu-id="6ebc7-187">Futtassa a parancsfájlt a [jelszó-szinkronizálási beállítások állapotának lekérése](#get-the-status-of-password-sync-settings) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-187">Run the script in the [Get the status of password sync settings](#get-the-status-of-password-sync-settings) section.</span></span> <span data-ttu-id="6ebc7-188">Ez lehetővé teszi a jelszó-szinkronizálás konfigurációs áttekintése.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-188">It gives you an overview of the password sync configuration.</span></span>  

    ![PowerShell parancsfájl kimenete, jelszó-szinkronizálási beállítások](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. <span data-ttu-id="6ebc7-190">Ha a szolgáltatás nincs engedélyezve az Azure ad-ben, vagy ha a szinkronizálási csatorna állapota nem engedélyezett, futtassa a Connect telepítővarázslóját.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-190">If the feature is not enabled in Azure AD or if the sync channel status is not enabled, run the Connect installation wizard.</span></span> <span data-ttu-id="6ebc7-191">Válassza ki **testre szabhatja a szinkronizálási beállítások**, és törölje a jelszó-szinkronizálást. Ez a változás ideiglenesen letiltja a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-191">Select **Customize synchronization options**, and unselect password sync. This change temporarily disables the feature.</span></span> <span data-ttu-id="6ebc7-192">Majd futtassa újra a varázslót, és engedélyezze újra a jelszó-szinkronizálást. Futtassa a ismét ellenőrizze, hogy a konfiguráció helyes-e.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-192">Then run the wizard again and re-enable password sync. Run the script again to verify that the configuration is correct.</span></span>

4. <span data-ttu-id="6ebc7-193">Keressen hibákat az eseménynaplóban találhatók.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-193">Look in the event log for errors.</span></span> <span data-ttu-id="6ebc7-194">Keresse meg a következő események, amelyek azt jelentené, hogy a problémát:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-194">Look for the following events, which would indicate a problem:</span></span>
    * <span data-ttu-id="6ebc7-195">Forrás: "A címtár-szinkronizálás" azonosító: 0, 611, 652, ha ezek az események 655 kapcsolat hibát.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-195">Source: "Directory synchronization" ID: 0, 611, 652, 655 If you see these events, you have a connectivity problem.</span></span> <span data-ttu-id="6ebc7-196">Az eseménynapló-üzenet esetében probléma erdő információkat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-196">The event log message contains forest information where you have a problem.</span></span> <span data-ttu-id="6ebc7-197">További információkért lásd: [csatlakozási probléma](#connectivity problem).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-197">For more information, see [Connectivity problem](#connectivity problem).</span></span>

5. <span data-ttu-id="6ebc7-198">Ha nem szívverés, vagy ha nincs más, futtassa [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-198">If you see no heartbeat or if nothing else worked, run [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span> <span data-ttu-id="6ebc7-199">A parancsfájl csak egyszer futtatnia.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-199">Run the script only once.</span></span>

6. <span data-ttu-id="6ebc7-200">Tekintse meg a [egy olyan objektum, amely nem szinkronizál jelszavak hibaelhárítása](#one-object-is-not-synchronizing-passwords) szakasz.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-200">See the [Troubleshoot one object that is not synchronizing passwords](#one-object-is-not-synchronizing-passwords) section.</span></span>

### <a name="connectivity-problems"></a><span data-ttu-id="6ebc7-201">Kapcsolódási problémák</span><span class="sxs-lookup"><span data-stu-id="6ebc7-201">Connectivity problems</span></span>

<span data-ttu-id="6ebc7-202">Az Azure AD-kapcsolat van?</span><span class="sxs-lookup"><span data-stu-id="6ebc7-202">Do you have connectivity with Azure AD?</span></span>

<span data-ttu-id="6ebc7-203">A fiók rendelkezik az összes tartomány kivonatai a szükséges jogosultságok?</span><span class="sxs-lookup"><span data-stu-id="6ebc7-203">Does the account have required permissions to read the password hashes in all domains?</span></span> <span data-ttu-id="6ebc7-204">Csatlakozás a gyorsbeállítások használatával telepítette, ha az engedélyek már kell helyes-e.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-204">If you installed Connect by using Express settings, the permissions should already be correct.</span></span> 

<span data-ttu-id="6ebc7-205">Ha egyéni telepítés, az engedélyek beállítása manuálisan a következő módon:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-205">If you used custom installation, set the permissions manually by doing the following:</span></span>
    
1. <span data-ttu-id="6ebc7-206">Keresse meg a fiókot az Active Directory-összekötő által használt, indítsa el a **Synchronization Service Managert**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-206">To find the account used by the Active Directory connector, start **Synchronization Service Manager**.</span></span> 
 
2. <span data-ttu-id="6ebc7-207">Ugrás a **összekötők**, majd keresse meg a hibaelhárítási a helyszíni Active Directory-erdőben.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-207">Go to **Connectors**, and then search for the on-premises Active Directory forest you are troubleshooting.</span></span> 
 
3. <span data-ttu-id="6ebc7-208">Válassza ki az összekötőt, és kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-208">Select the connector, and then click **Properties**.</span></span> 
 
4. <span data-ttu-id="6ebc7-209">Ugrás a **csatlakozni az Active Directory Erdőfelderítési**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-209">Go to **Connect to Active Directory Forest**.</span></span>  
    
    ![Active Directory-összekötő által használt fiók](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    <span data-ttu-id="6ebc7-211">Vegye figyelembe a felhasználónév és a fióknak a tartomány.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-211">Note the username and the domain where the account is located.</span></span>
    
5. <span data-ttu-id="6ebc7-212">Start **Active Directory – felhasználók és számítógépek**, és ellenőrizze, hogy a korábban talált fióknak az erdőben lévő összes tartományban gyökérkönyvtárában végezze engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-212">Start **Active Directory Users and Computers**, and then verify that the account you found earlier has the follow permissions set at the root of all domains in your forest:</span></span>
    * <span data-ttu-id="6ebc7-213">Változások replikálása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-213">Replicate Directory Changes</span></span>
    * <span data-ttu-id="6ebc7-214">Replikálása Directory összes módosítása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-214">Replicate Directory Changes All</span></span>

6. <span data-ttu-id="6ebc7-215">A tartományvezérlők elérhetők az Azure AD Connect?</span><span class="sxs-lookup"><span data-stu-id="6ebc7-215">Are the domain controllers reachable by Azure AD Connect?</span></span> <span data-ttu-id="6ebc7-216">Ha a Connect-kiszolgáló nem tud csatlakozni az összes olyan tartományvezérlőn, konfiguráljon **csak az elsődleges tartományvezérlőt használja**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-216">If the Connect server cannot connect to all domain controllers, configure **Only use preferred domain controller**.</span></span>  
    
    ![Az Active Directory-összekötő által használt tartományvezérlő](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. <span data-ttu-id="6ebc7-218">Lépjen vissza a **Synchronization Service Managert** és **címtárpartíciók konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-218">Go back to **Synchronization Service Manager** and **Configure Directory Partition**.</span></span> 
 
8. <span data-ttu-id="6ebc7-219">Válassza ki a tartományt a **válassza ki az a címtárpartíciókat**, jelölje be a **csak használja az elsődleges tartományvezérlő** jelölőnégyzetet, majd kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-219">Select your domain in **Select directory partitions**, select the **Only use preferred domain controllers** check box, and then click **Configure**.</span></span> 

9. <span data-ttu-id="6ebc7-220">A listában adja meg a tartományvezérlőn, amely a jelszó-szinkronizálás használata ajánlott a csatlakozás. Ugyanazt a listát importálásának és exportálásának is szolgál.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-220">In the list, enter the domain controllers that Connect should use for password sync. The same list is used for import and export as well.</span></span> <span data-ttu-id="6ebc7-221">Hajtsa végre ezeket a lépéseket minden tartományban.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-221">Do these steps for all your domains.</span></span>

10. <span data-ttu-id="6ebc7-222">Ha a parancsfájl azt mutatja, hogy nem szívverés, futtassa a parancsfájlt [indul el, a teljes szinkronizálás az összes jelszavak](#trigger-a-full-sync-of-all-passwords).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-222">If the script shows that there is no heartbeat, run the script in [Trigger a full sync of all passwords](#trigger-a-full-sync-of-all-passwords).</span></span>

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a><span data-ttu-id="6ebc7-223">Több objektum van nem jelszó-szinkronizálás: manuális hibaelhárítási lépések</span><span class="sxs-lookup"><span data-stu-id="6ebc7-223">One object is not synchronizing passwords: manual troubleshooting steps</span></span>
<span data-ttu-id="6ebc7-224">Jelszó-szinkronizálás problémák könnyen elháríthatja az egy objektumot állapotának megtekintésével.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-224">You can easily troubleshoot password synchronization issues by reviewing the status of an object.</span></span>

1. <span data-ttu-id="6ebc7-225">A **Active Directory – felhasználók és számítógépek**, keresse meg a felhasználót, és ellenőrizze, hogy a **kell változtatni a jelszót a következő bejelentkezéskor** jelölőnégyzet nincs bejelölve.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-225">In **Active Directory Users and Computers**, search for the user, and then verify that the **User must change password at next logon** check box is cleared.</span></span>  

    ![Active Directory hatékony jelszavak](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    <span data-ttu-id="6ebc7-227">Ha a jelölőnégyzet be van jelölve, kérje meg, hogy jelentkezzen be, és módosítsa a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-227">If the check box is selected, ask the user to sign in and change the password.</span></span> <span data-ttu-id="6ebc7-228">Ideiglenes jelszavak nincsenek szinkronizálva az Azure ad-val.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-228">Temporary passwords are not synchronized with Azure AD.</span></span>

2. <span data-ttu-id="6ebc7-229">Ha a jelszó helyes-e az Active Directoryban, hajtsa végre a szinkronizálási motor a felhasználót.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-229">If the password looks correct in Active Directory, follow the user in the sync engine.</span></span> <span data-ttu-id="6ebc7-230">A felhasználó következő a helyszíni Active Directoryból az Azure AD, láthatja, hogy van-e a leíró hiba az objektum.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-230">By following the user from on-premises Active Directory to Azure AD, you can see whether there is a descriptive error on the object.</span></span>

    <span data-ttu-id="6ebc7-231">a.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-231">a.</span></span> <span data-ttu-id="6ebc7-232">Indítsa el a [Synchronization Service Managert](active-directory-aadconnectsync-service-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-232">Start the [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).</span></span>

    <span data-ttu-id="6ebc7-233">b.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-233">b.</span></span> <span data-ttu-id="6ebc7-234">Kattintson a **összekötők**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-234">Click **Connectors**.</span></span>

    <span data-ttu-id="6ebc7-235">c.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-235">c.</span></span> <span data-ttu-id="6ebc7-236">Válassza ki a **Active Directory-összekötő** ahol a felhasználó tartozik.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-236">Select the **Active Directory Connector** where the user is located.</span></span>

    <span data-ttu-id="6ebc7-237">d.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-237">d.</span></span> <span data-ttu-id="6ebc7-238">Válassza ki **Összekötőtér keresési**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-238">Select **Search Connector Space**.</span></span>

    <span data-ttu-id="6ebc7-239">e.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-239">e.</span></span> <span data-ttu-id="6ebc7-240">Az a **hatókör** mezőben válassza **megkülönböztető név vagy a horgony**, és írja be a teljes DN hibaelhárítási felhasználó.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-240">In the **Scope** box, select **DN or Anchor**, and then enter the full DN of the user you are troubleshooting.</span></span>

    ![A kapcsolódási térbe megkülönböztető névvel rendelkező felhasználó keresése](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    <span data-ttu-id="6ebc7-242">f.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-242">f.</span></span> <span data-ttu-id="6ebc7-243">Keresse meg a felhasználó keres, és kattintson a **tulajdonságok** megtekintéséhez az összes attribútumot.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-243">Locate the user you are looking for, and then click **Properties** to see all the attributes.</span></span> <span data-ttu-id="6ebc7-244">Ha a felhasználó nincs a keresési eredményt, ellenőrizze a [szűrési szabályok](active-directory-aadconnectsync-configure-filtering.md) és győződjön meg arról, hogy [alkalmaz, és a módosítások ellenőrzéséhez](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) csatlakozás megjelennek a felhasználó számára.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-244">If the user is not in the search result, verify your [filtering rules](active-directory-aadconnectsync-configure-filtering.md) and make sure that you run [Apply and verify changes](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) for the user to appear in Connect.</span></span>

    <span data-ttu-id="6ebc7-245">g.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-245">g.</span></span> <span data-ttu-id="6ebc7-246">Kattintson a részletek a jelszó szinkronizálása az objektum az elmúlt héten, **napló**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-246">To see the password sync details of the object for the past week, click **Log**.</span></span>  

    ![Napló részletes adatai](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    <span data-ttu-id="6ebc7-248">Ha az objektum napló üres, az Azure AD Connect nem tudta olvasni a Jelszókivonat az Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-248">If the object log is empty, Azure AD Connect has been unable to read the password hash from Active Directory.</span></span> <span data-ttu-id="6ebc7-249">Folytassa a [kapcsolódási hibák](#connectivity-errors).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-249">Continue your troubleshooting with [Connectivity Errors](#connectivity-errors).</span></span> <span data-ttu-id="6ebc7-250">Ha látja, mint bármely más érték **sikeres**, tekintse meg a táblázatban szereplő [jelszó-szinkronizálás napló](#password-sync-log).</span><span class="sxs-lookup"><span data-stu-id="6ebc7-250">If you see any other value than **success**, refer to the table in [Password sync log](#password-sync-log).</span></span>

    <span data-ttu-id="6ebc7-251">h.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-251">h.</span></span> <span data-ttu-id="6ebc7-252">Válassza ki a **Leszármaztatás** lapot, és győződjön meg arról, hogy legalább egy szinkronizálási szabályának a **PasswordSync** oszlop **igaz**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-252">Select the **lineage** tab, and make sure that at least one sync rule in the **PasswordSync** column is **True**.</span></span> <span data-ttu-id="6ebc7-253">Az alapértelmezett beállítás a szinkronizálási szabály neve nem **a az AD - felhasználó AccountEnabled**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-253">In the default configuration, the name of the sync rule is **In from AD - User AccountEnabled**.</span></span>  

    ![Leszármaztatás felhasználókkal kapcsolatos információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    <span data-ttu-id="6ebc7-255">i.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-255">i.</span></span> <span data-ttu-id="6ebc7-256">Kattintson a **Metaverzum-objektum tulajdonságai** megjelenítése a felhasználói attribútumok listáját.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-256">Click **Metaverse Object Properties** to display a list of user attributes.</span></span>  

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    <span data-ttu-id="6ebc7-258">Győződjön meg arról, hogy nincs **cloudFiltered** attribútumnak jelen.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-258">Verify that there is no **cloudFiltered** attribute present.</span></span> <span data-ttu-id="6ebc7-259">Győződjön meg arról, hogy a tartomány tulajdonságai (domainFQDN és domainNetBios) a várt értékeket.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-259">Make sure that the domain attributes (domainFQDN and domainNetBios) have the expected values.</span></span>

    <span data-ttu-id="6ebc7-260">j.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-260">j.</span></span> <span data-ttu-id="6ebc7-261">Kattintson a **összekötők** fülre. Győződjön meg arról, hogy megjelenik-e a helyszíni Active Directory és az Azure AD-összekötőt.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-261">Click the **Connectors** tab. Make sure that you see connectors to both on-premises Active Directory and Azure AD.</span></span>

    ![Metaverzum-információk](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    <span data-ttu-id="6ebc7-263">k.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-263">k.</span></span> <span data-ttu-id="6ebc7-264">Válassza ki, amely jelöli az Azure ad-vel, kattintson a sor **tulajdonságok**, majd kattintson a **Leszármaztatás** lapon. Az összekötő terület objektumot kell egy kimenő forgalomra vonatkozó szabály a **PasswordSync** oszlop beállítása **igaz**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-264">Select the row that represents Azure AD, click **Properties**, and then click the **Lineage** tab. The connector space object should have an outbound rule in the **PasswordSync** column set to **True**.</span></span> <span data-ttu-id="6ebc7-265">Az alapértelmezett beállítás a szinkronizálási szabály neve nem **ki az AAD - csatlakozás a felhasználói**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-265">In the default configuration, the name of the sync rule is **Out to AAD - User Join**.</span></span>  

    ![Összekötő Összekötőtér-objektum tulajdonságai párbeszédpanel](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a><span data-ttu-id="6ebc7-267">Jelszó-szinkronizálás napló</span><span class="sxs-lookup"><span data-stu-id="6ebc7-267">Password sync log</span></span>
<span data-ttu-id="6ebc7-268">Az Állapot oszlop a következő értékeket veheti fel:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-268">The status column can have the following values:</span></span>

| <span data-ttu-id="6ebc7-269">status</span><span class="sxs-lookup"><span data-stu-id="6ebc7-269">Status</span></span> | <span data-ttu-id="6ebc7-270">Leírás</span><span class="sxs-lookup"><span data-stu-id="6ebc7-270">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6ebc7-271">Sikeres</span><span class="sxs-lookup"><span data-stu-id="6ebc7-271">Success</span></span> |<span data-ttu-id="6ebc7-272">Jelszó szinkronizálása sikerült.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-272">Password has been successfully synchronized.</span></span> |
| <span data-ttu-id="6ebc7-273">FilteredByTarget</span><span class="sxs-lookup"><span data-stu-id="6ebc7-273">FilteredByTarget</span></span> |<span data-ttu-id="6ebc7-274">Jelszó beállítása **kell változtatni a jelszót a következő bejelentkezéskor**.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-274">Password is set to **User must change password at next logon**.</span></span> <span data-ttu-id="6ebc7-275">Jelszó nem lett szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-275">Password has not been synchronized.</span></span> |
| <span data-ttu-id="6ebc7-276">NoTargetConnection</span><span class="sxs-lookup"><span data-stu-id="6ebc7-276">NoTargetConnection</span></span> |<span data-ttu-id="6ebc7-277">A metaverse vagy az Azure AD-kapcsolódási térbe nem objektum.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-277">No object in the metaverse or in the Azure AD connector space.</span></span> |
| <span data-ttu-id="6ebc7-278">SourceConnectorNotPresent</span><span class="sxs-lookup"><span data-stu-id="6ebc7-278">SourceConnectorNotPresent</span></span> |<span data-ttu-id="6ebc7-279">Nem található a helyszíni Active Directory connector területen objektum.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-279">No object found in the on-premises Active Directory connector space.</span></span> |
| <span data-ttu-id="6ebc7-280">TargetNotExportedToDirectory</span><span class="sxs-lookup"><span data-stu-id="6ebc7-280">TargetNotExportedToDirectory</span></span> |<span data-ttu-id="6ebc7-281">Az objektum az az Azure AD-kapcsolódási térbe még nem lett exportálva.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-281">The object in the Azure AD connector space has not yet been exported.</span></span> |
| <span data-ttu-id="6ebc7-282">MigratedCheckDetailsForMoreInfo</span><span class="sxs-lookup"><span data-stu-id="6ebc7-282">MigratedCheckDetailsForMoreInfo</span></span> |<span data-ttu-id="6ebc7-283">Naplóbejegyzés build 1.0.9125.0 előtt készült, és megjelenik-e a korábbi állapotába.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-283">Log entry was created before build 1.0.9125.0 and is shown in its legacy state.</span></span> |
| <span data-ttu-id="6ebc7-284">Hiba</span><span class="sxs-lookup"><span data-stu-id="6ebc7-284">Error</span></span> |<span data-ttu-id="6ebc7-285">Szolgáltatás ismeretlen hibával tért vissza.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-285">Service returned an unknown error.</span></span> |
| <span data-ttu-id="6ebc7-286">Ismeretlen</span><span class="sxs-lookup"><span data-stu-id="6ebc7-286">Unknown</span></span> |<span data-ttu-id="6ebc7-287">Hiba történt a jelszó-kivonatok köteg végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-287">An error occurred while trying to process a batch of password hashes.</span></span>  |
| <span data-ttu-id="6ebc7-288">MissingAttribute</span><span class="sxs-lookup"><span data-stu-id="6ebc7-288">MissingAttribute</span></span> |<span data-ttu-id="6ebc7-289">Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-289">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services are not available.</span></span> |
| <span data-ttu-id="6ebc7-290">RetryRequestedByTarget</span><span class="sxs-lookup"><span data-stu-id="6ebc7-290">RetryRequestedByTarget</span></span> |<span data-ttu-id="6ebc7-291">Adott attribútumok (például a Kerberos-kivonat) Azure AD tartományi szolgáltatások által igényelt korábban nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-291">Specific attributes (for example, Kerberos hash) required by Azure AD Domain Services were not available previously.</span></span> <span data-ttu-id="6ebc7-292">A felhasználó Jelszókivonat újraszinkronizálása kísérlet.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-292">An attempt to resynchronize the user's password hash is made.</span></span> |

## <a name="scripts-to-help-troubleshooting"></a><span data-ttu-id="6ebc7-293">Parancsfájlokat, amelyek segítségével a hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="6ebc7-293">Scripts to help troubleshooting</span></span>

### <a name="get-the-status-of-password-sync-settings"></a><span data-ttu-id="6ebc7-294">Jelszó-szinkronizálási beállítások állapotának lekérése</span><span class="sxs-lookup"><span data-stu-id="6ebc7-294">Get the status of password sync settings</span></span>
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
        Write-Warning "More than one Azure AD Connectors found. Please update the script to use the appropriate Connector."
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

#### <a name="trigger-a-full-sync-of-all-passwords"></a><span data-ttu-id="6ebc7-295">A teljes szinkronizálás az összes jelszavak eseményindító</span><span class="sxs-lookup"><span data-stu-id="6ebc7-295">Trigger a full sync of all passwords</span></span>
> [!NOTE]
> <span data-ttu-id="6ebc7-296">Ez a parancsfájl csak egyszer futtatnia.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-296">Run this script only once.</span></span> <span data-ttu-id="6ebc7-297">Egynél többször futtatásához szükséges, ha valami mást, a probléma.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-297">If you need to run it more than once, something else is the problem.</span></span> <span data-ttu-id="6ebc7-298">A probléma megoldásához forduljon a Microsoft támogatási szolgálatához.</span><span class="sxs-lookup"><span data-stu-id="6ebc7-298">To troubleshoot the problem, contact Microsoft support.</span></span>

<span data-ttu-id="6ebc7-299">Az összes jelszavak teljes szinkronizálásra is elindíthatja az alábbi parancsfájl használatával:</span><span class="sxs-lookup"><span data-stu-id="6ebc7-299">You can trigger a full sync of all passwords by using the following script:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6ebc7-300">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6ebc7-300">Next steps</span></span>
* [<span data-ttu-id="6ebc7-301">A jelszó-szinkronizálás és az Azure AD Connect-szinkronizálás megvalósítása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-301">Implementing password synchronization with Azure AD Connect sync</span></span>](active-directory-aadconnectsync-implement-password-synchronization.md)
* [<span data-ttu-id="6ebc7-302">Az Azure AD Connect-szinkronizálás: Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="6ebc7-302">Azure AD Connect Sync: Customizing synchronization options</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="6ebc7-303">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6ebc7-303">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
