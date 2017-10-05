---
title: "Az Azure AD Connect ismételt futtatásával telepítse a varázsló |} Microsoft Docs"
description: "Ismerteti a telepítési varázsló működése a második futtatásakor."
keywords: "Az Azure AD Connect telepítővarázsló lehetővé teszi, hogy konfigurálja a karbantartási beállításait, a második futtatásakor"
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
ms.openlocfilehash: 42855b785c0ab334e33a622c8db912ce2438c627
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-running-the-installation-wizard-a-second-time"></a><span data-ttu-id="af82b-104">Azure AD Connect szinkronizálása: még egyszer a telepítővarázsló futtatása</span><span class="sxs-lookup"><span data-stu-id="af82b-104">Azure AD Connect sync: Running the installation wizard a second time</span></span>
<span data-ttu-id="af82b-105">Az Azure AD Connect telepítővarázsló első futtatásakor az végigvezeti a telepítés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="af82b-105">The first time you run the Azure AD Connect installation wizard, it walks you through how to configure your installation.</span></span> <span data-ttu-id="af82b-106">Ha újra futtatni a telepítési varázsló, karbantartási lehetőségeinek kínál.</span><span class="sxs-lookup"><span data-stu-id="af82b-106">If you run the installation wizard again, it offers options for maintenance.</span></span>

<span data-ttu-id="af82b-107">A start menü nevű megtalálja a telepítési varázsló **az Azure AD Connect**.</span><span class="sxs-lookup"><span data-stu-id="af82b-107">You can find the installation wizard in the start menu named **Azure AD Connect**.</span></span>

![Start menü](./media/active-directory-aadconnectsync-installation-wizard/startmenu.png)

<span data-ttu-id="af82b-109">A telepítési varázsló elindításakor, beállítások lap jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="af82b-109">When you start the installation wizard, you see a page with these options:</span></span>

![További feladatok listáját tartalmazó lap](./media/active-directory-aadconnectsync-installation-wizard/additionaltasks.png)

<span data-ttu-id="af82b-111">Ha már telepítette az AD FS az Azure AD Connect, még akkor is több lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="af82b-111">If you have installed ADFS with Azure AD Connect, you have even more options.</span></span> <span data-ttu-id="af82b-112">A további beállításait az AD FS ismertetett [az AD FS felügyeleti](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="af82b-112">The additional options you have for ADFS are documented in [ADFS management](active-directory-aadconnect-federation-management.md#manage-ad-fs).</span></span>

<span data-ttu-id="af82b-113">Válassza ki a feladatok közül, és kattintson a **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="af82b-113">Select one of the tasks and click **Next** to continue.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="af82b-114">Miközben a telepítési varázsló megnyitása, a szinkronizálási motor az összes művelet fel vannak függesztve.</span><span class="sxs-lookup"><span data-stu-id="af82b-114">While you have the installation wizard open, all operations in the sync engine are suspended.</span></span> <span data-ttu-id="af82b-115">Győződjön meg arról, amint a konfigurációs módosítások végrehajtása zárja be a telepítővarázslót.</span><span class="sxs-lookup"><span data-stu-id="af82b-115">Make sure you close the installation wizard as soon as you have completed your configuration changes.</span></span>
>
>

## <a name="view-current-configuration"></a><span data-ttu-id="af82b-116">Aktuális konfiguráció megtekintése</span><span class="sxs-lookup"><span data-stu-id="af82b-116">View current configuration</span></span>
<span data-ttu-id="af82b-117">Ez a beállítás lehetővé teszi a jelenleg konfigurált beállítások gyors áttekintést.</span><span class="sxs-lookup"><span data-stu-id="af82b-117">This option gives you a quick view of your currently configured options.</span></span>

![Az összes beállítás és állapotukra listáját lap](./media/active-directory-aadconnectsync-installation-wizard/viewconfig.png)

<span data-ttu-id="af82b-119">Kattintson a **előző** térhet vissza.</span><span class="sxs-lookup"><span data-stu-id="af82b-119">Click **Previous** to go back.</span></span> <span data-ttu-id="af82b-120">Ha **kilépési**, akkor zárja be a telepítővarázslót.</span><span class="sxs-lookup"><span data-stu-id="af82b-120">If you select **Exit**, you close the installation wizard.</span></span>

## <a name="customize-synchronization-options"></a><span data-ttu-id="af82b-121">Szinkronizálási beállítások testreszabása</span><span class="sxs-lookup"><span data-stu-id="af82b-121">Customize synchronization options</span></span>
<span data-ttu-id="af82b-122">Ezzel a beállítással a szinkronizálási konfigurációs módosításokat.</span><span class="sxs-lookup"><span data-stu-id="af82b-122">This option is used to make changes to the sync configuration.</span></span> <span data-ttu-id="af82b-123">Egyéni konfiguráció telepítési elérési úton található beállítások egy részét látja.</span><span class="sxs-lookup"><span data-stu-id="af82b-123">You see a subset of options from the custom configuration installation path.</span></span> <span data-ttu-id="af82b-124">Látja ezt a beállítást, akkor is, ha az Expressz telepítés eredetileg használt.</span><span class="sxs-lookup"><span data-stu-id="af82b-124">You see this option even if you used express installation initially.</span></span>

* <span data-ttu-id="af82b-125">[Adja hozzá a további címtárakat](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span><span class="sxs-lookup"><span data-stu-id="af82b-125">[Add more directories](active-directory-aadconnect-get-started-custom.md#connect-your-directories).</span></span> <span data-ttu-id="af82b-126">A könyvtár törlése, lásd: [összekötő törlése](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span><span class="sxs-lookup"><span data-stu-id="af82b-126">For removing a directory, see [Delete a Connector](active-directory-aadconnectsync-service-manager-ui-connectors.md#delete).</span></span>
* <span data-ttu-id="af82b-127">[Módosítsa a tartomány és szervezeti egységek szűrése](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span><span class="sxs-lookup"><span data-stu-id="af82b-127">[Change Domain and OU filtering](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering).</span></span>
* <span data-ttu-id="af82b-128">Szűrés csoport eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="af82b-128">Remove Group filtering.</span></span>
* <span data-ttu-id="af82b-129">[Választható szolgáltatások módosítása](active-directory-aadconnect-get-started-custom.md#optional-features).</span><span class="sxs-lookup"><span data-stu-id="af82b-129">[Change optional features](active-directory-aadconnect-get-started-custom.md#optional-features).</span></span>

<span data-ttu-id="af82b-130">A többi beállítás a kezdeti telepítés nem módosítható, és nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="af82b-130">The other options from the initial installation cannot be changed and are not available.</span></span> <span data-ttu-id="af82b-131">Ezek a beállítások a következők:</span><span class="sxs-lookup"><span data-stu-id="af82b-131">These options are:</span></span>

* <span data-ttu-id="af82b-132">Módosítsa a userPrincipalName és a sourceAnchor attribútum.</span><span class="sxs-lookup"><span data-stu-id="af82b-132">Change the attribute to use for userPrincipalName and sourceAnchor.</span></span>
* <span data-ttu-id="af82b-133">Másik erdőből származó objektumok csatlakozó módjának módosítása.</span><span class="sxs-lookup"><span data-stu-id="af82b-133">Change the joining method for objects from different forest.</span></span>
* <span data-ttu-id="af82b-134">Csoport-alapú szűrés engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="af82b-134">Enable group-based filtering.</span></span>

## <a name="refresh-directory-schema"></a><span data-ttu-id="af82b-135">Directory-séma frissítése</span><span class="sxs-lookup"><span data-stu-id="af82b-135">Refresh directory schema</span></span>
<span data-ttu-id="af82b-136">Ez a beállítás használható, ha a séma módosította az egyik a helyszíni AD DS-erdőkből.</span><span class="sxs-lookup"><span data-stu-id="af82b-136">This option is used if you have changed the schema in one of your on-premises AD DS forests.</span></span> <span data-ttu-id="af82b-137">Például előfordulhat, hogy telepítette az Exchange vagy a Windows Server 2012 sémára eszköz objektumok frissítése.</span><span class="sxs-lookup"><span data-stu-id="af82b-137">For example, you might have installed Exchange or upgraded to a Windows Server 2012 schema with device objects.</span></span> <span data-ttu-id="af82b-138">Ebben az esetben szüksége arra utasítani az Azure AD Connect újból beolvasni a sémát az AD DS-ből, és frissíti a gyorsítótárat.</span><span class="sxs-lookup"><span data-stu-id="af82b-138">In this case, you need to instruct Azure AD Connect to read the schema again from AD DS and update its cache.</span></span> <span data-ttu-id="af82b-139">Ezzel a művelettel újragenerálja a szinkronizálási szabályok is.</span><span class="sxs-lookup"><span data-stu-id="af82b-139">This action also regenerates the Sync Rules.</span></span> <span data-ttu-id="af82b-140">Ha például az Exchange-séma ad hozzá, a konfigurációs kerülnek Exchange szinkronizálási szabályait.</span><span class="sxs-lookup"><span data-stu-id="af82b-140">If you add the Exchange schema, as an example, the Sync Rules for Exchange are added to the configuration.</span></span>

<span data-ttu-id="af82b-141">Ha ezt a lehetőséget választja, a konfigurációt a könyvtárak jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="af82b-141">When you select this option, all the directories in your configuration are listed.</span></span> <span data-ttu-id="af82b-142">Megtartani az alapértelmezett beállítást, és frissítse az összes erdőben, és némelyikük törölje.</span><span class="sxs-lookup"><span data-stu-id="af82b-142">You can keep the default setting and refresh all forests or unselect some of them.</span></span>

![A környezet összes könyvtárainak listáját tartalmazó lap](./media/active-directory-aadconnectsync-installation-wizard/refreshschema.png)

## <a name="configure-staging-mode"></a><span data-ttu-id="af82b-144">Átmeneti környezetű üzemmód konfigurálása</span><span class="sxs-lookup"><span data-stu-id="af82b-144">Configure staging mode</span></span>
<span data-ttu-id="af82b-145">Ezzel a beállítással engedélyezheti vagy letilthatja az átmeneti környezetű üzemmód a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="af82b-145">This option allows you to enable and disable staging mode on the server.</span></span> <span data-ttu-id="af82b-146">További információt az átmeneti módot, és hogyan használja fel azokat a található [műveletek](active-directory-aadconnectsync-operations.md#staging-mode).</span><span class="sxs-lookup"><span data-stu-id="af82b-146">More information about staging mode and how it is used can be found in [Operations](active-directory-aadconnectsync-operations.md#staging-mode).</span></span>

<span data-ttu-id="af82b-147">A beállítás jeleníti meg, ha átmeneti jelenleg engedélyezve vagy letiltva:</span><span class="sxs-lookup"><span data-stu-id="af82b-147">The option shows if staging is currently enabled or disabled:</span></span>  
![Lehetőség, amely a rendszer azt is jelzi az átmeneti környezetű üzemmód jelenlegi állapotában](./media/active-directory-aadconnectsync-installation-wizard/stagingmodecurrentstate.png)

<span data-ttu-id="af82b-149">Az állapot módosítására irányuló ezt a beállítást, és válassza ki, vagy törölje a jelölőnégyzet jelölését.</span><span class="sxs-lookup"><span data-stu-id="af82b-149">To change the state, select this option and select or unselect the checkbox.</span></span>  
![Lehetőség, amely a rendszer azt is jelzi az átmeneti környezetű üzemmód jelenlegi állapotában](./media/active-directory-aadconnectsync-installation-wizard/stagingmodeenable.png)

## <a name="change-user-sign-in"></a><span data-ttu-id="af82b-151">Felhasználói bejelentkezés módosítása</span><span class="sxs-lookup"><span data-stu-id="af82b-151">Change user sign-in</span></span>
<span data-ttu-id="af82b-152">Ez a beállítás lehetővé teszi, hogy a jelszó-szinkronizálás összevonási vagy megfordítva már módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="af82b-152">This option allows you to change from password sync to federation or the other way around.</span></span> <span data-ttu-id="af82b-153">Nem lehet átváltani **ne konfiguráljon**.</span><span class="sxs-lookup"><span data-stu-id="af82b-153">You cannot change to **do not configure**.</span></span>

<span data-ttu-id="af82b-154">Ez a beállítás további információkért lásd: [felhasználói bejelentkezés](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span><span class="sxs-lookup"><span data-stu-id="af82b-154">For more information on this option, see [user sign-in](active-directory-aadconnect-user-signin.md#changing-the-user-sign-in-method).</span></span>

## <a name="next-steps"></a><span data-ttu-id="af82b-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="af82b-155">Next steps</span></span>
* <span data-ttu-id="af82b-156">További tudnivalók az Azure AD Connect szinkronizálási által alkalmazott konfigurációs modell [ismertetése deklaratív kiépítés](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span><span class="sxs-lookup"><span data-stu-id="af82b-156">Learn more about the configuration model used by Azure AD Connect sync in [Understanding Declarative Provisioning](active-directory-aadconnectsync-understanding-declarative-provisioning.md).</span></span>

<span data-ttu-id="af82b-157">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="af82b-157">**Overview topics**</span></span>

* [<span data-ttu-id="af82b-158">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="af82b-158">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)
* [<span data-ttu-id="af82b-159">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="af82b-159">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
