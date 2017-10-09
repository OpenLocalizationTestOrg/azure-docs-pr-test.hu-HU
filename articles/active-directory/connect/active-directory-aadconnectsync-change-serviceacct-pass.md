---
title: "Azure AD Connect szinkronizálása: hello Azure AD Connect szinkronizáláshoz használt szolgáltatásfióknak módosítása |} Microsoft Docs"
description: "Ez a témakör a dokumentum ismerteti a hello titkosítási kulcsot, és hogyan tooabandon után hello jelszó megváltozott."
services: active-directory
keywords: "Azure AD sync szolgáltatás-fiók, jelszó"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: 76b19162-8b16-4960-9e22-bd64e6675ecc
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 11948ac4662f722e4f684ef6c9b9ccdc6387e60f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="changing-hello-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="a00d3-104">Hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavának módosítása</span><span class="sxs-lookup"><span data-stu-id="a00d3-104">Changing hello Azure AD Connect sync service account password</span></span>
<span data-ttu-id="a00d3-105">Ha hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavának módosításához hello szinkronizálási szolgáltatás nem lesz képes start megfelelően elhagyott hello titkosítási kulcs, és újra lesz inicializálva a hello Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="a00d3-105">If you change hello  Azure AD Connect sync service account password, hello Synchronization Service will not be able start correctly until you have abandoned hello encryption key and reinitialized hello Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="a00d3-106">Az Azure AD Connect szinkronizálási szolgáltatások hello részeként egy titkosítási kulcs toostore hello jelszavakat hello Active Directory tartományi szolgáltatások és az Azure AD szolgáltatásfiókok használ.</span><span class="sxs-lookup"><span data-stu-id="a00d3-106">Azure AD Connect, as part of hello Synchronization Services uses an encryption key toostore hello passwords of hello AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="a00d3-107">Ezeket a fiókokat ahhoz, azok hello adatbázis tárolja titkosított.</span><span class="sxs-lookup"><span data-stu-id="a00d3-107">These accounts are encrypted before they are stored in hello database.</span></span> 

<span data-ttu-id="a00d3-108">hello használt titkosítási kulcs használatával lett biztonságossá téve [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="a00d3-108">hello encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="a00d3-109">DPAPI védi hello titkosítási kulcsát hello **hello Azure AD Connect szinkronizálási szolgáltatás fiókja jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="a00d3-109">DPAPI protects hello encryption key using hello **password of hello Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="a00d3-110">Ha toochange hello szolgáltatás fiók jelszavát kell eljárásokkal hello a [Abandoning hello Azure AD Connect Sync titkosítási kulcs](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish ez.</span><span class="sxs-lookup"><span data-stu-id="a00d3-110">If you need toochange hello service account password you can use hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) tooaccomplish this.</span></span>  <span data-ttu-id="a00d3-111">Ezekkel az eljárásokkal kell is használható, ha bármilyen okból tooabandon hello titkosítási kulcs van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a00d3-111">These procedures should also be used if you need tooabandon hello encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-hello-password"></a><span data-ttu-id="a00d3-112">Problémák merülnek fel a hello jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="a00d3-112">Issues that arise from changing hello password</span></span>
<span data-ttu-id="a00d3-113">Két dolgot toobe végre, ha hello szolgáltatás fiók jelszavát módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="a00d3-113">There are two things that need toobe done when you change hello service account password.</span></span>

<span data-ttu-id="a00d3-114">Először toochange hello jelszót adott-e a Windows szolgáltatásvezérlő hello.</span><span class="sxs-lookup"><span data-stu-id="a00d3-114">First, you need toochange hello password under hello Windows Service Control Manager.</span></span>  <span data-ttu-id="a00d3-115">A probléma megoldásáig jelenik meg a következő hibák:</span><span class="sxs-lookup"><span data-stu-id="a00d3-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="a00d3-116">Ha toostart hello szinkronizálási szolgáltatás a Windows szolgáltatásvezérlő, hibaüzenet hello "**Windows nem tudta elindítani a hello Microsoft Azure AD Sync szolgáltatást a helyi számítógépen**".</span><span class="sxs-lookup"><span data-stu-id="a00d3-116">If you try toostart hello Synchronization Service in Windows Service Control Manager, you receive hello error "**Windows could not start hello Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="a00d3-117">**1069. hiba: hello szolgáltatás nem indult el tooa bejelentkezési hiba miatt.** "</span><span class="sxs-lookup"><span data-stu-id="a00d3-117">**Error 1069: hello service did not start due tooa logon failure.**"</span></span>
- <span data-ttu-id="a00d3-118">A Windows Eseménynapló hello rendszer eseménynaplójában található hiba **Event ID 7038** és az üzenet "**hello ADSync szolgáltatás időpontja nem lehet toolog, mint a jelenleg konfigurált hello jelszó miatt toohello alábbi hiba: hello felhasználónév vagy jelszó érvénytelen.** "</span><span class="sxs-lookup"><span data-stu-id="a00d3-118">Under Windows Event Viewer, hello system event log contains an error with **Event ID 7038** and message “**hello ADSync service was unable toolog on as with hello currently configured password due toohello following error: hello user name or password is incorrect.**"</span></span>

<span data-ttu-id="a00d3-119">Második meghatározott feltételek mellett hello jelszó frissül, ha hello Synchronization Service már nem lekérheti hello titkosítási kulcsa a DPAPI-t keresztül.</span><span class="sxs-lookup"><span data-stu-id="a00d3-119">Second, under specific conditions, if hello password is updated, hello Synchronization Service can no longer retrieve hello encryption key via DPAPI.</span></span> <span data-ttu-id="a00d3-120">Hello titkosítási kulcs nélkül hello szinkronizálási szolgáltatás nem tudja visszafejteni a hello jelszavak szükséges toosynchronize és a helyszíni AD és az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a00d3-120">Without hello encryption key, hello Synchronization Service cannot decrypt hello passwords required toosynchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="a00d3-121">Hibák például jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a00d3-121">You will see errors such as:</span></span>

- <span data-ttu-id="a00d3-122">A Windows szolgáltatásvezérlő, próbál toostart hello szinkronizálási szolgáltatás, és nem tudja lekérni hello titkosítási kulcs, ha sikertelen a következő hiba "** Windows nem tudta elindítani a Microsoft Azure AD Sync hello a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="a00d3-122">Under Windows Service Control Manager, if you try toostart hello Synchronization Service and it cannot retrieve hello encryption key, it fails with error “**Windows could not start hello Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="a00d3-123">További információkért tekintse át a hello rendszer-eseménynaplóban találhatók.</span><span class="sxs-lookup"><span data-stu-id="a00d3-123">For more information, review hello System Event log.</span></span> <span data-ttu-id="a00d3-124">Ha ez egy nem Microsoft-szolgáltatást, lépjen kapcsolatba a hello szolgáltatás szállítójával, és tekintse meg a tooservice-specifikus hibakód **-21451857952 x. "</span><span class="sxs-lookup"><span data-stu-id="a00d3-124">If this is a non-Microsoft service, contact hello service vendor, and refer tooservice-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="a00d3-125">A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6028** és hibaüzenet *"**hello kiszolgáló titkosítási kulcs nem érhető el.* *"*</span><span class="sxs-lookup"><span data-stu-id="a00d3-125">Under Windows Event Viewer, hello application event log contains an error with **Event ID 6028** and error message *“**hello server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="a00d3-126">tooensure, hogy nem jelenik meg ezeket a hibákat a hello eljárások [Abandoning hello Azure AD Connect Sync titkosítási kulcs](#abandoning-the-azure-ad-connect-sync-encryption-key) hello jelszó módosításakor.</span><span class="sxs-lookup"><span data-stu-id="a00d3-126">tooensure that you do not receive these errors, follow hello procedures in [Abandoning hello Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing hello password.</span></span>
 
## <a name="abandoning-hello-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="a00d3-127">Hello Azure AD Connect szinkronizálási szolgáltatás titkosítási kulcs megszakítása</span><span class="sxs-lookup"><span data-stu-id="a00d3-127">Abandoning hello Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="a00d3-128">hello alábbi eljárások csak akkor jutnak érvényre tooAzure AD Connect build 1.1.443.0 vagy annál régebbi.</span><span class="sxs-lookup"><span data-stu-id="a00d3-128">hello following procedures only apply tooAzure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="a00d3-129">A következő eljárások tooabandon hello titkosítási kulcs hello használata.</span><span class="sxs-lookup"><span data-stu-id="a00d3-129">Use hello following procedures tooabandon hello encryption key.</span></span>

### <a name="what-toodo-if-you-need-tooabandon-hello-encryption-key"></a><span data-ttu-id="a00d3-130">Milyen toodo, ha szüksége tooabandon hello titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="a00d3-130">What toodo if you need tooabandon hello encryption key</span></span>

<span data-ttu-id="a00d3-131">Ha tooabandon hello titkosítási kulcs van szüksége, használja a következő eljárások tooaccomplish hello ezt.</span><span class="sxs-lookup"><span data-stu-id="a00d3-131">If you need tooabandon hello encryption key, use hello following procedures tooaccomplish this.</span></span>

1. [<span data-ttu-id="a00d3-132">Szakítsa hello meglévő titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="a00d3-132">Abandon hello existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="a00d3-133">Adja meg az AD DS-fiókjához hello hello jelszavát</span><span class="sxs-lookup"><span data-stu-id="a00d3-133">Provide hello password of hello AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="a00d3-134">Hello Azure AD sync fiók jelszavát hello újrainicializálása</span><span class="sxs-lookup"><span data-stu-id="a00d3-134">Reinitialize hello password of hello Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="a00d3-135">Indítsa el a Synchronization Service hello</span><span class="sxs-lookup"><span data-stu-id="a00d3-135">Start hello Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-hello-existing-encryption-key"></a><span data-ttu-id="a00d3-136">Szakítsa hello meglévő titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="a00d3-136">Abandon hello existing encryption key</span></span>
<span data-ttu-id="a00d3-137">Szakítsa hello meglévő titkosítási kulcsot, így az új titkosítási kulcs is létrehozható:</span><span class="sxs-lookup"><span data-stu-id="a00d3-137">Abandon hello existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="a00d3-138">Bejelentkezés tooyour az Azure AD Connect kiszolgáló rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="a00d3-138">Log in tooyour Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="a00d3-139">Indítson el egy új PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="a00d3-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="a00d3-140">Keresse meg a toofolder:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="a00d3-140">Navigate toofolder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="a00d3-141">Hello parancsot:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="a00d3-141">Run hello command: `./miiskmu.exe /a`</span></span>

![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-hello-password-of-hello-ad-ds-account"></a><span data-ttu-id="a00d3-143">Adja meg az AD DS-fiókjához hello hello jelszavát</span><span class="sxs-lookup"><span data-stu-id="a00d3-143">Provide hello password of hello AD DS account</span></span>
<span data-ttu-id="a00d3-144">Hello hello adatbázis tárolt meglévő jelszavakat nem lehet visszafejteni, ahogyan kell tooprovide hello szinkronizálási szolgáltatás hello jelszóval hello Tartományi fiók.</span><span class="sxs-lookup"><span data-stu-id="a00d3-144">As hello existing passwords stored inside hello database can no longer be decrypted, you need tooprovide hello Synchronization Service with hello password of hello AD DS account.</span></span> <span data-ttu-id="a00d3-145">Szinkronizálási szolgáltatás hello hello jelszavak hello új titkosítási kulcs használatával titkosítja:</span><span class="sxs-lookup"><span data-stu-id="a00d3-145">hello Synchronization Service encrypts hello passwords using hello new encryption key:</span></span>

1. <span data-ttu-id="a00d3-146">Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.</span><span class="sxs-lookup"><span data-stu-id="a00d3-146">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="a00d3-147">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="a00d3-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="a00d3-148">Nyissa meg toohello **összekötők** fülre.</span><span class="sxs-lookup"><span data-stu-id="a00d3-148">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="a00d3-149">Jelölje be hello **Címtárösszekötőben** , amely megfelel a tooyour a helyszíni AD.</span><span class="sxs-lookup"><span data-stu-id="a00d3-149">Select hello **AD Connector** that corresponds tooyour on-premises AD.</span></span> <span data-ttu-id="a00d3-150">Ha több AD-összekötő, ismételje meg az egyes azokat a lépéseket követve hello.</span><span class="sxs-lookup"><span data-stu-id="a00d3-150">If you have more than one AD connector, repeat hello following steps for each of them.</span></span>
4. <span data-ttu-id="a00d3-151">A **műveletek**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="a00d3-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="a00d3-152">Hello előugró párbeszédpanelen válassza ki a **tooActive Directory-erdő csatlakozás**:</span><span class="sxs-lookup"><span data-stu-id="a00d3-152">In hello pop-up dialog, select **Connect tooActive Directory Forest**:</span></span>
6. <span data-ttu-id="a00d3-153">Adjon meg Active Directory tartományi szolgáltatások hello fiók jelszavát hello hello **jelszó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="a00d3-153">Enter hello password of hello AD DS account in hello **Password** textbox.</span></span> <span data-ttu-id="a00d3-154">Ha nem ismeri a jelszavát, be kell állítani a lépés végrehajtása előtt érték ismert tooa.</span><span class="sxs-lookup"><span data-stu-id="a00d3-154">If you do not know its password, you must set it tooa known value before performing this step.</span></span>
7. <span data-ttu-id="a00d3-155">Kattintson a **OK** toosave hello új jelszót és Bezárás hello előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="a00d3-155">Click **OK** toosave hello new password and close hello pop-up dialog.</span></span>
<span data-ttu-id="a00d3-156">![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="a00d3-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-hello-password-of-hello-azure-ad-sync-account"></a><span data-ttu-id="a00d3-157">Hello Azure AD sync fiók jelszavát hello újrainicializálása</span><span class="sxs-lookup"><span data-stu-id="a00d3-157">Reinitialize hello password of hello Azure AD sync account</span></span>
<span data-ttu-id="a00d3-158">Közvetlenül a hello Azure AD szolgáltatás fiók toohello szinkronizálási szolgáltatás hello jelszavát nem alkalmas.</span><span class="sxs-lookup"><span data-stu-id="a00d3-158">You cannot directly provide hello password of hello Azure AD service account toohello Synchronization Service.</span></span> <span data-ttu-id="a00d3-159">Ehelyett szüksége toouse hello parancsmag **Add-ADSyncAADServiceAccount** tooreinitialize hello Azure AD-szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="a00d3-159">Instead, you need toouse hello cmdlet **Add-ADSyncAADServiceAccount** tooreinitialize hello Azure AD service account.</span></span> <span data-ttu-id="a00d3-160">hello parancsmag hello fiók jelszavának alaphelyzetbe állítása, és lehetővé teszi az elérhető toohello szinkronizálási szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="a00d3-160">hello cmdlet resets hello account password and makes it available toohello Synchronization Service:</span></span>

1. <span data-ttu-id="a00d3-161">Indítsa el egy új PowerShell-munkamenetet a hello Azure AD Connect-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="a00d3-161">Start a new PowerShell session on hello Azure AD Connect server.</span></span>
2. <span data-ttu-id="a00d3-162">Parancsmag futtatásához `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="a00d3-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="a00d3-163">Hello előugró párbeszédpanelen adja meg az Azure AD-bérlő hello Azure AD globális rendszergazda hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="a00d3-163">In hello pop-up dialog, provide hello Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="a00d3-164">![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="a00d3-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="a00d3-165">Ha sikeres, megjelenik a hello PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="a00d3-165">If it is successful, you will see hello PowerShell command prompt.</span></span>

#### <a name="start-hello-synchronization-service"></a><span data-ttu-id="a00d3-166">Indítsa el a Synchronization Service hello</span><span class="sxs-lookup"><span data-stu-id="a00d3-166">Start hello Synchronization Service</span></span>
<span data-ttu-id="a00d3-167">Most, hogy a szinkronizálási szolgáltatás hello hozzáférés toohello titkosítási kulccsal rendelkezik, ezért minden hello jelszavak kell, a Windows szolgáltatásvezérlő hello újraindíthatja hello szolgáltatást:</span><span class="sxs-lookup"><span data-stu-id="a00d3-167">Now that hello Synchronization Service has access toohello encryption key and all hello passwords it needs, you can restart hello service in hello Windows Service Control Manager:</span></span>


1. <span data-ttu-id="a00d3-168">Nyissa meg a szolgáltatásvezérlő kezelőjéhez (KEZDŐ → szolgáltatások) tooWindows.</span><span class="sxs-lookup"><span data-stu-id="a00d3-168">Go tooWindows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="a00d3-169">Válassza ki **Microsoft Azure AD Sync** és levő Újraindítás lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="a00d3-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a00d3-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a00d3-170">Next steps</span></span>
<span data-ttu-id="a00d3-171">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="a00d3-171">**Overview topics**</span></span>

* [<span data-ttu-id="a00d3-172">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="a00d3-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="a00d3-173">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a00d3-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
