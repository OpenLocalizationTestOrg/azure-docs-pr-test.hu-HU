---
title: "Az Azure AD Connect: Preview szolgáltatásai |} Microsoft Docs"
description: "Ez a témakör ismerteti a további részletek funkciók még csak előzetes verziójúak, az Azure AD Connectben."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: c75cd8cf-3eff-4619-bbca-66276757cc07
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: bcfc710861b19d8f86f094ced0d1c691e0911f08
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="more-details-about-features-in-preview"></a><span data-ttu-id="d275c-103">További információt az előzetes funkciók</span><span class="sxs-lookup"><span data-stu-id="d275c-103">More details about features in preview</span></span>
<span data-ttu-id="d275c-104">Ez a témakör ismerteti, hogyan toouse jelenleg előzetes verzióban érhetők szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="d275c-104">This topic describes how toouse features currently in preview.</span></span>

## <a name="group-writeback"></a><span data-ttu-id="d275c-105">Group writeback (Csoportvisszaíró)</span><span class="sxs-lookup"><span data-stu-id="d275c-105">Group writeback</span></span>
<span data-ttu-id="d275c-106">a csoportok visszaírásához a választható funkciók a hello beállítás lehetővé teszi toowriteback **Office 365-csoportok** telepített Exchange tooa erdő.</span><span class="sxs-lookup"><span data-stu-id="d275c-106">hello option for group writeback in optional features allows you toowriteback **Office 365 Groups** tooa forest with Exchange installed.</span></span> <span data-ttu-id="d275c-107">Ez az egy csoportot, amely mindig hello felhőben értékűre.</span><span class="sxs-lookup"><span data-stu-id="d275c-107">This is a group that is always mastered in hello cloud.</span></span> <span data-ttu-id="d275c-108">Ha a helyszíni Exchange-hez, majd írhat vissza ezen csoportok tooon helyi, a felhasználók egy helyszíni Exchange postaládával küldhet és fogadhat e-mailek ezekből a csoportokból.</span><span class="sxs-lookup"><span data-stu-id="d275c-108">If you have Exchange on-premises, then you can write back these groups tooon-premises so users with an on-premises Exchange mailbox can send and receive emails from these groups.</span></span>

<span data-ttu-id="d275c-109">További információ az Office 365-csoportokat, és hogyan toouse őket található [Itt](http://aka.ms/O365g).</span><span class="sxs-lookup"><span data-stu-id="d275c-109">More information about Office 365 Groups and how toouse them can be found [here](http://aka.ms/O365g).</span></span>

<span data-ttu-id="d275c-110">Az Office 365-csoportok egy jelenik meg a helyszíni terjesztési csoport Active Directory tartományi Szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="d275c-110">An Office 365 group is represented as a distribution group in on-premises AD DS.</span></span> <span data-ttu-id="d275c-111">A helyszíni Exchange server Ez az új csoporttípus Exchange 2013-as összesítő frissítés 8 (2015. márciusi jelent) vagy az Exchange 2016 toorecognize kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d275c-111">Your on-premises Exchange server must be on Exchange 2013 cumulative update 8 (released in March 2015) or Exchange 2016 toorecognize this new group type.</span></span>

<span data-ttu-id="d275c-112">**Hello előzetes megjegyzések**</span><span class="sxs-lookup"><span data-stu-id="d275c-112">**Notes during hello preview**</span></span>

* <span data-ttu-id="d275c-113">hello cím könyv attribútum jelenleg nincs feltöltve a hello előzetes kiadásában.</span><span class="sxs-lookup"><span data-stu-id="d275c-113">hello address book attribute is currently not populated in hello preview.</span></span> <span data-ttu-id="d275c-114">Ez az attribútum nélkül hello csoport nincs a globális Címlista hello látható.</span><span class="sxs-lookup"><span data-stu-id="d275c-114">Without this attribute, hello group is not visible in hello GAL.</span></span> <span data-ttu-id="d275c-115">Ennek legegyszerűbb módja toopopulate hello toouse hello Exchange PowerShell-parancsmag erre az attribútumra `update-recipient`.</span><span class="sxs-lookup"><span data-stu-id="d275c-115">hello easiest way toopopulate this attribute is toouse hello Exchange PowerShell cmdlet `update-recipient`.</span></span>
* <span data-ttu-id="d275c-116">Hello Exchange-séma csak erdőt a csoportok érvényes cél.</span><span class="sxs-lookup"><span data-stu-id="d275c-116">Only forests with hello Exchange schema are valid targets for groups.</span></span> <span data-ttu-id="d275c-117">Ha nincs Exchange észlelte, majd csoportvisszaírásról nincs lehetséges tooenable.</span><span class="sxs-lookup"><span data-stu-id="d275c-117">If no Exchange was detected, then group writeback is not possible tooenable.</span></span>
* <span data-ttu-id="d275c-118">Csak egyerdős Exchange-szervezet telepítések jelenleg támogatottak.</span><span class="sxs-lookup"><span data-stu-id="d275c-118">Only single-forest Exchange organization deployments are currently supported.</span></span> <span data-ttu-id="d275c-119">Ha több mint egy szervezet helyszíni Exchange-, akkor szüksége egy helyszíni GALSync megoldás ezen csoportok tooappear a más erdőkben.</span><span class="sxs-lookup"><span data-stu-id="d275c-119">If you have more than one Exchange organization on-premises, then you need an on-premises GALSync solution for these groups tooappear in your other forests.</span></span>
* <span data-ttu-id="d275c-120">hello csoport visszaírási szolgáltatása nem kezeli a biztonsági csoportok vagy terjesztési csoportok.</span><span class="sxs-lookup"><span data-stu-id="d275c-120">hello Group writeback feature does not handle security groups or distribution groups.</span></span>

> [!NOTE]
> <span data-ttu-id="d275c-121">Egy előfizetés tooAzure AD Premium szükség a csoportok visszaírásához.</span><span class="sxs-lookup"><span data-stu-id="d275c-121">A subscription tooAzure AD Premium is required for group writeback.</span></span>
> 
>

## <a name="user-writeback"></a><span data-ttu-id="d275c-122">Felhasználók visszaírásához.</span><span class="sxs-lookup"><span data-stu-id="d275c-122">User writeback</span></span>
> [!IMPORTANT]
> <span data-ttu-id="d275c-123">hello felhasználói visszaírási előzetes funkció el lett távolítva a hello 2015. augusztus frissítés tooAzure AD Connect.</span><span class="sxs-lookup"><span data-stu-id="d275c-123">hello user writeback preview feature was removed in hello August 2015 update tooAzure AD Connect.</span></span> <span data-ttu-id="d275c-124">Ha engedélyezte azt, majd tiltsa le ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d275c-124">If you have enabled it, then you should disable this feature.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="d275c-125">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d275c-125">Next steps</span></span>
<span data-ttu-id="d275c-126">Továbbra is a [az Azure AD Connect egyéni telepítési](active-directory-aadconnect-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d275c-126">Continue your [Custom installation of Azure AD Connect](active-directory-aadconnect-get-started-custom.md).</span></span>

<span data-ttu-id="d275c-127">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="d275c-127">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
