---
title: "Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése |} Microsoft Docs"
description: "Ez a témakör azt javasolja, hogy az AD Lomtár szolgáltatás az Azure AD Connect hello használatát."
services: active-directory
keywords: "Az AD Lomtár véletlen törlés, forráshorgony"
documentationcenter: 
author: cychua
manager: femila
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 2bb4827d677ccecfd8d2861f2a2fcf73b8cc2d95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="4b1a3-104">Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése</span><span class="sxs-lookup"><span data-stu-id="4b1a3-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="4b1a3-105">Ajánlott a helyszíni Active könyvtárak, amelyek szinkronizált tooAzure AD hello AD Lomtár funkciót engedélyezni.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-105">It is recommended that you enable hello AD Recycle Bin feature for your on-premises Active Directories, which are synchronized tooAzure AD.</span></span> 

<span data-ttu-id="4b1a3-106">Ha véletlenül törölt egy helyszíni AD-felhasználói objektum és a visszaállítási hello funkcióval, az Azure AD helyreállítja hello megfelelő az Azure AD felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-106">If you accidentally deleted an on-premises AD user object and restore it using hello feature, Azure AD restores hello corresponding Azure AD user object.</span></span>  <span data-ttu-id="4b1a3-107">Hello AD Lomtár szolgáltatással kapcsolatos további információkért tekintse meg a tooarticle [forgatókönyv áttekintése visszaállítása törölt Active Directory-objektumok](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="4b1a3-107">For information about hello AD Recycle Bin feature, refer tooarticle [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-hello-ad-recycle-bin"></a><span data-ttu-id="4b1a3-108">A Lomtár engedélyezése hello AD előnyei</span><span class="sxs-lookup"><span data-stu-id="4b1a3-108">Benefits of enabling hello AD recycle bin</span></span>
<span data-ttu-id="4b1a3-109">Ez a szolgáltatás segít az Azure AD felhasználói objektumok visszaállítására hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="4b1a3-109">This feature helps with restoring Azure AD user objects by doing hello following:</span></span>

* <span data-ttu-id="4b1a3-110">Ha véletlenül törölt egy helyszíni AD-felhasználó objektum hello megfelelő az Azure AD felhasználói objektum törli hello a következő szinkronizálási ciklusban.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-110">If you accidentally deleted an on-premises AD user object, hello corresponding Azure AD user object will be deleted in hello next sync cycle.</span></span> <span data-ttu-id="4b1a3-111">Alapértelmezés szerint az Azure AD tartja hello törölni az Azure AD felhasználói objektum törölt állapotban 30 napig.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-111">By default, Azure AD keeps hello deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="4b1a3-112">Ha a helyszíni AD Lomtár szolgáltatás engedélyezve van, a törölt hello visszaállíthatja a helyszíni AD-felhasználói objektum Forráshorgony értékének módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-112">If you have on-premises AD Recycle Bin feature enabled, you can restore hello deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="4b1a3-113">Ha hello helyreállítani a helyszíni AD-felhasználói objektum szinkronizálása tooAzure AD, az Azure AD visszaállítja hello megfelelő helyreállíthatóan törölt az Azure AD felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-113">When hello recovered on-premises AD user object is synchronized tooAzure AD, Azure AD will restore hello corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="4b1a3-114">Forráshorgony attribútum kapcsolatos információkért tekintse meg a tooarticle [az Azure AD Connect: tervezési alapelvek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="4b1a3-114">For information about Source Anchor attribute, refer tooarticle [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="4b1a3-115">Ha nem rendelkezik a helyszíni AD Recycle Bin szolgáltatás engedélyezve van, előfordulhat, hogy az AD felhasználói objektum tooreplace hello törölt objektum szükséges toocreate.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required toocreate an AD user object tooreplace hello deleted object.</span></span> <span data-ttu-id="4b1a3-116">Ha az Azure AD Connect szinkronizálási szolgáltatást konfigurált toouse a rendszer AD attribútum (például az ObjectGuid) hello Forráshorgony attribútuma, hello újonnan létrehozott AD-felhasználói objektum lesz nem rendelkezik hello azonos Forráshorgony érték hello AD felhasználó-objektum törlése.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-116">If Azure AD Connect Synchronization Service is configured toouse system-generated AD attribute (such as ObjectGuid) for hello Source Anchor attribute, hello newly created AD user object will not have hello same Source Anchor value as hello deleted AD user object.</span></span> <span data-ttu-id="4b1a3-117">Ha hello újonnan létrehozott AD felhasználói objektum szinkronizált tooAzure AD, az Azure AD létrehoz egy új Azure AD felhasználói objektum visszaállítása helyreállíthatóan törölt hello Azure AD felhasználói objektum helyett.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-117">When hello newly created AD user object is synchronized tooAzure AD, Azure AD creates a new Azure AD user object instead of restoring hello soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="4b1a3-118">Alapértelmezés szerint az Azure AD tartja törölni az Azure AD felhasználói objektumok helyreállíthatóan törölt állapotban 30 nap elteltével a rendszer véglegesen törli.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="4b1a3-119">Azonban a rendszergazdák meggyorsíthatja ilyen objektumok hello törlése.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-119">However, administrators can accelerate hello deletion of such objects.</span></span> <span data-ttu-id="4b1a3-120">Hello objektumok véglegesen törlődnek, ha azok már nem állíthatók helyre, még akkor is, ha a helyszíni AD Lomtár szolgáltatás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="4b1a3-120">Once hello objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="4b1a3-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4b1a3-121">Next steps</span></span>
<span data-ttu-id="4b1a3-122">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="4b1a3-122">**Overview topics**</span></span>

* [<span data-ttu-id="4b1a3-123">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="4b1a3-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="4b1a3-124">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="4b1a3-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
