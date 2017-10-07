---
title: "aaaAzure Data Catalog előfeltételei |} Microsoft Docs"
description: "További tudnivalók az Azure Data Catalog használatába tooget kell hello előfeltételek."
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
ms.openlocfilehash: 0c8e768e5846c61b542b746d7ad80121725a9ec7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-prerequisites"></a><span data-ttu-id="66482-103">Az Azure Data Catalog előfeltételei</span><span class="sxs-lookup"><span data-stu-id="66482-103">Azure Data Catalog prerequisites</span></span>

<span data-ttu-id="66482-104">Mielőtt Azure Data Catalog állíthat be kell tootake néhány dolgot kiszolgálásához.</span><span class="sxs-lookup"><span data-stu-id="66482-104">You need tootake care of a few things before you can set up Azure Data Catalog.</span></span> <span data-ttu-id="66482-105">Ne aggódjon, a folyamat nem tart sokáig.</span><span class="sxs-lookup"><span data-stu-id="66482-105">Don’t worry, this process does not take long.</span></span>

## <a name="azure-subscription"></a><span data-ttu-id="66482-106">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="66482-106">Azure subscription</span></span>
<span data-ttu-id="66482-107">tooset mentése a Data Catalog hello tulajdonosa vagy egy Azure-előfizetés társtulajdonos kell lennie.</span><span class="sxs-lookup"><span data-stu-id="66482-107">tooset up Data Catalog, you must be hello owner or co-owner of an Azure subscription.</span></span>

<span data-ttu-id="66482-108">Azure-előfizetések megkönnyítik például a Data Catalog toocloud-szolgáltatás erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="66482-108">Azure subscriptions help you organize access toocloud-service resources such as Data Catalog.</span></span> <span data-ttu-id="66482-109">Előfizetések is segíthetnek, erőforrás-használat jelentett, számlázva és kifizette vezérlésére.</span><span class="sxs-lookup"><span data-stu-id="66482-109">Subscriptions also help you control how resource usage is reported, billed, and paid for.</span></span> <span data-ttu-id="66482-110">Minden előfizetés van külön számlázási és a fizetési telepítő, így előfizetések és a csomagok, amelyek módosítják a részleg, a projekt, a regionális office és az stb.</span><span class="sxs-lookup"><span data-stu-id="66482-110">Each subscription can have a separate billing and payment setup, so you can have subscriptions and plans that vary by department, project, regional office, and so on.</span></span> <span data-ttu-id="66482-111">Minden felhőalapú szolgáltatás tooa előfizetéshez tartozik, és meg kell toohave előfizetés, a Data Catalog beállítása előtt.</span><span class="sxs-lookup"><span data-stu-id="66482-111">Every cloud service belongs tooa subscription, and you need toohave a subscription before you set up Data Catalog.</span></span> <span data-ttu-id="66482-112">több, lásd: toolearn [kezelhetők a fiókok, előfizetések és rendszergazdai szerepkörök](../active-directory/active-directory-assign-admin-roles.md).</span><span class="sxs-lookup"><span data-stu-id="66482-112">toolearn more, see [Manage accounts, subscriptions, and administrative roles](../active-directory/active-directory-assign-admin-roles.md).</span></span>

## <a name="azure-active-directory"></a><span data-ttu-id="66482-113">Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="66482-113">Azure Active Directory</span></span>
<span data-ttu-id="66482-114">tooset mentése a Data Catalog, be kell jelentkeznie be egy Azure Active Directory (Azure AD) felhasználói fiókkal.</span><span class="sxs-lookup"><span data-stu-id="66482-114">tooset up Data Catalog, you must be signed in with an Azure Active Directory (Azure AD) user account.</span></span>

<span data-ttu-id="66482-115">Az Azure AD az üzleti toomanage identitás és hozzáférés, egyszerre hello felhőben és helyszíni egyszerű módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="66482-115">Azure AD provides an easy way for your business toomanage identity and access, both in hello cloud and on-premises.</span></span> <span data-ttu-id="66482-116">Egyszeri bejelentkezés tooany felhő felhasználók használja egy egyetlen munkahelyi vagy iskolai fiókkal, és a helyszíni webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="66482-116">Users can use a single work or school account for single sign-in tooany cloud and on-premises web application.</span></span> <span data-ttu-id="66482-117">A Data Catalog használja az Azure AD tooauthenticate bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="66482-117">Data Catalog uses Azure AD tooauthenticate sign-in.</span></span> <span data-ttu-id="66482-118">több, lásd: toolearn [Mi az Azure Active Directory?](../active-directory/active-directory-whatis.md).</span><span class="sxs-lookup"><span data-stu-id="66482-118">toolearn more, see [What is Azure Active Directory?](../active-directory/active-directory-whatis.md).</span></span>

> [!NOTE]
> <span data-ttu-id="66482-119">Hello segítségével [Azure-portálon](http://portal.azure.com/), jelentkezhet be személyes Microsoft-fiókkal vagy egy Azure Active Directory munkahelyi vagy iskolai fiók.</span><span class="sxs-lookup"><span data-stu-id="66482-119">By using hello [Azure portal](http://portal.azure.com/), you can sign in with either a personal Microsoft account or an Azure Active Directory work or school account.</span></span> <span data-ttu-id="66482-120">használatával a Data Catalog mentése tooset vagy hello Azure-portálon vagy hello [Data Catalog-portál](http://www.azuredatacatalog.com), Azure Active Directory-fiókkal, nem egy személyes fiókkal kell bejelentkeznie.</span><span class="sxs-lookup"><span data-stu-id="66482-120">tooset up Data Catalog by using either hello Azure portal or hello [Data Catalog portal](http://www.azuredatacatalog.com), you must sign in with an Azure Active Directory account, not a personal account.</span></span>
>
>

## <a name="active-directory-policy-configuration"></a><span data-ttu-id="66482-121">Az Active Directory-házirend konfigurálása</span><span class="sxs-lookup"><span data-stu-id="66482-121">Active Directory policy configuration</span></span>
<span data-ttu-id="66482-122">Olyan helyzet ahol bejelentkezhet toohello Data Catalog-portált, de az adatforrás-regisztráló eszköz toohello toosign tett kísérlet során előforduló, észlel olyan hibaüzenetet, amely megakadályozza, hogy jelentkezik be.</span><span class="sxs-lookup"><span data-stu-id="66482-122">You might encounter a situation where you can sign in toohello Data Catalog portal, but when you attempt toosign in toohello data source registration tool, you encounter an error message that prevents you from signing in.</span></span> <span data-ttu-id="66482-123">Ez a probléma a probléma akkor fordulhat elő, csak akkor, ha az Ön hello vállalati hálózaton található, vagy Ez akkor fordulhat elő, csak ha csatlakoztat külső hello vállalati hálózatról.</span><span class="sxs-lookup"><span data-stu-id="66482-123">This problem behavior might occur only when you're on hello company network, or it might occur only when you're connecting from outside hello company network.</span></span>

<span data-ttu-id="66482-124">hello adatforrás-regisztráló eszköz használja az űrlapos hitelesítés toovalidate a felhasználói hitelesítő adatait az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="66482-124">hello data source registration tool uses Forms Authentication toovalidate your user credentials against Active Directory.</span></span> <span data-ttu-id="66482-125">toohelp a bejelentkezés sikeres, az Active Directory-rendszergazda kell engedélyezheti az űrlapos hitelesítést a globális hitelesítési házirend hello.</span><span class="sxs-lookup"><span data-stu-id="66482-125">toohelp you sign in successfully, an Active Directory administrator must enable Forms Authentication in hello Global Authentication Policy.</span></span>

<span data-ttu-id="66482-126">Hitelesítési módszerek hello globális hitelesítési házirend, ahogy az alábbi képernyőfelvétel a hello is lehet külön-külön engedélyezni, az intranetes és extranetes kapcsolatok esetén.</span><span class="sxs-lookup"><span data-stu-id="66482-126">In hello Global Authentication Policy, authentication methods can be enabled separately for intranet and extranet connections, as shown in hello following screenshot.</span></span> <span data-ttu-id="66482-127">Bejelentkezési hiba is felléphet, ha a hello hálózati kapcsolat, amelyből nincs engedélyezve az űrlapos hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="66482-127">Sign-in errors might occur if Forms Authentication is not enabled for hello network from which you're connecting.</span></span>

 ![Az Active Directory globális hitelesítési házirend](./media/data-catalog-prerequisites/global-auth-policy.png)

## <a name="next-steps"></a><span data-ttu-id="66482-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="66482-129">Next steps</span></span>
<span data-ttu-id="66482-130">További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).</span><span class="sxs-lookup"><span data-stu-id="66482-130">For more information, see [Configuring Authentication Policies](https://technet.microsoft.com/library/dn486781.aspx).</span></span>
