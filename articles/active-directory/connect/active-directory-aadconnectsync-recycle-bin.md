---
title: "Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése |} Microsoft Docs"
description: "Ez a témakör azt javasolja, hogy az Azure AD Connect AD Lomtár funkciójának használatát."
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
ms.openlocfilehash: eb455477547f3db8245cf3601576eba9c6fdc56f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a><span data-ttu-id="acb79-104">Az Azure AD Connect szinkronizálási szolgáltatás: AD Lomtár engedélyezése</span><span class="sxs-lookup"><span data-stu-id="acb79-104">Azure AD Connect sync: Enable AD recycle bin</span></span>
<span data-ttu-id="acb79-105">Javasoljuk, hogy az AD Lomtár funkció engedélyezése a helyszíni Active címtárak, amely szinkronizálva az Azure AD.</span><span class="sxs-lookup"><span data-stu-id="acb79-105">It is recommended that you enable the AD Recycle Bin feature for your on-premises Active Directories, which are synchronized to Azure AD.</span></span> 

<span data-ttu-id="acb79-106">Ha véletlenül törölt egy helyszíni AD-felhasználói objektum és a visszaállítási funkcióval, az Azure AD helyreállítja a megfelelő Azure AD-felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="acb79-106">If you accidentally deleted an on-premises AD user object and restore it using the feature, Azure AD restores the corresponding Azure AD user object.</span></span>  <span data-ttu-id="acb79-107">Az AD Lomtár funkció kapcsolatos információkért tekintse meg a cikk [forgatókönyv áttekintése visszaállítása törölt Active Directory-objektumok](https://technet.microsoft.com/library/dd379542.aspx).</span><span class="sxs-lookup"><span data-stu-id="acb79-107">For information about the AD Recycle Bin feature, refer to article [Scenario Overview for Restoring Deleted Active Directory Objects](https://technet.microsoft.com/library/dd379542.aspx).</span></span>

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a><span data-ttu-id="acb79-108">Az AD Lomtár engedélyezése előnyei</span><span class="sxs-lookup"><span data-stu-id="acb79-108">Benefits of enabling the AD recycle bin</span></span>
<span data-ttu-id="acb79-109">Ez a szolgáltatás segít az Azure Active Directory-felhasználói objektumok visszaállítása a következő módon:</span><span class="sxs-lookup"><span data-stu-id="acb79-109">This feature helps with restoring Azure AD user objects by doing the following:</span></span>

* <span data-ttu-id="acb79-110">Ha véletlenül törölt egy helyszíni AD-felhasználó objektumazonosító, a megfelelő Azure AD-felhasználói objektum törli a következő szinkronizálási ciklusban.</span><span class="sxs-lookup"><span data-stu-id="acb79-110">If you accidentally deleted an on-premises AD user object, the corresponding Azure AD user object will be deleted in the next sync cycle.</span></span> <span data-ttu-id="acb79-111">Alapértelmezés szerint az Azure AD megtartja a törölt felhasználóobjektum az Azure AD 30 napig helyreállíthatóan törölt állapotban.</span><span class="sxs-lookup"><span data-stu-id="acb79-111">By default, Azure AD keeps the deleted Azure AD user object in soft-deleted state for 30 days.</span></span>

* <span data-ttu-id="acb79-112">Ha a helyszíni AD Recycle Bin szolgáltatás engedélyezve van, a törölt visszaállíthatja a helyszíni AD-felhasználói objektum Forráshorgony értékének módosítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="acb79-112">If you have on-premises AD Recycle Bin feature enabled, you can restore the deleted on-premises AD user object without changing its Source Anchor value.</span></span> <span data-ttu-id="acb79-113">Ha a helyreállított a helyszíni AD-felhasználói objektum szinkronizálva az Azure AD, az Azure AD fog visszaállítása a megfelelő helyreállíthatóan törölt az Azure AD felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="acb79-113">When the recovered on-premises AD user object is synchronized to Azure AD, Azure AD will restore the corresponding soft-deleted Azure AD user object.</span></span> <span data-ttu-id="acb79-114">Forráshorgony attribútum kapcsolatos információkért tekintse meg a cikk [az Azure AD Connect: tervezési alapelvek](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span><span class="sxs-lookup"><span data-stu-id="acb79-114">For information about Source Anchor attribute, refer to article [Azure AD Connect: Design concepts](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).</span></span>

* <span data-ttu-id="acb79-115">Ha nem rendelkezik a helyszíni AD Lomtár szolgáltatás engedélyezve van, akkor lehet szükség az AD-felhasználói objektum lecseréli a törölt objektum létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="acb79-115">If you do not have on-premises AD Recycle Bin feature enabled, you may be required to create an AD user object to replace the deleted object.</span></span> <span data-ttu-id="acb79-116">Ha az Azure AD Connect szinkronizálási szolgáltatást a Forráshorgony attribútum a rendszer AD attribútum (például az ObjectGuid) használatára van konfigurálva, az újonnan létrehozott AD-felhasználói objektum nem lesz Forráshorgony értéke megegyezik az AD felhasználó objektum.</span><span class="sxs-lookup"><span data-stu-id="acb79-116">If Azure AD Connect Synchronization Service is configured to use system-generated AD attribute (such as ObjectGuid) for the Source Anchor attribute, the newly created AD user object will not have the same Source Anchor value as the deleted AD user object.</span></span> <span data-ttu-id="acb79-117">Amikor az újonnan létrehozott AD-felhasználói objektum szinkronizálja az Azure AD, az Azure AD létrehoz egy új Azure AD felhasználói objektum helyett a helyreállíthatóan törölt visszaállítása az Azure AD felhasználói objektum.</span><span class="sxs-lookup"><span data-stu-id="acb79-117">When the newly created AD user object is synchronized to Azure AD, Azure AD creates a new Azure AD user object instead of restoring the soft-deleted Azure AD user object.</span></span>

> [!NOTE]
> <span data-ttu-id="acb79-118">Alapértelmezés szerint az Azure AD tartja törölni az Azure AD felhasználói objektumok helyreállíthatóan törölt állapotban 30 nap elteltével a rendszer véglegesen törli.</span><span class="sxs-lookup"><span data-stu-id="acb79-118">By default, Azure AD keeps deleted Azure AD user objects in soft-deleted state for 30 days before they are permanently deleted.</span></span> <span data-ttu-id="acb79-119">Azonban a rendszergazdák az ilyen objektumok a törlés meggyorsíthatja.</span><span class="sxs-lookup"><span data-stu-id="acb79-119">However, administrators can accelerate the deletion of such objects.</span></span> <span data-ttu-id="acb79-120">Az objektumok véglegesen törlődnek, ha azok már nem állíthatók helyre, még akkor is, ha a helyszíni AD Lomtár szolgáltatás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="acb79-120">Once the objects are permanently deleted, they can no longer be recovered, even if on-premises AD Recycle Bin feature is enabled.</span></span>



## <a name="next-steps"></a><span data-ttu-id="acb79-121">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="acb79-121">Next steps</span></span>
<span data-ttu-id="acb79-122">**Áttekintő témakör**</span><span class="sxs-lookup"><span data-stu-id="acb79-122">**Overview topics**</span></span>

* [<span data-ttu-id="acb79-123">Azure AD Connect szinkronizálása: megértéséhez, valamint a szinkronizálás testreszabása</span><span class="sxs-lookup"><span data-stu-id="acb79-123">Azure AD Connect sync: Understand and customize synchronization</span></span>](active-directory-aadconnectsync-whatis.md)

* [<span data-ttu-id="acb79-124">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="acb79-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
