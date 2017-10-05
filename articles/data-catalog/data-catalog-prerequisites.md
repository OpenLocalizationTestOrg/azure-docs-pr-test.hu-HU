---
title: "Az Azure Data Catalog előfeltételei |} Microsoft Docs"
description: "További tudnivalók az Azure Data Catalog használatába kell előfeltételek."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: ef497a54-dc4d-4820-b5bf-c361b64b964d
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 3fdef7bb58a5cd5dfbe4d37d9baf9c8e392ebe42
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-catalog-prerequisites"></a><span data-ttu-id="d299d-103">Az Azure Data Catalog előfeltételei</span><span class="sxs-lookup"><span data-stu-id="d299d-103">Azure Data Catalog prerequisites</span></span>

<span data-ttu-id="d299d-104">Néhány dolgot gondoskodunk Azure Data Catalog beállítása előtt kell.</span><span class="sxs-lookup"><span data-stu-id="d299d-104">You need to take care of a few things before you can set up Azure Data Catalog.</span></span> <span data-ttu-id="d299d-105">Ne aggódjon, a folyamat nem tart sokáig.</span><span class="sxs-lookup"><span data-stu-id="d299d-105">Don’t worry, this process does not take long.</span></span>

## <a name="azure-subscription"></a><span data-ttu-id="d299d-106">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="d299d-106">Azure subscription</span></span>
<span data-ttu-id="d299d-107">A Data Catalog beállítása, a tulajdonos vagy a társtulajdonos az Azure-előfizetéssel kell lennie.</span><span class="sxs-lookup"><span data-stu-id="d299d-107">To set up Data Catalog, you must be the owner or co-owner of an Azure subscription.</span></span>

<span data-ttu-id="d299d-108">Azure-előfizetések megkönnyítik felhőszolgáltatás erőforrások, például a Data Catalog elérésére.</span><span class="sxs-lookup"><span data-stu-id="d299d-108">Azure subscriptions help you organize access to cloud-service resources such as Data Catalog.</span></span> <span data-ttu-id="d299d-109">Előfizetések is segíthetnek, erőforrás-használat jelentett, számlázva és kifizette vezérlésére.</span><span class="sxs-lookup"><span data-stu-id="d299d-109">Subscriptions also help you control how resource usage is reported, billed, and paid for.</span></span> <span data-ttu-id="d299d-110">Minden előfizetés van külön számlázási és a fizetési telepítő, így előfizetések és a csomagok, amelyek módosítják a részleg, a projekt, a regionális office és az stb.</span><span class="sxs-lookup"><span data-stu-id="d299d-110">Each subscription can have a separate billing and payment setup, so you can have subscriptions and plans that vary by department, project, regional office, and so on.</span></span> <span data-ttu-id="d299d-111">Egy előfizetés tartozik minden felhőalapú szolgáltatás, és kell rendelkeznie egy előfizetéshez, mielőtt a Data Catalog beállítása.</span><span class="sxs-lookup"><span data-stu-id="d299d-111">Every cloud service belongs to a subscription, and you need to have a subscription before you set up Data Catalog.</span></span> <span data-ttu-id="d299d-112">További információkat a [fiókok, előfizetések és rendszergazdai szerepkörök kezeléséről](../active-directory/active-directory-assign-admin-roles.md) szóló cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="d299d-112">To learn more, see [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md).</span></span>

## <a name="azure-active-directory"></a><span data-ttu-id="d299d-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d299d-113">Azure Active Directory</span></span>
<span data-ttu-id="d299d-114">A Data Catalog beállításához be kell jelentkeznie be egy Azure Active Directory (Azure AD) felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d299d-114">To set up Data Catalog, you must be signed in with an Azure Active Directory (Azure AD) user account.</span></span>

<span data-ttu-id="d299d-115">Az Azure AD egyszerű módot kínál vállalkozásának az identitás és a hozzáférés kezelésére, mind a felhőben, mind a helyszínen.</span><span class="sxs-lookup"><span data-stu-id="d299d-115">Azure AD provides an easy way for your business to manage identity and access, both in the cloud and on-premises.</span></span> <span data-ttu-id="d299d-116">Felhasználók is használhat egy egyetlen munkahelyi vagy iskolai fiókkal egyszeri bejelentkezés az összes felhő, és a helyszíni webalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d299d-116">Users can use a single work or school account for single sign-in to any cloud and on-premises web application.</span></span> <span data-ttu-id="d299d-117">A Data Catalog használ az Azure AD-bejelentkezés hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="d299d-117">Data Catalog uses Azure AD to authenticate sign-in.</span></span> <span data-ttu-id="d299d-118">További tudnivalókért lásd: [Mi az Azure Active Directory?](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="d299d-118">To learn more, see [What is Azure Active Directory?](../active-directory/active-directory-whatis.md).</span></span>

> [!NOTE]
> <span data-ttu-id="d299d-119">Használatával a [Azure-portálon](http://portal.azure.com/), jelentkezhet be személyes Microsoft-fiókkal vagy egy Azure Active Directory munkahelyi vagy iskolai fiók.</span><span class="sxs-lookup"><span data-stu-id="d299d-119">By using the [Azure portal](http://portal.azure.com/), you can sign in with either a personal Microsoft account or an Azure Active Directory work or school account.</span></span> <span data-ttu-id="d299d-120">Állíthatja be a Data Catalog használatával vagy az Azure-portálon vagy a [Data Catalog-portál](http://www.azuredatacatalog.com), Azure Active Directory-fiókkal, nem egy személyes fiókkal kell bejelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="d299d-120">To set up Data Catalog by using either the Azure portal or the [Data Catalog portal](http://www.azuredatacatalog.com), you must sign in with an Azure Active Directory account, not a personal account.</span></span>
>
>

## <a name="active-directory-policy-configuration"></a><span data-ttu-id="d299d-121">Az Active Directory-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d299d-121">Active Directory policy configuration</span></span>
<span data-ttu-id="d299d-122">Akkor léphetnek olyan helyzet, ahol bejelentkezhet a Data Catalog-portált, de ha megkísérli az jelentkezzen be az adatforrás-regisztráló eszköz, olyan hibaüzenetet, amely megakadályozza, hogy aláírási észlel.</span><span class="sxs-lookup"><span data-stu-id="d299d-122">You might encounter a situation where you can sign in to the Data Catalog portal, but when you attempt to sign in to the data source registration tool, you encounter an error message that prevents you from signing in.</span></span> <span data-ttu-id="d299d-123">Ez a probléma a probléma akkor fordulhat elő, csak akkor, ha a vállalati hálózaton, vagy Ez akkor fordulhat elő, csak akkor, ha, amelyből csatlakozik a vállalati hálózaton kívülről.</span><span class="sxs-lookup"><span data-stu-id="d299d-123">This problem behavior might occur only when you're on the company network, or it might occur only when you're connecting from outside the company network.</span></span>

<span data-ttu-id="d299d-124">Az adatforrás-regisztráló eszköz Active Directoryban a felhasználói fiók hitelesítő adatainak érvényesítéséhez használja a az űrlapos hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d299d-124">The data source registration tool uses Forms Authentication to validate your user credentials against Active Directory.</span></span> <span data-ttu-id="d299d-125">Segítséget a bejelentkezés sikeres, az Active Directory-rendszergazda engedélyeznie kell az űrlapos hitelesítést a globális hitelesítési házirend.</span><span class="sxs-lookup"><span data-stu-id="d299d-125">To help you sign in successfully, an Active Directory administrator must enable Forms Authentication in the Global Authentication Policy.</span></span>

<span data-ttu-id="d299d-126">A globális hitelesítési házirend a hitelesítési módszerek engedélyezhető külön-külön az intranetes és extranetes kapcsolatok, az alábbi képernyőfelvételen látható módon.</span><span class="sxs-lookup"><span data-stu-id="d299d-126">In the Global Authentication Policy, authentication methods can be enabled separately for intranet and extranet connections, as shown in the following screenshot.</span></span> <span data-ttu-id="d299d-127">Bejelentkezési hiba is felléphet, ha a hálózati kapcsolat, amelyből nincs engedélyezve az űrlapos hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="d299d-127">Sign-in errors might occur if Forms Authentication is not enabled for the network from which you're connecting.</span></span>

 ![Az Active Directory globális hitelesítési házirend](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a><span data-ttu-id="d299d-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d299d-129">Next steps</span></span>
<span data-ttu-id="d299d-130">További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="d299d-130">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>
