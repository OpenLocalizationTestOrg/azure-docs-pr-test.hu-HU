---
title: "Azure AD Connect szinkronizálása: az Azure AD Connect szinkronizálási szolgáltatás fiók módosítása |} Microsoft Docs"
description: "Ez a témakör a dokumentum útmutatást nyújt a titkosítási kulcsot és abandon azt a jelszó módosítása után."
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
ms.openlocfilehash: bf6234d0810f870909957ee1c1e33c225a4922b9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="changing-the-azure-ad-connect-sync-service-account-password"></a><span data-ttu-id="6b364-104">Az Azure AD Connect szinkronizálási szolgáltatásfiók jelszavának módosítása</span><span class="sxs-lookup"><span data-stu-id="6b364-104">Changing the Azure AD Connect sync service account password</span></span>
<span data-ttu-id="6b364-105">Ha módosítja a Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát, a szinkronizálási szolgáltatás nem lesz képes start megfelelően félbehagyná a titkosítási kulcs, és újra lesz inicializálva a Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b364-105">If you change the  Azure AD Connect sync service account password, the Synchronization Service will not be able start correctly until you have abandoned the encryption key and reinitialized the Azure AD Connect sync service account password.</span></span> 

<span data-ttu-id="6b364-106">Az Azure AD Connect, a szinkronizálási szolgáltatások részeként titkosítási kulcs tárolására használja az Active Directory tartományi szolgáltatások és az Azure AD a szolgáltatásfiókok jelszavait.</span><span class="sxs-lookup"><span data-stu-id="6b364-106">Azure AD Connect, as part of the Synchronization Services uses an encryption key to store the passwords of the AD DS and Azure AD service accounts.</span></span>  <span data-ttu-id="6b364-107">Ezeket a fiókokat ahhoz, azok vannak tárolva az adatbázisban titkosított.</span><span class="sxs-lookup"><span data-stu-id="6b364-107">These accounts are encrypted before they are stored in the database.</span></span> 

<span data-ttu-id="6b364-108">A használt titkosítási kulcs használatával lett biztonságossá téve [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b364-108">The encryption key used is secured using [Windows Data Protection (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx).</span></span> <span data-ttu-id="6b364-109">DPAPI védi, a titkosítási kulcs használatával a **az Azure AD Connect szinkronizálási szolgáltatás fiók jelszavát**.</span><span class="sxs-lookup"><span data-stu-id="6b364-109">DPAPI protects the encryption key using the **password of the Azure AD Connect sync service account**.</span></span> 

<span data-ttu-id="6b364-110">Ha a szolgáltatásfiók jelszavát módosítani kell az eljárásokkal [az Azure AD Connect szinkronizálási szolgáltatás titkosítási kulcs kivonásának](#abandoning-the-azure-ad-connect-sync-encryption-key) ehhez.</span><span class="sxs-lookup"><span data-stu-id="6b364-110">If you need to change the service account password you can use the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) to accomplish this.</span></span>  <span data-ttu-id="6b364-111">Ezekkel az eljárásokkal kell is használható, ha bármilyen okból a titkosítási kulcs abandon kell.</span><span class="sxs-lookup"><span data-stu-id="6b364-111">These procedures should also be used if you need to abandon the encryption key for any reason.</span></span>

##<a name="issues-that-arise-from-changing-the-password"></a><span data-ttu-id="6b364-112">A jelszó módosításának felmerülő problémák</span><span class="sxs-lookup"><span data-stu-id="6b364-112">Issues that arise from changing the password</span></span>
<span data-ttu-id="6b364-113">Két dolgot kell elvégezni, ha módosítja a szolgáltatás fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b364-113">There are two things that need to be done when you change the service account password.</span></span>

<span data-ttu-id="6b364-114">Először módosítani szeretné a jelszó alapján a Windows szolgáltatásvezérlő kezelőjétől.</span><span class="sxs-lookup"><span data-stu-id="6b364-114">First, you need to change the password under the Windows Service Control Manager.</span></span>  <span data-ttu-id="6b364-115">A probléma megoldásáig jelenik meg a következő hibák:</span><span class="sxs-lookup"><span data-stu-id="6b364-115">Until this issue is resolved you will see following errors:</span></span>


- <span data-ttu-id="6b364-116">Indítsa el a Synchronization Service Windows szolgáltatásvezérlő kísérli meg, ha a hibaüzenet "**Windows nem sikerült elindítani a Microsoft Azure AD Sync szolgáltatás a helyi számítógépen**".</span><span class="sxs-lookup"><span data-stu-id="6b364-116">If you try to start the Synchronization Service in Windows Service Control Manager, you receive the error "**Windows could not start the Microsoft Azure AD Sync service on Local Computer**".</span></span> <span data-ttu-id="6b364-117">**1069. hiba: A szolgáltatás nem indult el bejelentkezési hiba miatt.** "</span><span class="sxs-lookup"><span data-stu-id="6b364-117">**Error 1069: The service did not start due to a logon failure.**"</span></span>
- <span data-ttu-id="6b364-118">A Windows Eseménynapló, a rendszer eseménynaplójában található hiba **Event ID 7038** és az üzenet "**az ADSync szolgáltatás nem tudott bejelentkezni csakúgy, mint a jelenleg konfigurált jelszóval a következő hiba miatt: A felhasználó név vagy a jelszó nem megfelelő.** "</span><span class="sxs-lookup"><span data-stu-id="6b364-118">Under Windows Event Viewer, the system event log contains an error with **Event ID 7038** and message “**The ADSync service was unable to log on as with the currently configured password due to the following error: The user name or password is incorrect.**"</span></span>

<span data-ttu-id="6b364-119">Második meghatározott feltételek mellett frissítése után a jelszót, a szinkronizálási szolgáltatást is már nem beolvasni a titkosítási kulcsot DPAPI keresztül.</span><span class="sxs-lookup"><span data-stu-id="6b364-119">Second, under specific conditions, if the password is updated, the Synchronization Service can no longer retrieve the encryption key via DPAPI.</span></span> <span data-ttu-id="6b364-120">A titkosítási kulcs nélkül a szinkronizálási szolgáltatás nem tudja visszafejteni a jelszavak szinkronizálása vagy a helyszíni AD és az Azure AD szükséges.</span><span class="sxs-lookup"><span data-stu-id="6b364-120">Without the encryption key, the Synchronization Service cannot decrypt the passwords required to synchronize to/from on-premises AD and Azure AD.</span></span>
<span data-ttu-id="6b364-121">Hibák például jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="6b364-121">You will see errors such as:</span></span>

- <span data-ttu-id="6b364-122">A Windows szolgáltatásvezérlő, ha a szinkronizálási szolgáltatás indításakor, és nem tudja lekérni a titkosítási kulcs sikertelen a következő hiba "** a Windows nem sikerült elindítani a Microsoft Azure AD Sync helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="6b364-122">Under Windows Service Control Manager, if you try to start the Synchronization Service and it cannot retrieve the encryption key, it fails with error “**Windows could not start the Microsoft Azure AD Sync on Local Computer.</span></span> <span data-ttu-id="6b364-123">További információkért tekintse át a rendszer-eseménynaplóban találhatók.</span><span class="sxs-lookup"><span data-stu-id="6b364-123">For more information, review the System Event log.</span></span> <span data-ttu-id="6b364-124">Ha ez egy nem Microsoft-szolgáltatás, a szállítót, és a szolgáltatásspecifikus hibakódot **-21451857952 x. "</span><span class="sxs-lookup"><span data-stu-id="6b364-124">If this is a non-Microsoft service, contact the service vendor, and refer to service-specific error code **-21451857952****.”</span></span>
- <span data-ttu-id="6b364-125">A Windows eseménynaplóban, az alkalmazások eseménynaplójában keresse meg a hibát tartalmaz **Event ID 6028** és hibaüzenet *"**nem érhető el a kiszolgáló titkosítási kulcsához.* *"*</span><span class="sxs-lookup"><span data-stu-id="6b364-125">Under Windows Event Viewer, the application event log contains an error with **Event ID 6028** and error message *“**The server encryption key cannot be accessed.**”*</span></span>

<span data-ttu-id="6b364-126">Győződjön meg arról, hogy nem jelenik meg ezeket a hibákat, kövesse az ismertetett [az Azure AD Connect szinkronizálási szolgáltatás titkosítási kulcs kivonásának](#abandoning-the-azure-ad-connect-sync-encryption-key) Ha megváltoztatta a jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b364-126">To ensure that you do not receive these errors, follow the procedures in [Abandoning the Azure AD Connect Sync encryption key](#abandoning-the-azure-ad-connect-sync-encryption-key) when changing the password.</span></span>
 
## <a name="abandoning-the-azure-ad-connect-sync-encryption-key"></a><span data-ttu-id="6b364-127">Az Azure AD Connect szinkronizálási szolgáltatás titkosítási kulcs megszakítása</span><span class="sxs-lookup"><span data-stu-id="6b364-127">Abandoning the Azure AD Connect Sync encryption key</span></span>
>[!IMPORTANT]
><span data-ttu-id="6b364-128">A következő eljárásokat csak alkalmazni az Azure AD Connect build 1.1.443.0 vagy annál régebbi.</span><span class="sxs-lookup"><span data-stu-id="6b364-128">The following procedures only apply to Azure AD Connect build 1.1.443.0 or older.</span></span>

<span data-ttu-id="6b364-129">Az alábbi eljárásokkal kénytelen volt megszakítani ezt a titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="6b364-129">Use the following procedures to abandon the encryption key.</span></span>

### <a name="what-to-do-if-you-need-to-abandon-the-encryption-key"></a><span data-ttu-id="6b364-130">Mi a teendő, ha szüksége kénytelen volt megszakítani ezt a titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="6b364-130">What to do if you need to abandon the encryption key</span></span>

<span data-ttu-id="6b364-131">Ha a titkosítási kulcs abandon van szüksége, az alábbi eljárásokkal ehhez.</span><span class="sxs-lookup"><span data-stu-id="6b364-131">If you need to abandon the encryption key, use the following procedures to accomplish this.</span></span>

1. [<span data-ttu-id="6b364-132">Szakítsa meg a meglévő titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="6b364-132">Abandon the existing encryption key</span></span>](#abandon-the-existing-encryption-key)

2. [<span data-ttu-id="6b364-133">Adja meg az Active Directory tartományi szolgáltatások fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="6b364-133">Provide the password of the AD DS account</span></span>](#provide-the-password-of-the-ad-ds-account)

3. [<span data-ttu-id="6b364-134">Inicializálja újra az Azure AD sync fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="6b364-134">Reinitialize the password of the Azure AD sync account</span></span>](#reinitialize-the-password-of-the-azure-ad-sync-account)

4. [<span data-ttu-id="6b364-135">Indítsa el a Synchronization Service</span><span class="sxs-lookup"><span data-stu-id="6b364-135">Start the Synchronization Service</span></span>](#start-the-synchronization-service)

#### <a name="abandon-the-existing-encryption-key"></a><span data-ttu-id="6b364-136">Szakítsa meg a meglévő titkosítási kulcs</span><span class="sxs-lookup"><span data-stu-id="6b364-136">Abandon the existing encryption key</span></span>
<span data-ttu-id="6b364-137">A meglévő titkosítási kulcs Abandon, így az új titkosítási kulcs is létrehozható:</span><span class="sxs-lookup"><span data-stu-id="6b364-137">Abandon the existing encryption key so that new encryption key can be created:</span></span>

1. <span data-ttu-id="6b364-138">Jelentkezzen be rendszergazdaként az Azure AD Connect-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6b364-138">Log in to your Azure AD Connect Server as administrator.</span></span>

2. <span data-ttu-id="6b364-139">Indítson el egy új PowerShell-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="6b364-139">Start a new PowerShell session.</span></span>

3. <span data-ttu-id="6b364-140">Navigáljon a következő mappához:`$env:Program Files\Microsoft Azure AD Sync\bin\`</span><span class="sxs-lookup"><span data-stu-id="6b364-140">Navigate to folder: `$env:Program Files\Microsoft Azure AD Sync\bin\`</span></span>

4. <span data-ttu-id="6b364-141">Futtassa a parancsot:`./miiskmu.exe /a`</span><span class="sxs-lookup"><span data-stu-id="6b364-141">Run the command: `./miiskmu.exe /a`</span></span>

![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key5.png)

#### <a name="provide-the-password-of-the-ad-ds-account"></a><span data-ttu-id="6b364-143">Adja meg az Active Directory tartományi szolgáltatások fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="6b364-143">Provide the password of the AD DS account</span></span>
<span data-ttu-id="6b364-144">A meglévő az adatbázisban tárolt jelszavak már nem lehet visszafejteni, mert meg kell adnia a szinkronizálási szolgáltatás az Active Directory tartományi szolgáltatások fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b364-144">As the existing passwords stored inside the database can no longer be decrypted, you need to provide the Synchronization Service with the password of the AD DS account.</span></span> <span data-ttu-id="6b364-145">A szinkronizálási szolgáltatás titkosítja a jelszavakat az új titkosítási kulcs használatával:</span><span class="sxs-lookup"><span data-stu-id="6b364-145">The Synchronization Service encrypts the passwords using the new encryption key:</span></span>

1. <span data-ttu-id="6b364-146">Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="6b364-146">Start the Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="6b364-147">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="6b364-147">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>  
2. <span data-ttu-id="6b364-148">Lépjen a **összekötők** fülre.</span><span class="sxs-lookup"><span data-stu-id="6b364-148">Go to the **Connectors** tab.</span></span>
3. <span data-ttu-id="6b364-149">Válassza ki a **Címtárösszekötőben** , amely megfelel a helyszíni AD.</span><span class="sxs-lookup"><span data-stu-id="6b364-149">Select the **AD Connector** that corresponds to your on-premises AD.</span></span> <span data-ttu-id="6b364-150">Ha több AD-összekötő, esetében ismételje meg az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6b364-150">If you have more than one AD connector, repeat the following steps for each of them.</span></span>
4. <span data-ttu-id="6b364-151">A **műveletek**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="6b364-151">Under **Actions**, select **Properties**.</span></span>
5. <span data-ttu-id="6b364-152">Az előugró párbeszédpanelen válassza ki a **csatlakozás az Active Directory-erdő**:</span><span class="sxs-lookup"><span data-stu-id="6b364-152">In the pop-up dialog, select **Connect to Active Directory Forest**:</span></span>
6. <span data-ttu-id="6b364-153">Adja meg az AD DS-fiókjához tartozó jelszót a **jelszó** szövegmező.</span><span class="sxs-lookup"><span data-stu-id="6b364-153">Enter the password of the AD DS account in the **Password** textbox.</span></span> <span data-ttu-id="6b364-154">Ha nem ismeri a jelszavát, be kell állítani egy ismert értékre a lépés végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="6b364-154">If you do not know its password, you must set it to a known value before performing this step.</span></span>
7. <span data-ttu-id="6b364-155">Kattintson a **OK** az új jelszó mentse és zárja be az előugró párbeszédpanelen.</span><span class="sxs-lookup"><span data-stu-id="6b364-155">Click **OK** to save the new password and close the pop-up dialog.</span></span>
<span data-ttu-id="6b364-156">![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="6b364-156">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>

#### <a name="reinitialize-the-password-of-the-azure-ad-sync-account"></a><span data-ttu-id="6b364-157">Inicializálja újra az Azure AD sync fiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="6b364-157">Reinitialize the password of the Azure AD sync account</span></span>
<span data-ttu-id="6b364-158">A szinkronizálási szolgáltatás közvetlenül nem adja meg az Azure AD szolgáltatás fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="6b364-158">You cannot directly provide the password of the Azure AD service account to the Synchronization Service.</span></span> <span data-ttu-id="6b364-159">Ehelyett használja a parancsmagot kell **Add-ADSyncAADServiceAccount** újrainicializálni az Azure AD szolgáltatás-fiókot.</span><span class="sxs-lookup"><span data-stu-id="6b364-159">Instead, you need to use the cmdlet **Add-ADSyncAADServiceAccount** to reinitialize the Azure AD service account.</span></span> <span data-ttu-id="6b364-160">A parancsmag a fiók jelszavának alaphelyzetbe állítása, és lehetővé teszi a szinkronizálási szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="6b364-160">The cmdlet resets the account password and makes it available to the Synchronization Service:</span></span>

1. <span data-ttu-id="6b364-161">Új PowerShell-munkamenet el az Azure AD Connect-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="6b364-161">Start a new PowerShell session on the Azure AD Connect server.</span></span>
2. <span data-ttu-id="6b364-162">Parancsmag futtatásához `Add-ADSyncAADServiceAccount`.</span><span class="sxs-lookup"><span data-stu-id="6b364-162">Run cmdlet `Add-ADSyncAADServiceAccount`.</span></span>
3. <span data-ttu-id="6b364-163">Az előugró párbeszédpanelen adja meg az Azure AD globális rendszergazda hitelesítő adatait az Azure AD-bérlő.</span><span class="sxs-lookup"><span data-stu-id="6b364-163">In the pop-up dialog, provide the Azure AD Global admin credentials for your Azure AD tenant.</span></span>
<span data-ttu-id="6b364-164">![Az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key7.png)</span><span class="sxs-lookup"><span data-stu-id="6b364-164">![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key7.png)</span></span>
4. <span data-ttu-id="6b364-165">Ha sikeres, megjelenik a PowerShell-parancssort.</span><span class="sxs-lookup"><span data-stu-id="6b364-165">If it is successful, you will see the PowerShell command prompt.</span></span>

#### <a name="start-the-synchronization-service"></a><span data-ttu-id="6b364-166">Indítsa el a Synchronization Service</span><span class="sxs-lookup"><span data-stu-id="6b364-166">Start the Synchronization Service</span></span>
<span data-ttu-id="6b364-167">Most, hogy a szinkronizálási szolgáltatás hozzáfér a titkosítási kulcsot és a szükséges jelszavak, újraindíthatja a szolgáltatást a a Windows szolgáltatásvezérlő:</span><span class="sxs-lookup"><span data-stu-id="6b364-167">Now that the Synchronization Service has access to the encryption key and all the passwords it needs, you can restart the service in the Windows Service Control Manager:</span></span>


1. <span data-ttu-id="6b364-168">Nyissa meg a Windows szolgáltatásvezérlő kezelőjéhez (KEZDŐ → szolgáltatások).</span><span class="sxs-lookup"><span data-stu-id="6b364-168">Go to Windows Service Control Manager (START → Services).</span></span>
2. <span data-ttu-id="6b364-169">Válassza ki **Microsoft Azure AD Sync** és levő Újraindítás lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="6b364-169">Select **Microsoft Azure AD Sync** and click Restart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b364-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6b364-170">Next steps</span></span>
<span data-ttu-id="6b364-171">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="6b364-171">**Overview topics**</span></span>

* [<span data-ttu-id="6b364-172">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="6b364-172">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="6b364-173">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="6b364-173">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
