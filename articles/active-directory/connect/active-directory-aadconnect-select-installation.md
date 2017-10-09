---
title: "Az Azure AD Connect: A telepítés típusának kiválasztása |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan hogyan tooselect hello telepítési adja meg az Azure AD Connect toouse"
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
ms.openlocfilehash: a700e59eb05947ee1dbd9993141200c9340b2f1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="select-which-installation-type-toouse-for-azure-ad-connect"></a><span data-ttu-id="4d518-103">Az Azure AD Connect melyik telepítési típus toouse kiválasztása</span><span class="sxs-lookup"><span data-stu-id="4d518-103">Select which installation type toouse for Azure AD Connect</span></span>
<span data-ttu-id="4d518-104">Az Azure AD Connect két tartozik telepítése új telepítés: Express és testre szabható.</span><span class="sxs-lookup"><span data-stu-id="4d518-104">Azure AD Connect has two installation types for new installation: Express and customized.</span></span> <span data-ttu-id="4d518-105">Ez a témakör segít toodecide, amely toouse lehetőség a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="4d518-105">This topic helps you toodecide which option toouse during installation.</span></span>

## <a name="express"></a><span data-ttu-id="4d518-106">Express</span><span class="sxs-lookup"><span data-stu-id="4d518-106">Express</span></span>
<span data-ttu-id="4d518-107">Express hello leggyakrabban használt beállítás, és minden új telepítések körülbelül 90 %-át használja.</span><span class="sxs-lookup"><span data-stu-id="4d518-107">Express is hello most common option and is used by about 90% of all new installations.</span></span> <span data-ttu-id="4d518-108">Tervezett tooprovide hello leggyakoribb forgatókönyvet a megfelelő konfigurációt volt.</span><span class="sxs-lookup"><span data-stu-id="4d518-108">It was designed tooprovide a configuration that works for hello most common customer scenarios.</span></span>

<span data-ttu-id="4d518-109">Ez azt feltételezi, hogy:</span><span class="sxs-lookup"><span data-stu-id="4d518-109">It assumes:</span></span>

- <span data-ttu-id="4d518-110">A helyi erdő egy egyetlen Active Directory rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4d518-110">You have a single Active Directory forest on-premises.</span></span>
- <span data-ttu-id="4d518-111">Hello telepítéséhez használhatja a vállalati rendszergazdai fiókkal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4d518-111">You have an enterprise administrator account you can use for hello installation.</span></span>
- <span data-ttu-id="4d518-112">A helyszíni Active Directoryban kisebb, mint 100 000 objektummal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4d518-112">You have less than 100,000 objects in your on-premises Active Directory.</span></span>

<span data-ttu-id="4d518-113">Elérhetővé:</span><span class="sxs-lookup"><span data-stu-id="4d518-113">You get:</span></span>

- <span data-ttu-id="4d518-114">[A jelszó-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) a helyszíni tooAzure AD az egyszeri bejelentkezést.</span><span class="sxs-lookup"><span data-stu-id="4d518-114">[Password synchronization](active-directory-aadconnectsync-implement-password-synchronization.md) from on-premises tooAzure AD for single sign-on.</span></span>
- <span data-ttu-id="4d518-115">A konfiguráció, amely szinkronizálja [felhasználók, csoportok, névjegyek és Windows 10-es számítógépeken](active-directory-aadconnectsync-understanding-default-configuration.md).</span><span class="sxs-lookup"><span data-stu-id="4d518-115">A configuration that synchronizes [users, groups, contacts, and Windows 10 computers](active-directory-aadconnectsync-understanding-default-configuration.md).</span></span>
- <span data-ttu-id="4d518-116">Az összes tartomány és az összes szervezeti egység összes jogosult objektumok szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="4d518-116">Synchronization of all eligible objects in all domains and all OUs.</span></span>
- <span data-ttu-id="4d518-117">[Automatikus frissítés](active-directory-aadconnect-feature-automatic-upgrade.md) van engedélyezett toomake mindig kell használnia, hello elérhető legújabb verzióra.</span><span class="sxs-lookup"><span data-stu-id="4d518-117">[Automatic upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) is enabled toomake sure you always use hello latest available version.</span></span>

<span data-ttu-id="4d518-118">Ha továbbra is használhatja az expressz beállításokat:</span><span class="sxs-lookup"><span data-stu-id="4d518-118">Options where you can still use Express:</span></span>

- <span data-ttu-id="4d518-119">Ha nem szeretné, hogy toosynchronize minden hozzá, továbbra is használhat Express és hello utolsó lapján kijelölésének **hello szinkronizálási folyamat indítása...** *.</span><span class="sxs-lookup"><span data-stu-id="4d518-119">If you do not want toosynchronize all OUs, you can still use Express and on hello last page, unselect **Start hello synchronization process...***.</span></span> <span data-ttu-id="4d518-120">Ezután futtassa újra a hello telepítővarázslóját, és módosítsa a hello szervezeti egységek az [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) és ütemezett szinkronizálás engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="4d518-120">Then run hello installation wizard again and change hello OUs in [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options) and enable scheduled sync.</span></span>
- <span data-ttu-id="4d518-121">Azt szeretné, például a jelszóvisszaírást az Azure AD Premium hello funkcióinak egyike a tooenable.</span><span class="sxs-lookup"><span data-stu-id="4d518-121">You want tooenable one of hello features in Azure AD Premium, such as Password writeback.</span></span> <span data-ttu-id="4d518-122">Először halad át expressz tooget hello kezdeti telepítése befejeződött.</span><span class="sxs-lookup"><span data-stu-id="4d518-122">First go through express tooget hello initial installation completed.</span></span> <span data-ttu-id="4d518-123">Ezután futtassa újra a hello telepítővarázslóját, és módosítsa a hello [konfigurációs beállítások](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span><span class="sxs-lookup"><span data-stu-id="4d518-123">Then run hello installation wizard again and change hello [configuration options](active-directory-aadconnectsync-installation-wizard.md#customize-synchronization-options).</span></span>

## <a name="custom"></a><span data-ttu-id="4d518-124">Egyéni</span><span class="sxs-lookup"><span data-stu-id="4d518-124">Custom</span></span>
<span data-ttu-id="4d518-125">hello egyéni elérési út lehetővé teszi, hogy számos további lehetőség express-nál.</span><span class="sxs-lookup"><span data-stu-id="4d518-125">hello customized path allows many more options than express.</span></span> <span data-ttu-id="4d518-126">Minden olyan esetben, ahol az express előző szakaszban leírt hello konfigurációs nincs a szervezet reprezentatív használandó.</span><span class="sxs-lookup"><span data-stu-id="4d518-126">It should be used in all cases where hello configuration described in previous section for express is not representative for your organization.</span></span>

<span data-ttu-id="4d518-127">A következő esetekben használja:</span><span class="sxs-lookup"><span data-stu-id="4d518-127">Use when:</span></span>

- <span data-ttu-id="4d518-128">Önnek nincs hozzáférési tooan vállalati rendszergazdai fiók az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="4d518-128">You do not have access tooan enterprise admin account in Active Directory.</span></span>
- <span data-ttu-id="4d518-129">Több erdővel rendelkezik, vagy egynél több erdő a jövőbeli hello toosynchronize tervezi.</span><span class="sxs-lookup"><span data-stu-id="4d518-129">You have more than one forest or you plan toosynchronize more than one forest in hello future.</span></span>
- <span data-ttu-id="4d518-130">Az erdő hello Connect-kiszolgáló nem elérhető tartományok vannak.</span><span class="sxs-lookup"><span data-stu-id="4d518-130">You have domains in your forest not reachable from hello Connect server.</span></span>
- <span data-ttu-id="4d518-131">Tervezi toouse összevonási vagy a felhasználói bejelentkezés átmenő hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="4d518-131">You plan toouse federation or pass-through authentication for user sign-in.</span></span>
- <span data-ttu-id="4d518-132">Több mint 100 000 objektummal rendelkezik, és toouse teljes SQL Server szükséges.</span><span class="sxs-lookup"><span data-stu-id="4d518-132">You have more than 100,000 objects and need toouse a full SQL Server.</span></span>
- <span data-ttu-id="4d518-133">Tervezi toouse csoport alapú szűrés és nem csak tartomány vagy szervezeti egység alapú szűrés.</span><span class="sxs-lookup"><span data-stu-id="4d518-133">You plan toouse group-based filtering and not only domain or OU-based filtering.</span></span>

## <a name="upgrade-from-dirsync"></a><span data-ttu-id="4d518-134">Frissítés a DirSync szolgáltatásról</span><span class="sxs-lookup"><span data-stu-id="4d518-134">Upgrade from DirSync</span></span>
<span data-ttu-id="4d518-135">Ha a DirSync jelenleg használ, majd kövesse hello [frissítésre a Dirsyncről](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade a meglévő konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="4d518-135">If you are currently using DirSync, then follow hello steps in [Upgrade from DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md) tooupgrade your existing configuration.</span></span> <span data-ttu-id="4d518-136">Két különböző frissítési lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="4d518-136">There are two different upgrade options:</span></span>

- <span data-ttu-id="4d518-137">Helybeni verziófrissítés tooinstall Csatakoznia hello azonos kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="4d518-137">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="4d518-138">Párhuzamos központi telepítés tooinstall Connect egy új kiszolgálón hello működő DirSync-kiszolgálóval pedig továbbra is működik.</span><span class="sxs-lookup"><span data-stu-id="4d518-138">Parallel deployment tooinstall Connect on a new server while hello existing DirSync server is still operational.</span></span>

## <a name="upgrade-from-azure-ad-sync"></a><span data-ttu-id="4d518-139">Frissítés Azure AD-szinkronizálóról</span><span class="sxs-lookup"><span data-stu-id="4d518-139">Upgrade from Azure AD Sync</span></span>
<span data-ttu-id="4d518-140">Ha Azure AD Sync jelenleg használ, akkor hajtsa végre az hello [ugyanazokat a lépéseket](active-directory-aadconnect-upgrade-previous-version.md) , amikor frissít egy csatlakozás verziója tooa újabb a.</span><span class="sxs-lookup"><span data-stu-id="4d518-140">If you are currently using Azure AD Sync, then you can follow hello [same steps](active-directory-aadconnect-upgrade-previous-version.md) as when you upgrade from one Connect version tooa newer.</span></span> <span data-ttu-id="4d518-141">Két különböző frissítési lehetőségek közül:</span><span class="sxs-lookup"><span data-stu-id="4d518-141">There are two different upgrade options:</span></span>

- <span data-ttu-id="4d518-142">Helybeni verziófrissítés tooinstall Csatakoznia hello azonos kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="4d518-142">In-place upgrade tooinstall Connect on hello same server.</span></span>
- <span data-ttu-id="4d518-143">Mozgó-áttelepítési tooinstall Connect hello meglévő Azure AD Sync server során egy új kiszolgálón továbbra is működik.</span><span class="sxs-lookup"><span data-stu-id="4d518-143">Swing-migration tooinstall Connect on a new server while hello existing Azure AD Sync server is still operational.</span></span>

## <a name="migrate-from-fim2010-or-mim2016"></a><span data-ttu-id="4d518-144">FIM2010 vagy MIM2016 áttelepítése</span><span class="sxs-lookup"><span data-stu-id="4d518-144">Migrate from FIM2010 or MIM2016</span></span>
<span data-ttu-id="4d518-145">Ha a Forefront Identity Manager 2010 vagy a Microsoft Identity Manager 2016 az Azure AD-összekötő hello jelenleg használ, majd egyetlen választása marad: áttelepítés.</span><span class="sxs-lookup"><span data-stu-id="4d518-145">If you are currently using Forefront Identity Manager 2010 or Microsoft Identity Manager 2016 with hello Azure AD Connector, then your only option is a migration.</span></span> <span data-ttu-id="4d518-146">Kövesse az ismertetett lépések hello [mozgó-áttelepítési](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span><span class="sxs-lookup"><span data-stu-id="4d518-146">Follow hello steps described in [swing-migration](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).</span></span> <span data-ttu-id="4d518-147">Cserélje le az Azure AD Sync bármely hashtagként hello lépéseket FIM2010/MIM2016.</span><span class="sxs-lookup"><span data-stu-id="4d518-147">In hello steps, replace any mention of Azure AD Sync with FIM2010/MIM2016.</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d518-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4d518-148">Next steps</span></span>
<span data-ttu-id="4d518-149">Attól függően, hogy hello lehetőséget választotta a toouse, hello táblázat a tartalom toohello bal oldali toofind hello a cikk részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="4d518-149">Depending on hello option you have selected toouse, use hello table of content toohello left toofind your article with hello detailed steps.</span></span>
