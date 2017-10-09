---
title: "az Azure AD Synchronization Service Manager felhasználói felületén hello aaaConnectors |} Microsoft Docs"
description: "Ismerje meg, az Azure AD Connect hello összekötők lapján hello Synchronization Service Managert."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 60f1d979-8e6d-4460-aaab-747fffedfc1e
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c0969630313178b1e299385b1289360c8f787cb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-connectors-with-hello-azure-ad-connect-sync-service-manager"></a><span data-ttu-id="53b6b-103">Összekötők használata az Azure AD Connect szinkronizálási szolgáltatás Manager hello</span><span class="sxs-lookup"><span data-stu-id="53b6b-103">Using connectors with hello Azure AD Connect Sync Service Manager</span></span>

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectors.png)

<span data-ttu-id="53b6b-105">hello összekötők lapon használt toomanage összes rendszerek hello szinkronizálási motor van csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="53b6b-105">hello Connectors tab is used toomanage all systems hello sync engine is connected to.</span></span>

## <a name="connector-actions"></a><span data-ttu-id="53b6b-106">Összekötő műveletek</span><span class="sxs-lookup"><span data-stu-id="53b6b-106">Connector actions</span></span>
| <span data-ttu-id="53b6b-107">Műveletek</span><span class="sxs-lookup"><span data-stu-id="53b6b-107">Action</span></span> | <span data-ttu-id="53b6b-108">Megjegyzés</span><span class="sxs-lookup"><span data-stu-id="53b6b-108">Comment</span></span> |
| --- | --- |
| <span data-ttu-id="53b6b-109">Létrehozás</span><span class="sxs-lookup"><span data-stu-id="53b6b-109">Create</span></span> |<span data-ttu-id="53b6b-110">Ne használjon.</span><span class="sxs-lookup"><span data-stu-id="53b6b-110">Do not use.</span></span> <span data-ttu-id="53b6b-111">Csatlakozás tooadditional AD erdők, hello telepítés varázsló használatával.</span><span class="sxs-lookup"><span data-stu-id="53b6b-111">For connecting tooadditional AD forests, use hello installation wizard.</span></span> |
| <span data-ttu-id="53b6b-112">Tulajdonságok</span><span class="sxs-lookup"><span data-stu-id="53b6b-112">Properties</span></span> |<span data-ttu-id="53b6b-113">Tartomány és szervezeti egységek szűrése használható.</span><span class="sxs-lookup"><span data-stu-id="53b6b-113">Used for domain and OU filtering.</span></span> |
| [<span data-ttu-id="53b6b-114">Törlés</span><span class="sxs-lookup"><span data-stu-id="53b6b-114">Delete</span></span>](#delete) |<span data-ttu-id="53b6b-115">Használt tooeither hello összekötő szóköz vagy toodelete kapcsolat tooa erdő hello adatok törlése.</span><span class="sxs-lookup"><span data-stu-id="53b6b-115">Used tooeither delete hello data in hello connector space or toodelete connection tooa forest.</span></span> |
| [<span data-ttu-id="53b6b-116">Futtatási profilok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="53b6b-116">Configure Run Profiles</span></span>](#configure-run-profiles) |<span data-ttu-id="53b6b-117">Szűrés, semmi nem tartományi kivételével tooconfigure itt.</span><span class="sxs-lookup"><span data-stu-id="53b6b-117">Except for domain filtering, nothing tooconfigure here.</span></span> <span data-ttu-id="53b6b-118">Ez a művelet is használhatja a futtatási profil toosee már be van állítva.</span><span class="sxs-lookup"><span data-stu-id="53b6b-118">You can use this action toosee already configured run profiles.</span></span> |
| <span data-ttu-id="53b6b-119">Futtassa a következőt:</span><span class="sxs-lookup"><span data-stu-id="53b6b-119">Run</span></span> |<span data-ttu-id="53b6b-120">Egy ad hoc futtassa egy profil toostart használt.</span><span class="sxs-lookup"><span data-stu-id="53b6b-120">Used toostart a one-off run of a profile.</span></span> |
| <span data-ttu-id="53b6b-121">Leállítás</span><span class="sxs-lookup"><span data-stu-id="53b6b-121">Stop</span></span> |<span data-ttu-id="53b6b-122">Jelenleg fut egy profil összekötő leáll.</span><span class="sxs-lookup"><span data-stu-id="53b6b-122">Stops a Connector currently running a profile.</span></span> |
| <span data-ttu-id="53b6b-123">Összekötő exportálása</span><span class="sxs-lookup"><span data-stu-id="53b6b-123">Export Connector</span></span> |<span data-ttu-id="53b6b-124">Ne használjon.</span><span class="sxs-lookup"><span data-stu-id="53b6b-124">Do not use.</span></span> |
| <span data-ttu-id="53b6b-125">Összekötő importálása</span><span class="sxs-lookup"><span data-stu-id="53b6b-125">Import Connector</span></span> |<span data-ttu-id="53b6b-126">Ne használjon.</span><span class="sxs-lookup"><span data-stu-id="53b6b-126">Do not use.</span></span> |
| <span data-ttu-id="53b6b-127">Frissítés összekötő</span><span class="sxs-lookup"><span data-stu-id="53b6b-127">Update Connector</span></span> |<span data-ttu-id="53b6b-128">Ne használjon.</span><span class="sxs-lookup"><span data-stu-id="53b6b-128">Do not use.</span></span> |
| <span data-ttu-id="53b6b-129">Séma frissítése</span><span class="sxs-lookup"><span data-stu-id="53b6b-129">Refresh Schema</span></span> |<span data-ttu-id="53b6b-130">Hello gyorsítótárazott séma frissítése.</span><span class="sxs-lookup"><span data-stu-id="53b6b-130">Refreshes hello cached schema.</span></span> <span data-ttu-id="53b6b-131">Azt a beállítás előnyben részesített toouse hello hello telepítővarázslóban ehelyett óta, amely is frissítések szabályok szinkronizálása.</span><span class="sxs-lookup"><span data-stu-id="53b6b-131">It is preferred toouse hello option in hello installation wizard instead, since that also updates sync rules.</span></span> |
| [<span data-ttu-id="53b6b-132">Összekötőtér keresése</span><span class="sxs-lookup"><span data-stu-id="53b6b-132">Search Connector Space</span></span>](#search-connector-space) |<span data-ttu-id="53b6b-133">Felhasznált toofind objektum és túl[hajtsa végre az objektum és az adatok hello rendszeren keresztül](#follow-an-object-and-its-data-through-the-system).</span><span class="sxs-lookup"><span data-stu-id="53b6b-133">Used toofind objects and too[Follow an object and its data through hello system](#follow-an-object-and-its-data-through-the-system).</span></span> |

### <a name="delete"></a><span data-ttu-id="53b6b-134">Törlés</span><span class="sxs-lookup"><span data-stu-id="53b6b-134">Delete</span></span>
<span data-ttu-id="53b6b-135">két különböző fogalom hello delete művelet használható.</span><span class="sxs-lookup"><span data-stu-id="53b6b-135">hello delete action is used for two different things.</span></span>  
![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/connectordelete.png)

<span data-ttu-id="53b6b-137">a beállítás hello **törlése csak a kapcsolódási térbe** eltávolítható az összes adat, de tartsa hello konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="53b6b-137">hello option **Delete connector space only** removes all data, but keep hello configuration.</span></span>

<span data-ttu-id="53b6b-138">beállítás hello **összekötő törlése és a connector** hello adatot és konfigurációs hello eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="53b6b-138">hello option **Delete Connector and connector space** removes hello data and hello configuration.</span></span> <span data-ttu-id="53b6b-139">Ez a beállítás akkor használatos, ha a nem kívánt tooconnect tooa erdő többé.</span><span class="sxs-lookup"><span data-stu-id="53b6b-139">This option is used when you do not want tooconnect tooa forest anymore.</span></span>

<span data-ttu-id="53b6b-140">Mindkét lehetőség minden objektumokat szinkronizálni, és frissítése hello metaverzum-objektum.</span><span class="sxs-lookup"><span data-stu-id="53b6b-140">Both options sync all objects and update hello metaverse objects.</span></span> <span data-ttu-id="53b6b-141">Ez a művelet hosszú ideig futó művelet.</span><span class="sxs-lookup"><span data-stu-id="53b6b-141">This action is a long running operation.</span></span>

### <a name="configure-run-profiles"></a><span data-ttu-id="53b6b-142">Futtatási profilok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="53b6b-142">Configure Run Profiles</span></span>
<span data-ttu-id="53b6b-143">Ez a beállítás lehetővé teszi toosee hello futtatási profil összekötő konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="53b6b-143">This option allows you toosee hello run profiles configured for a Connector.</span></span>

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/configurerunprofiles.png)

### <a name="search-connector-space"></a><span data-ttu-id="53b6b-145">Összekötőtér keresése</span><span class="sxs-lookup"><span data-stu-id="53b6b-145">Search Connector Space</span></span>
<span data-ttu-id="53b6b-146">hello keresési összekötő terület művelet hasznos toofind objektumok és adatok problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="53b6b-146">hello search connector space action is useful toofind objects and troubleshoot data issues.</span></span>

![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearch.png)

<span data-ttu-id="53b6b-148">Kiválasztásával indítsa el a **hatókör**.</span><span class="sxs-lookup"><span data-stu-id="53b6b-148">Start by selecting a **scope**.</span></span> <span data-ttu-id="53b6b-149">Kereshet az adatok alapján (RDN, DN, rögzítési, részfájának), vagy állami hello objektum (minden más beállítás).</span><span class="sxs-lookup"><span data-stu-id="53b6b-149">You can search based on data (RDN, DN, Anchor, Sub-Tree), or state of hello object (all other options).</span></span>  
<span data-ttu-id="53b6b-150">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span><span class="sxs-lookup"><span data-stu-id="53b6b-150">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchscope.png)</span></span>  
<span data-ttu-id="53b6b-151">Ha például egy részfájának keresést, egy szervezeti egység összes objektum beolvasása.</span><span class="sxs-lookup"><span data-stu-id="53b6b-151">If you for example do a Sub-Tree search, you get all objects in one OU.</span></span>  
<span data-ttu-id="53b6b-152">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span><span class="sxs-lookup"><span data-stu-id="53b6b-152">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/cssearchsubtree.png)</span></span>  
<span data-ttu-id="53b6b-153">A rácsban jelöljön ki egy objektumot, válassza ki **tulajdonságok**, és [követve](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) hello adatforrás kapcsolódási térbe, keresztül hello metaverse és toohello cél kapcsolódási térbe.</span><span class="sxs-lookup"><span data-stu-id="53b6b-153">From this grid you can select an object, select **properties**, and [follow it](active-directory-aadconnectsync-troubleshoot-object-not-syncing.md) from hello source connector space, through hello metaverse, and toohello target connector space.</span></span>

### <a name="changing-hello-ad-ds-account-password"></a><span data-ttu-id="53b6b-154">Hello AD DS fiók jelszavának módosítása</span><span class="sxs-lookup"><span data-stu-id="53b6b-154">Changing hello AD DS account password</span></span>
<span data-ttu-id="53b6b-155">Hello fiók jelszavának módosításakor hello Synchronization Service már nem lesz képes tooimport/export módosítások tooon helyszíni AD.</span><span class="sxs-lookup"><span data-stu-id="53b6b-155">If you change hello account password, hello Synchronization Service will no longer be able tooimport/export changes tooon-premises AD.</span></span>   <span data-ttu-id="53b6b-156">Hello következő jelenhetnek meg:</span><span class="sxs-lookup"><span data-stu-id="53b6b-156">You may see hello following:</span></span>

- <span data-ttu-id="53b6b-157">hello importálási/exportálási lépést az AD-összekötő hello "no-start-hitelesítő adatok" hibaüzenettel meghiúsul.</span><span class="sxs-lookup"><span data-stu-id="53b6b-157">hello import/export step for hello AD connector fails with "no-start-credentials" error.</span></span>
- <span data-ttu-id="53b6b-158">A Windows Eseménynapló hello alkalmazások eseménynaplójában keresse meg hibát tartalmaz Event ID 6000 és az üzenet "hello felügyeleti ügynök a"contoso.com"nem sikerült toorun mert hello hitelesítő adatok érvénytelenek voltak."</span><span class="sxs-lookup"><span data-stu-id="53b6b-158">Under Windows Event Viewer, hello application event log contains an error with Event ID 6000 and message “hello management agent “contoso.com” failed toorun because hello credentials were invalid.”</span></span>

<span data-ttu-id="53b6b-159">tooresolve hello ki, a frissítés hello Active Directory tartományi szolgáltatások felhasználói fiók hello alábbi:</span><span class="sxs-lookup"><span data-stu-id="53b6b-159">tooresolve hello issue, update hello AD DS user account using hello following:</span></span>


1. <span data-ttu-id="53b6b-160">Indítsa el a Synchronization Service Managert (KEZDŐ → szinkronizálási szolgáltatás) hello.</span><span class="sxs-lookup"><span data-stu-id="53b6b-160">Start hello Synchronization Service Manager (START → Synchronization Service).</span></span>
</br><span data-ttu-id="53b6b-161">![Szinkronizálás a Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span><span class="sxs-lookup"><span data-stu-id="53b6b-161">![Sync Service Manager](./media/active-directory-aadconnectsync-service-manager-ui/startmenu.png)</span></span>
2. <span data-ttu-id="53b6b-162">Nyissa meg toohello **összekötők** fülre.</span><span class="sxs-lookup"><span data-stu-id="53b6b-162">Go toohello **Connectors** tab.</span></span>
3. <span data-ttu-id="53b6b-163">Válassza ki a hello AD-összekötő, amely konfigurált toouse hello AD DS-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="53b6b-163">Select hello AD Connector which is configured toouse hello AD DS account.</span></span>
4. <span data-ttu-id="53b6b-164">A műveletek, válassza ki a **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="53b6b-164">Under Actions, select **Properties**.</span></span>
5. <span data-ttu-id="53b6b-165">Hello előugró párbeszédpanelen válassza ki a Connect tooActive Directory-erdő:</span><span class="sxs-lookup"><span data-stu-id="53b6b-165">In hello pop-up dialog, select Connect tooActive Directory Forest:</span></span>
6. <span data-ttu-id="53b6b-166">hello erdő neve jelzi hello megfelelő helyszíni AD.</span><span class="sxs-lookup"><span data-stu-id="53b6b-166">hello Forest name indicates hello corresponding on-prem AD.</span></span>
7. <span data-ttu-id="53b6b-167">hello felhasználónév azt jelzi, hogy a szinkronizáláshoz használt hello AD DS-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="53b6b-167">hello User name indicates hello AD DS account used for synchronization.</span></span>
8. <span data-ttu-id="53b6b-168">Adjon meg új jelszót hello hello AD DS-fiókjához hello jelszó szövegmezőjét ![az Azure AD Connect Sync titkosítási kulcs segédprogram](media/active-directory-aadconnectsync-encryption-key/key6.png)</span><span class="sxs-lookup"><span data-stu-id="53b6b-168">Enter hello new password of hello AD DS account in hello Password textbox ![Azure AD Connect Sync Encryption Key Utility](media/active-directory-aadconnectsync-encryption-key/key6.png)</span></span>
9. <span data-ttu-id="53b6b-169">Kattintson az OK toosave hello új jelszót, és indítsa újra a hello szinkronizálási szolgáltatás tooremove hello régi jelszó memória-gyorsítótárból.</span><span class="sxs-lookup"><span data-stu-id="53b6b-169">Click OK toosave hello new password and restart hello Synchronization Service tooremove hello old password from memory cache.</span></span>



## <a name="next-steps"></a><span data-ttu-id="53b6b-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="53b6b-170">Next steps</span></span>
<span data-ttu-id="53b6b-171">További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="53b6b-171">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="53b6b-172">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="53b6b-172">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
