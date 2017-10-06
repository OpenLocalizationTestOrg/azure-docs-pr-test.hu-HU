---
title: "Ismételt futtatásával hello Azure AD Connect varázsló |} Microsoft Docs"
description: "Ismerteti, hello telepítési varázsló működése hello még egyszer kell futtatnia."
keywords: "hello Azure AD Connect telepítővarázsló lehetővé teszi a karbantartási beállítások hello konfigurálása még egyszer kell futtatnia"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: d800214e-e591-4297-b9b5-d0b1581cc36a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 83cc74aca471ef9b4f65f7f3582e3e48d3d81cfe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-running-hello-installation-wizard-a-second-time"></a><span data-ttu-id="924f2-104">Azure AD Connect szinkronizálása: hello telepítővarázsló futtatása még egyszer</span><span class="sxs-lookup"><span data-stu-id="924f2-104">Azure AD Connect sync: Running hello installation wizard a second time</span></span>
<span data-ttu-id="924f2-105">hello hello Azure AD Connect telepítővarázsló első futtatásakor az végigvezeti tooconfigure a telepítést.</span><span class="sxs-lookup"><span data-stu-id="924f2-105">hello first time you run hello Azure AD Connect installation wizard, it walks you through how tooconfigure your installation.</span></span> <span data-ttu-id="924f2-106">Ha újra hello telepítési varázsló, karbantartási lehetőségeinek kínál.</span><span class="sxs-lookup"><span data-stu-id="924f2-106">If you run hello installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="924f2-107">Hello telepítővarázsló nevű hello start menüben található **az Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="924f2-107">You can find hello installation wizard in hello start menu named **Azure AD Connect**.</span></span>

![Start menü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="924f2-109">Hello telepítővarázsló indításakor, beállítások lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="924f2-109">When you start hello installation wizard, you see a page with these options:</span></span>

![További feladatok listáját tartalmazó lap](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="924f2-111">Ha már telepítette az AD FS az Azure AD Connect, még akkor is több lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="924f2-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="924f2-112">az AD FS ismertetett rendelkezik további beállítások hello [az AD FS felügyeleti](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="924f2-112">hello additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="924f2-113">Válassza ki valamelyik hello feladatot, és kattintson a **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="924f2-113">Select one of hello tasks and click **Next** toocontinue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="924f2-114">Miközben hello telepítési varázsló megnyitása, a szinkronizálási motor hello szereplő összes művelet fel vannak függesztve.</span><span class="sxs-lookup"><span data-stu-id="924f2-114">While you have hello installation wizard open, all operations in hello sync engine are suspended.</span></span> <span data-ttu-id="924f2-115">Ellenőrizze, hogy hello telepítővarázsló bezárása, amint befejezte a konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="924f2-115">Make sure you close hello installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="924f2-116">Aktuális konfiguráció megtekintése</span><span class="sxs-lookup"><span data-stu-id="924f2-116">View current configuration</span></span>
<span data-ttu-id="924f2-117">Ez a beállítás lehetővé teszi a jelenleg konfigurált beállítások gyors áttekintést.</span><span class="sxs-lookup"><span data-stu-id="924f2-117">This option gives you a quick view of your currently configured options.</span></span>

![Az összes beállítás és állapotukra listáját lap](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="924f2-119">Kattintson a **előző** toogo vissza.</span><span class="sxs-lookup"><span data-stu-id="924f2-119">Click **Previous** toogo back.</span></span> <span data-ttu-id="924f2-120">Ha **kilépési**, hello telepítési varázsló bezárásához.</span><span class="sxs-lookup"><span data-stu-id="924f2-120">If you select **Exit**, you close hello installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="924f2-121">Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="924f2-121">Customize synchronization options</span></span>
<span data-ttu-id="924f2-122">Ez a beállítás akkor használt toomake módosítások toohello sync konfigurációja.</span><span class="sxs-lookup"><span data-stu-id="924f2-122">This option is used toomake changes toohello sync configuration.</span></span> <span data-ttu-id="924f2-123">Megjelenik egy részhalmazát hello egyéni konfigurációs telepítési elérési utat a beállításai.</span><span class="sxs-lookup"><span data-stu-id="924f2-123">You see a subset of options from hello custom configuration installation path.</span></span> <span data-ttu-id="924f2-124">Látja ezt a beállítást, akkor is, ha az Expressz telepítés eredetileg használt.</span><span class="sxs-lookup"><span data-stu-id="924f2-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="924f2-125">[Adja hozzá a további címtárakat](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="924f2-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="924f2-126">A könyvtár törlése, lásd: [összekötő törlése](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="924f2-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="924f2-127">[Módosítsa a tartomány és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="924f2-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="924f2-128">Szűrés csoport eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="924f2-128">Remove Group filtering.</span></span>
* <span data-ttu-id="924f2-129">[Választható szolgáltatások módosítása](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="924f2-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="924f2-130">hello egyéb beállítások és a kezdeti telepítés hello nem módosítható és nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="924f2-130">hello other options from hello initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="924f2-131">Ezek a beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="924f2-131">These options are:</span></span>

* <span data-ttu-id="924f2-132">A userPrincipalName és sourceAnchor hello attribútum toouse módosítása.</span><span class="sxs-lookup"><span data-stu-id="924f2-132">Change hello attribute toouse for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="924f2-133">Csatlakozás másik erdőből származó objektumok metódus hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="924f2-133">Change hello joining method for objects from different forest.</span></span>
* <span data-ttu-id="924f2-134">Csoport-alapú szűrés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="924f2-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="924f2-135">Directory-séma frissítése</span><span class="sxs-lookup"><span data-stu-id="924f2-135">Refresh directory schema</span></span>
<span data-ttu-id="924f2-136">Ez a beállítás használható, ha hello séma módosította az egyik a helyszíni AD DS-erdőkből.</span><span class="sxs-lookup"><span data-stu-id="924f2-136">This option is used if you have changed hello schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="924f2-137">Például előfordulhat, hogy telepített Exchange, vagy frissített Windows Server 2012 tooa séma eszköz objektummal.</span><span class="sxs-lookup"><span data-stu-id="924f2-137">For example, you might have installed Exchange or upgraded tooa Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="924f2-138">Ebben az esetben kell tooinstruct az Azure AD Connect tooread hello séma újra az Active Directory tartományi Szolgáltatásokban, és frissítse gyorsítótárát.</span><span class="sxs-lookup"><span data-stu-id="924f2-138">In this case, you need tooinstruct Azure AD Connect tooread hello schema again from AD DS and update its cache.</span></span> <span data-ttu-id="924f2-139">Ezzel a művelettel újragenerálja a hello szinkronizálási szabályokat is.</span><span class="sxs-lookup"><span data-stu-id="924f2-139">This action also regenerates hello Sync Rules.</span></span> <span data-ttu-id="924f2-140">Példa felvételekor hello Exchange-séma, az Exchange hello szinkronizálási szabályok hozzáadása történik meg toohello konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="924f2-140">If you add hello Exchange schema, as an example, hello Sync Rules for Exchange are added toohello configuration.</span></span>

<span data-ttu-id="924f2-141">Ha ezt a lehetőséget választja, a konfiguráció összes hello könyvtárak jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="924f2-141">When you select this option, all hello directories in your configuration are listed.</span></span> <span data-ttu-id="924f2-142">Hello alapértelmezett megtartásához és frissítse az összes erdőben, és némelyikük törölje.</span><span class="sxs-lookup"><span data-stu-id="924f2-142">You can keep hello default setting and refresh all forests or unselect some of them.</span></span>

![Az összes könyvtár hello környezetben listáját lap](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="924f2-144">Átmeneti környezetű üzemmód konfigurálása</span><span class="sxs-lookup"><span data-stu-id="924f2-144">Configure staging mode</span></span>
<span data-ttu-id="924f2-145">Ez a beállítás lehetővé teszi tooenable, és tiltsa le az átmeneti környezetű üzemmód hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="924f2-145">This option allows you tooenable and disable staging mode on hello server.</span></span> <span data-ttu-id="924f2-146">További információt az átmeneti módot, és hogyan használja fel azokat a található [műveletek](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="924f2-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="924f2-147">hello beállítást jeleníti meg, ha átmeneti jelenleg engedélyezve vagy letiltva:</span><span class="sxs-lookup"><span data-stu-id="924f2-147">hello option shows if staging is currently enabled or disabled:</span></span>  
![Lehetőség, amely is láthatók az átmeneti környezetű üzemmód hello aktuális állapota](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="924f2-149">toochange hello állapotát, jelölje be ezt a beállítást és válassza ki, vagy visszavonása hello jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="924f2-149">toochange hello state, select this option and select or unselect hello checkbox.</span></span>  
![Lehetőség, amely is láthatók az átmeneti környezetű üzemmód hello aktuális állapota](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="924f2-151">Felhasználói bejelentkezés módosítása</span><span class="sxs-lookup"><span data-stu-id="924f2-151">Change user sign-in</span></span>
<span data-ttu-id="924f2-152">Ez a beállítás lehetővé teszi a jelszó-szinkronizálás toofederation vagy megfordítva már hello toochange.</span><span class="sxs-lookup"><span data-stu-id="924f2-152">This option allows you toochange from password sync toofederation or hello other way around.</span></span> <span data-ttu-id="924f2-153">Nem módosítható túl**ne konfiguráljon**.</span><span class="sxs-lookup"><span data-stu-id="924f2-153">You cannot change too**do not configure**.</span></span>

<span data-ttu-id="924f2-154">Ez a beállítás további információkért lásd: [felhasználói bejelentkezés](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="924f2-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="924f2-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="924f2-155">Next steps</span></span>
* <span data-ttu-id="924f2-156">További tudnivalók az Azure AD Connect szinkronizálási által használt hello konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="924f2-156">Learn more about hello configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="924f2-157">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="924f2-157">**Overview topics**</span></span>

* [<span data-ttu-id="924f2-158">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="924f2-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="924f2-159">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="924f2-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
