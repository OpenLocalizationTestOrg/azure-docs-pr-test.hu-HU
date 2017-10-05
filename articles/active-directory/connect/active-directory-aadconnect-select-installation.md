---
title: "Az Azure AD Connect: A telepítés típusának kiválasztása |} Microsoft Docs"
description: "Ez a témakör végigvezeti az Azure AD Connect használatával a telepítés típusának kiválasztása"
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: a5697686bd1f41d581554b27ce78897963e38c74
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="select-which-installation-type-to-use-for-azure-ad-connect"></a><span data-ttu-id="c44fe-103">Jelölje ki a használatára az Azure AD Connect telepítési típusát</span><span class="sxs-lookup"><span data-stu-id="c44fe-103">Select which installation type to use for Azure AD Connect</span></span>
<span data-ttu-id="c44fe-104">Az Azure AD Connect két tartozik telepítése új telepítés: Express és testre szabható.</span><span class="sxs-lookup"><span data-stu-id="c44fe-104">Azure AD Connect has two installation types for new installation: Express and customized.</span></span> <span data-ttu-id="c44fe-105">A témakörben leírtak segítségével eldöntheti, hogy a telepítés során használandó módszer.</span><span class="sxs-lookup"><span data-stu-id="c44fe-105">This topic helps you to decide which option to use during installation.</span></span>

## <a name="express"></a><span data-ttu-id="c44fe-106">Express</span><span class="sxs-lookup"><span data-stu-id="c44fe-106">Express</span></span>
<span data-ttu-id="c44fe-107">Express a leggyakrabban használt beállítás, és minden új telepítések körülbelül 90 %-át használja.</span><span class="sxs-lookup"><span data-stu-id="c44fe-107">Express is the most common option and is used by about 90% of all new installations.</span></span> <span data-ttu-id="c44fe-108">Adja meg a konfigurációt a leggyakoribb forgatókönyvet készült.</span><span class="sxs-lookup"><span data-stu-id="c44fe-108">It was designed to provide a configuration that works for the most common customer scenarios.</span></span>

<span data-ttu-id="c44fe-109">Ez azt feltételezi, hogy:</span><span class="sxs-lookup"><span data-stu-id="c44fe-109">It assumes:</span></span>

- <span data-ttu-id="c44fe-110">A helyi erdő egy egyetlen Active Directory rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c44fe-110">You have a single Active Directory forest on-premises.</span></span>
- <span data-ttu-id="c44fe-111">A telepítéshez használható vállalati rendszergazdai fiókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c44fe-111">You have an enterprise administrator account you can use for the installation.</span></span>
- <span data-ttu-id="c44fe-112">A helyszíni Active Directoryban kisebb, mint 100 000 objektummal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c44fe-112">You have less than 100,000 objects in your on-premises Active Directory.</span></span>

<span data-ttu-id="c44fe-113">Elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="c44fe-113">You get:</span></span>

- <span data-ttu-id="c44fe-114">[A jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) az egyszeri bejelentkezés az Azure AD helyszíni.</span><span class="sxs-lookup"><span data-stu-id="c44fe-114">[Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) from on-premises to Azure AD for single sign-on.</span></span>
- <span data-ttu-id="c44fe-115">A konfiguráció, amely szinkronizálja [felhasználók, csoportok, névjegyek és Windows 10-es számítógépeken](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="c44fe-115">A configuration that synchronizes [users, groups, contacts, and Windows 10 computers](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
- <span data-ttu-id="c44fe-116">Az összes tartomány és az összes szervezeti egység összes jogosult objektumok szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="c44fe-116">Synchronization of all eligible objects in all domains and all OUs.</span></span>
- <span data-ttu-id="c44fe-117">[Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) engedélyezve van-e győződjön meg arról, hogy mindig használja az elérhető legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="c44fe-117">[Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is enabled to make sure you always use the latest available version.</span></span>

<span data-ttu-id="c44fe-118">Ha továbbra is használhatja az expressz beállításokat:</span><span class="sxs-lookup"><span data-stu-id="c44fe-118">Options where you can still use Express:</span></span>

- <span data-ttu-id="c44fe-119">Ha nem szeretné szinkronizálni az összes szervezeti egységek, is Express használja, és az utolsó oldalon törölje **a szinkronizálási folyamat indítása...** *.</span><span class="sxs-lookup"><span data-stu-id="c44fe-119">If you do not want to synchronize all OUs, you can still use Express and on the last page, unselect **Start the synchronization process...***.</span></span> <span data-ttu-id="c44fe-120">Ezután futtassa újból a telepítési varázsló, és módosítsa a szervezeti egységek az [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) és ütemezett szinkronizálás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c44fe-120">Then run the installation wizard again and change the OUs in [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) and enable scheduled sync.</span></span>
- <span data-ttu-id="c44fe-121">Például a jelszóvisszaírást az Azure AD Premium szolgáltatásainak egyike a engedélyezni szeretné.</span><span class="sxs-lookup"><span data-stu-id="c44fe-121">You want to enable one of the features in Azure AD Premium, such as Password writeback.</span></span> <span data-ttu-id="c44fe-122">Először halad át express lekérni a kezdeti telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="c44fe-122">First go through express to get the initial installation completed.</span></span> <span data-ttu-id="c44fe-123">Ezután futtassa újból a telepítési varázsló, és módosítsa a [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span><span class="sxs-lookup"><span data-stu-id="c44fe-123">Then run the installation wizard again and change the [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span></span>

## <a name="custom"></a><span data-ttu-id="c44fe-124">Egyéni</span><span class="sxs-lookup"><span data-stu-id="c44fe-124">Custom</span></span>
<span data-ttu-id="c44fe-125">Az egyéni elérési út lehetővé teszi, hogy számos további lehetőség express-nál.</span><span class="sxs-lookup"><span data-stu-id="c44fe-125">The customized path allows many more options than express.</span></span> <span data-ttu-id="c44fe-126">Minden olyan esetben, ha expressz az előző szakaszban leírt konfiguráció nincs a szervezet reprezentatív használandó.</span><span class="sxs-lookup"><span data-stu-id="c44fe-126">It should be used in all cases where the configuration described in previous section for express is not representative for your organization.</span></span>

<span data-ttu-id="c44fe-127">A következő esetekben használja:</span><span class="sxs-lookup"><span data-stu-id="c44fe-127">Use when:</span></span>

- <span data-ttu-id="c44fe-128">Az Active Directory nincs egy vállalati rendszergazdai fiók eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c44fe-128">You do not have access to an enterprise admin account in Active Directory.</span></span>
- <span data-ttu-id="c44fe-129">Több erdővel rendelkezik, vagy egynél több erdő a jövőben szinkronizálni szeretne.</span><span class="sxs-lookup"><span data-stu-id="c44fe-129">You have more than one forest or you plan to synchronize more than one forest in the future.</span></span>
- <span data-ttu-id="c44fe-130">Az erdő a Connect-kiszolgáló nem elérhető tartományok vannak.</span><span class="sxs-lookup"><span data-stu-id="c44fe-130">You have domains in your forest not reachable from the Connect server.</span></span>
- <span data-ttu-id="c44fe-131">A felhasználói bejelentkezés összevonási vagy átmenő hitelesítés használatát tervezi.</span><span class="sxs-lookup"><span data-stu-id="c44fe-131">You plan to use federation or pass-through authentication for user sign-in.</span></span>
- <span data-ttu-id="c44fe-132">Több mint 100 000 objektummal rendelkezik, és szeretné használni, a teljes SQL-kiszolgálóról.</span><span class="sxs-lookup"><span data-stu-id="c44fe-132">You have more than 100,000 objects and need to use a full SQL Server.</span></span>
- <span data-ttu-id="c44fe-133">Biztonságicsoport-alapú szűrés és nem csak tartomány vagy szervezeti egység alapú szűrés használatát tervezi.</span><span class="sxs-lookup"><span data-stu-id="c44fe-133">You plan to use group-based filtering and not only domain or OU-based filtering.</span></span>

## <a name="upgrade-from-dirsync"></a><span data-ttu-id="c44fe-134">Frissítés a DirSync szolgáltatásról</span><span class="sxs-lookup"><span data-stu-id="c44fe-134">Upgrade from DirSync</span></span>
<span data-ttu-id="c44fe-135">Ha a DirSync jelenleg használ, majd kövesse a [frissítésre a Dirsyncről](active-directory-aadconnect-dirsync-upgrade-get-started.md) frissítésére a meglévő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="c44fe-135">If you are currently using DirSync, then follow the steps in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) to upgrade your existing configuration.</span></span> <span data-ttu-id="c44fe-136">Két különböző frissítési lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="c44fe-136">There are two different upgrade options:</span></span>

- <span data-ttu-id="c44fe-137">Frissítés helyben Connect telepíthető ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="c44fe-137">In-place upgrade to install Connect on the same server.</span></span>
- <span data-ttu-id="c44fe-138">Párhuzamos üzembe helyezés a Connect telepítése egy új kiszolgálóra, míg a DirSync meglévő kiszolgáló továbbra is működik.</span><span class="sxs-lookup"><span data-stu-id="c44fe-138">Parallel deployment to install Connect on a new server while the existing DirSync server is still operational.</span></span>

## <a name="upgrade-from-azure-ad-sync"></a><span data-ttu-id="c44fe-139">Frissítés Azure AD-szinkronizálóról</span><span class="sxs-lookup"><span data-stu-id="c44fe-139">Upgrade from Azure AD Sync</span></span>
<span data-ttu-id="c44fe-140">Ha a jelenleg használt Azure AD Sync, majd kövesse a [ugyanazokat a lépéseket](active-directory-aadconnect-upgrade-previous-version.md) , amikor egy csatlakozás verziója rendszerről frissít egy újabb.</span><span class="sxs-lookup"><span data-stu-id="c44fe-140">If you are currently using Azure AD Sync, then you can follow the [same steps](active-directory-aadconnect-upgrade-previous-version.md) as when you upgrade from one Connect version to a newer.</span></span> <span data-ttu-id="c44fe-141">Két különböző frissítési lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="c44fe-141">There are two different upgrade options:</span></span>

- <span data-ttu-id="c44fe-142">Frissítés helyben Connect telepíthető ugyanarra a kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="c44fe-142">In-place upgrade to install Connect on the same server.</span></span>
- <span data-ttu-id="c44fe-143">Mozgó-áttelepítési Connect új kiszolgáló telepítése során a meglévő Azure AD Sync-kiszolgáló egy továbbra is működik.</span><span class="sxs-lookup"><span data-stu-id="c44fe-143">Swing-migration to install Connect on a new server while the existing Azure AD Sync server is still operational.</span></span>

## <a name="migrate-from-fim2010-or-mim2016"></a><span data-ttu-id="c44fe-144">FIM2010 vagy MIM2016 áttelepítése</span><span class="sxs-lookup"><span data-stu-id="c44fe-144">Migrate from FIM2010 or MIM2016</span></span>
<span data-ttu-id="c44fe-145">Ha a jelenleg használt Forefront Identity Manager 2010 vagy a Microsoft Identity Manager 2016 az Azure AD-összekötő használatával, majd egyetlen választása marad: áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="c44fe-145">If you are currently using Forefront Identity Manager 2010 or Microsoft Identity Manager 2016 with the Azure AD Connector, then your only option is a migration.</span></span> <span data-ttu-id="c44fe-146">Az ismertetett lépéseket követve [mozgó-áttelepítési](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="c44fe-146">Follow the steps described in [swing-migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span> <span data-ttu-id="c44fe-147">A lépéseket cserélje le az Azure AD Sync bármely hashtagként FIM2010/MIM2016.</span><span class="sxs-lookup"><span data-stu-id="c44fe-147">In the steps, replace any mention of Azure AD Sync with FIM2010/MIM2016.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c44fe-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c44fe-148">Next steps</span></span>
<span data-ttu-id="c44fe-149">Attól függően, hogy a kijelölt használandó beállítás a tartalom a bal oldali tábla használatával a részletes lépéseket a cikk megtalálását.</span><span class="sxs-lookup"><span data-stu-id="c44fe-149">Depending on the option you have selected to use, use the table of content to the left to find your article with the detailed steps.</span></span>
