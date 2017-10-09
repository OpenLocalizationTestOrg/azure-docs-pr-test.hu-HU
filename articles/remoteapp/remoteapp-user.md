---
title: "egy felhasználó tooyour Azure RemoteApp-gyűjtemény aaaAdd |} Microsoft Docs"
description: "Megtudhatja, hogyan tooadd felhasználók tooyour Azure RemoteApp-gyűjtemény"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 6b751fd2-2b11-499f-a2eb-12cb47f3129b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 0ae88e04c8bfc2ed55dc963945ed7e9ff687b603
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooadd-a-user-tooyour-azure-remoteapp-collection"></a><span data-ttu-id="778ad-103">Hogyan tooadd egy felhasználó tooyour Azure RemoteApp-gyűjtemény</span><span class="sxs-lookup"><span data-stu-id="778ad-103">How tooadd a user tooyour Azure RemoteApp collection</span></span>
> [!IMPORTANT]
> <span data-ttu-id="778ad-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="778ad-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="778ad-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="778ad-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="778ad-106">Mielőtt a felhasználók láthatnak és az alkalmazások az Azure Remoteappban használja, meg kell toogrant tooyour gyűjteménye érhető el őket.</span><span class="sxs-lookup"><span data-stu-id="778ad-106">Before your users can see and use your apps in Azure RemoteApp, you have toogrant them access tooyour collection.</span></span> <span data-ttu-id="778ad-107">Ez része hello egyszerű: A hello **felhasználói hozzáférés** lapra, adja meg a hello hello felhasználóhoz tartozó fiókadatokat, és kattintson a hello pipára.</span><span class="sxs-lookup"><span data-stu-id="778ad-107">This is hello easy part: On hello **User Access** tab, enter hello account information for hello user, and then click hello check mark.</span></span>

<span data-ttu-id="778ad-108">Milyen fiókot információra van szüksége?</span><span class="sxs-lookup"><span data-stu-id="778ad-108">What account information do you need?</span></span> <span data-ttu-id="778ad-109">Hello a típusú gyűjtemény alapján létrehozott (felhőalapú vagy hibrid), és hogy a Office 365 ProPlus a gyűjteményben, amely függ.</span><span class="sxs-lookup"><span data-stu-id="778ad-109">That depends on hello type of collection you created (cloud or hybrid) and whether you are using Office 365 ProPlus in that collection.</span></span>

## <a name="supported-user-identities"></a><span data-ttu-id="778ad-110">Támogatott felhasználói identitások</span><span class="sxs-lookup"><span data-stu-id="778ad-110">Supported user identities</span></span>
<span data-ttu-id="778ad-111">hello különböző gyűjteménytípusok (felhőalapú vagy hibrid) támogatja a hozzáférés tooapplications különböző felhasználói identitásokat.</span><span class="sxs-lookup"><span data-stu-id="778ad-111">hello different collection types (cloud vs. hybrid) support using different user identities for access tooapplications.</span></span>  

<span data-ttu-id="778ad-112">RemoteApp hibrid gyűjtemény meg kell tooset fel az Active Directory tartományi infrastruktúra helyi és a címtár-integráció az Azure Active Directory-bérlő (és opcionálisan egyszeri bejelentkezés).</span><span class="sxs-lookup"><span data-stu-id="778ad-112">For a hybrid collection of RemoteApp, you need tooset up an Active Directory domain infrastructure on premises and an Azure Active Directory tenant with Directory Integration (and optionally single sign-on).</span></span> <span data-ttu-id="778ad-113">Ezen felül kell toocreate egyes Active Directory-objektumok hello helyszíni címtárban.</span><span class="sxs-lookup"><span data-stu-id="778ad-113">Additionally, you need toocreate some Active Directory objects in hello on-premises directory.</span></span>  

<span data-ttu-id="778ad-114">A RemoteApp felhőalapú gyűjtemény minden olyan felhasználó, amely rendelkezik az Azure Active Directory identitások támogatásához lehet adni a felhasználói hozzáférés tooRemoteApp tooinclude Microsoft Accounts.</span><span class="sxs-lookup"><span data-stu-id="778ad-114">For a cloud collection of RemoteApp, any user that has Azure Active Directory support identities can be granted user access tooRemoteApp tooinclude Microsoft Accounts.</span></span>  <span data-ttu-id="778ad-115">Az alábbi hello táblázatban találja.</span><span class="sxs-lookup"><span data-stu-id="778ad-115">See hello table below.</span></span>

<span data-ttu-id="778ad-116">Office 365-felhasználók az Azure Active Directory-felhasználók.</span><span class="sxs-lookup"><span data-stu-id="778ad-116">Office 365 users are Azure Active Directory users.</span></span> <span data-ttu-id="778ad-117">Ha az Azure Active Directory hibrid rendelkeznek, Directory szinkronizált fiókokkal, azokat is hozzáférést felhasználó RemoteApp hibrid telepítés.</span><span class="sxs-lookup"><span data-stu-id="778ad-117">If they have Azure Active Directory hybrid, Directory synchronized accounts, they can be granted user access in a RemoteApp hybrid deployment.</span></span>   

<span data-ttu-id="778ad-118">Ez a táblázat azonosító mentésre alkalmas a gyűjteményt, és milyen hello Active Directory követelményei vannak a gyors referenciaként használható.</span><span class="sxs-lookup"><span data-stu-id="778ad-118">You can use this table as a quick reference for which identity is supported in your collection and what hello Active Directory requirements are.</span></span>

| <span data-ttu-id="778ad-119">Felhasználói fiókok</span><span class="sxs-lookup"><span data-stu-id="778ad-119">User accounts</span></span> | <span data-ttu-id="778ad-120">Felhő</span><span class="sxs-lookup"><span data-stu-id="778ad-120">Cloud</span></span> | <span data-ttu-id="778ad-121">Hibrid</span><span class="sxs-lookup"><span data-stu-id="778ad-121">Hybrid</span></span> |
| --- | --- | --- |
| <span data-ttu-id="778ad-122">Microsoft-fiók</span><span class="sxs-lookup"><span data-stu-id="778ad-122">Microsoft Account</span></span> |<span data-ttu-id="778ad-123">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-123">Yes</span></span> |<span data-ttu-id="778ad-124">Nem</span><span class="sxs-lookup"><span data-stu-id="778ad-124">No</span></span> |
| <span data-ttu-id="778ad-125">Azure Active Directory (Azure AD)</span><span class="sxs-lookup"><span data-stu-id="778ad-125">Azure Active Directory (Azure AD)</span></span> | | |
| <span data-ttu-id="778ad-126">Csak az Azure AD felhő</span><span class="sxs-lookup"><span data-stu-id="778ad-126">Azure AD cloud only</span></span> |<span data-ttu-id="778ad-127">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-127">Yes</span></span> |<span data-ttu-id="778ad-128">Nem</span><span class="sxs-lookup"><span data-stu-id="778ad-128">No</span></span> |
| <span data-ttu-id="778ad-129">Jelszó-szinkronizálással ADsync</span><span class="sxs-lookup"><span data-stu-id="778ad-129">ADsync with password sync</span></span> |<span data-ttu-id="778ad-130">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-130">Yes</span></span> |<span data-ttu-id="778ad-131">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-131">Yes</span></span> |
| <span data-ttu-id="778ad-132">A jelszó-szinkronizálás nélkül ADsync</span><span class="sxs-lookup"><span data-stu-id="778ad-132">ADsync without password sync</span></span> |<span data-ttu-id="778ad-133">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-133">Yes</span></span> |<span data-ttu-id="778ad-134">Nem</span><span class="sxs-lookup"><span data-stu-id="778ad-134">No</span></span> |
| <span data-ttu-id="778ad-135">Az AD FS ADsync</span><span class="sxs-lookup"><span data-stu-id="778ad-135">ADsync with AD FS</span></span> |<span data-ttu-id="778ad-136">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-136">Yes</span></span> |<span data-ttu-id="778ad-137">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-137">Yes</span></span> |
| <span data-ttu-id="778ad-138">[3. fél Azure támogatott Identitásszolgáltatók](https://msdn.microsoft.com/library/azure/jj679342.aspx) (például a Ping)</span><span class="sxs-lookup"><span data-stu-id="778ad-138">[3rd-party Azure supported identity providers](https://msdn.microsoft.com/library/azure/jj679342.aspx)  (example Ping)</span></span> |<span data-ttu-id="778ad-139">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-139">Yes</span></span> |<span data-ttu-id="778ad-140">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-140">Yes</span></span> |
| <span data-ttu-id="778ad-141">Multi-Factor Authentication</span><span class="sxs-lookup"><span data-stu-id="778ad-141">Multi-Factor Authentication</span></span> |<span data-ttu-id="778ad-142">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-142">Yes</span></span> |<span data-ttu-id="778ad-143">Igen</span><span class="sxs-lookup"><span data-stu-id="778ad-143">Yes</span></span> |

<span data-ttu-id="778ad-144">Tekintse meg [további információt](remoteapp-ad.md) vonatkozó Active Directory konfigurálása a RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="778ad-144">Check out [more information](remoteapp-ad.md) about configuring Active Directory for RemoteApp.</span></span>

> [!NOTE]
> <span data-ttu-id="778ad-145">hello Azure Active Directory-felhasználók hello bérlői előfizetéshez tartozó kell lennie.</span><span class="sxs-lookup"><span data-stu-id="778ad-145">hello Azure Active Directory users must be from hello tenant that's associated with your subscription.</span></span> <span data-ttu-id="778ad-146">(Megtekintheti és módosíthatja az előfizetését hello **beállítások** hello portálon lapon.</span><span class="sxs-lookup"><span data-stu-id="778ad-146">(You can view and modify your subscription on hello **Settings** tab in hello portal.</span></span> <span data-ttu-id="778ad-147">Lásd: [módosítás hello Azure Active Directory-bérlő RemoteApp által használt](remoteapp-changetenant.md) további információt.)</span><span class="sxs-lookup"><span data-stu-id="778ad-147">See [Change hello Azure Active Directory tenant used by RemoteApp](remoteapp-changetenant.md) for more information.)</span></span>
> 
> 

## <a name="office-365-proplus-user-account-information"></a><span data-ttu-id="778ad-148">Az Office 365 ProPlus felhasználói fiókok adatait</span><span class="sxs-lookup"><span data-stu-id="778ad-148">Office 365 ProPlus user account information</span></span>
<span data-ttu-id="778ad-149">Ha a gyűjtemény hello Office 365 ProPlus sablon rendszerképet használ *vagy* Ha létrehozott egy egyéni lemezképet, amely használja az Office 365, csak engedélyezett hello Office 365-előfizetés rendelkező tooadd Azure Active Directory-felhasználók alapértelmezett tartomány az előfizetéséhez.</span><span class="sxs-lookup"><span data-stu-id="778ad-149">If you are using hello Office 365 ProPlus template image in your collection *or* if you created a custom image that uses Office 365, you are only allowed tooadd Azure Active Directory users that have Office 365 subscriptions for hello default domain of your subscription.</span></span> <span data-ttu-id="778ad-150">Lásd: [Office 365 segítségével az Azure RemoteApp](remoteapp-o365.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="778ad-150">See [Using Office 365 with Azure RemoteApp](remoteapp-o365.md) for more information.</span></span>

