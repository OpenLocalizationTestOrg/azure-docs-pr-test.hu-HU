---
title: "az Azure Active Directoryban aaaAdministrative egységek felügyeleti előzetes verzió"
description: "Adminisztratív egységei használ az Azure Active Directory engedélyeit részletesebb delegálása"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 8464cd6b-1d1a-470d-a4fb-ee29b8eab4c4
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/17/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: oldportal;it-pro;
ms.openlocfilehash: ee2c7beb6f9f6292bbf3cdeab00801ac066ae0e4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="administrative-units-management-in-azure-ad---public-preview"></a><span data-ttu-id="148a7-103">Az adminisztrációs egységek felügyelete az Azure AD - nyilvános előzetes verzió</span><span class="sxs-lookup"><span data-stu-id="148a7-103">Administrative units management in Azure AD - public preview</span></span>
<span data-ttu-id="148a7-104">Ez a cikk ismerteti adminisztratív egységei – egy új Azure Active Directory-tároló rendszergazdai engedélyek delegálása felhasználók részhalmaza és alkalmazása házirendek tooa részhalmazát felhasználók használható erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="148a7-104">This article describes administrative units – a new Azure Active Directory container of resources that can be used for delegating administrative permissions over subsets of users and applying policies tooa subset of users.</span></span> <span data-ttu-id="148a7-105">Az Azure Active Directoryban a felügyeleti egység központi rendszergazdák toodelegate engedélyek tooregional rendszergazdák vagy tooset házirend alapszinten engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="148a7-105">In Azure Active Directory, administrative units enable central administrators toodelegate permissions tooregional administrators or tooset policy at a granular level.</span></span>

<span data-ttu-id="148a7-106">Ez akkor hasznos, a független részlegek, például a nagy egyetemi sok autonóm iskola (üzleti iskolai, műszaki osztály iskolai, és így tovább) egymástól független végzett rendelkező szervezetek számára.</span><span class="sxs-lookup"><span data-stu-id="148a7-106">This is useful in organizations with independent divisions, for example, a large university that is made up of many autonomous schools (Business school, Engineering school, and so on) which are independent from each other.</span></span> <span data-ttu-id="148a7-107">Az ilyen osztályok rendelkezik saját informatikai rendszergazdáknak, akik hozzáférést, a felhasználók kezelése és a kifejezetten a felosztás vonatkozó házirendek beállítása.</span><span class="sxs-lookup"><span data-stu-id="148a7-107">Such divisions have their own IT administrators who control access, manage users, and set policies specifically for their division.</span></span> <span data-ttu-id="148a7-108">Központi rendszergazdáknak is érdemes toobe képes grant ezen részlegszintű rendszergazdák engedélyek hello felhasználók számára az adott részlegek keresztül.</span><span class="sxs-lookup"><span data-stu-id="148a7-108">Central administrators want toobe able grant these divisional administrators permissions over hello users in their particular divisions.</span></span> <span data-ttu-id="148a7-109">Pontosabban ebben a példában, egy központi felügyeleti segítségével, például egy adott iskolai (üzleti iskolai) a felügyeleti egység létrehozása és feltöltése hello üzleti iskolai felhasználókra.</span><span class="sxs-lookup"><span data-stu-id="148a7-109">More specifically, using this example, a central administrator can, for instance, create an administrative unit for a particular school (Business school) and populate it with only hello Business school users.</span></span> <span data-ttu-id="148a7-110">A központi felügyeleti majd hello üzleti iskolai informatikai munkatársak hatókörű tooa szerepkör, más szóval grant hello üzleti iskolai felügyeleti engedélyeket csak keresztül hello iskolai felügyeleti részleg informatikai munkatársak adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="148a7-110">Then a central administrator can add hello Business school IT staff tooa scoped role, in other words, grant hello IT staff of Business school administrative permissions only over hello Business school administrative unit.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="148a7-111">Felügyeleti egység hatókörbe tartozó rendszergazdai szerepkörök csak akkor, ha engedélyezi a Azure Active Directory Premium rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="148a7-111">You can assign administrative unit-scoped admin roles only if you enable Azure Active Directory Premium.</span></span> <span data-ttu-id="148a7-112">További információkért lásd: [Ismerkedés az Azure AD Premium](active-directory-get-started-premium.md).</span><span class="sxs-lookup"><span data-stu-id="148a7-112">For more information, see [Getting started with Azure AD Premium](active-directory-get-started-premium.md).</span></span>
>


<span data-ttu-id="148a7-113">Hello központi felügyeleti szempontból egy felügyeleti egység hozható létre és töltődik erőforrások címtárobjektum.</span><span class="sxs-lookup"><span data-stu-id="148a7-113">From hello central administrator’s point of view, an administrative unit is a directory object that can be created and populated with resources.</span></span> <span data-ttu-id="148a7-114">**Ebben az előzetes kiadásban ezeket az erőforrásokat lehet felhasználókra.**</span><span class="sxs-lookup"><span data-stu-id="148a7-114">**In this preview release, these resources can be only users.**</span></span> <span data-ttu-id="148a7-115">Miután létre és töltődik fel, hello felügyeleti egység egy hatókör toorestrict hello engedélyt csak hello felügyeleti egység található erőforrások keresztül is használható.</span><span class="sxs-lookup"><span data-stu-id="148a7-115">Once created and populated, hello administrative unit can be used as a scope toorestrict hello granted permission only over resources contained in hello administrative unit.</span></span>

## <a name="managing-administrative-units"></a><span data-ttu-id="148a7-116">Felügyeleti egységek kezelése</span><span class="sxs-lookup"><span data-stu-id="148a7-116">Managing administrative units</span></span>
<span data-ttu-id="148a7-117">Ebben az előzetes kiadásban létrehozhat és adminisztratív egységei hello Azure Active Directory modul a Windows PowerShell-parancsmagok használatával kezelheti.</span><span class="sxs-lookup"><span data-stu-id="148a7-117">In this preview release, you can create and manage administrative units using hello Azure Active Directory Module for Windows PowerShell cmdlets.</span></span> <span data-ttu-id="148a7-118">toolearn kapcsolatos további által látható toodo [adminisztratív egységei használata](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span><span class="sxs-lookup"><span data-stu-id="148a7-118">toolearn more about how toodo that, see [Working with Administrative Units](https://docs.microsoft.com/powershell/azure/active-directory/working-with-administrative-units?view=azureadps-2.0)</span></span>

<span data-ttu-id="148a7-119">További információ a szoftverkövetelmények és hello Azure AD-modul telepítése, és hello Azure AD-modullal parancsmagok adminisztratív egységei, beleértve a szintaxist, a paraméterek leírásait és a példákat, kezelésével kapcsolatos információk [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="148a7-119">For more information on software requirements and installing hello Azure AD module, and for information on hello Azure AD Module cmdlets for managing administrative units, including syntax, parameter descriptions, and examples, see [Azure Active Directory PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-2.0).</span></span>

## <a name="next-steps"></a><span data-ttu-id="148a7-120">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="148a7-120">Next steps</span></span>
[<span data-ttu-id="148a7-121">Az Azure Active Directory-kiadások</span><span class="sxs-lookup"><span data-stu-id="148a7-121">Azure Active Directory editions</span></span>](active-directory-editions.md)
