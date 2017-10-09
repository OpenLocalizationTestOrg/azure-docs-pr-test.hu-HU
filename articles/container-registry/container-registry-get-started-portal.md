---
title: "aaaCreate titkos Docker beállításjegyzék - Azure-portál |} Microsoft Docs"
description: "Első lépések, létrehozását és kezelését a személyes Docker-tároló nyilvántartó, hello Azure-portálon"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40f3ce44fea26e5fbeca865c9da6df55c2df9511
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-private-docker-container-registry-using-hello-azure-portal"></a><span data-ttu-id="41c31-103">Hozza létre a titkos Docker tároló beállításkulcs hello Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="41c31-103">Create a private Docker container registry using hello Azure portal</span></span>
<span data-ttu-id="41c31-104">Az Azure portál toocreate tároló beállításjegyzékbeli hello használatát, és a beállítások kezelését.</span><span class="sxs-lookup"><span data-stu-id="41c31-104">Use hello Azure portal toocreate a container registry and manage its settings.</span></span> <span data-ttu-id="41c31-105">Is hozhat létre, és hello segítségével kezeléséhez tároló [Azure CLI 2.0 parancsok](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) vagy programozott módon hello tároló beállításjegyzék [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span><span class="sxs-lookup"><span data-stu-id="41c31-105">You can also create and manage container registries using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with hello Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="41c31-106">Háttér és fogalmak: [áttekintése hello](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="41c31-106">For background and concepts, see [hello overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="41c31-107">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="41c31-107">Create a container registry</span></span>
1. <span data-ttu-id="41c31-108">A hello [Azure-portálon](https://portal.azure.com), kattintson a **+ új**.</span><span class="sxs-lookup"><span data-stu-id="41c31-108">In hello [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="41c31-109">Hello a piactéren **Azure tároló beállításjegyzék**.</span><span class="sxs-lookup"><span data-stu-id="41c31-109">Search hello marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="41c31-110">Válassza ki az **Azure Container Registry-t**, amelynek közzétevője a **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="41c31-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="41c31-111">![Container Registry szolgáltatás az Azure Marketplace-en](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="41c31-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="41c31-112">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="41c31-112">Click **Create**.</span></span> <span data-ttu-id="41c31-113">Hello **Azure tároló beállításjegyzék** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="41c31-113">hello **Azure Container Registry** blade appears.</span></span>

    ![A tároló-beállításjegyzékek beállításai](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="41c31-115">A hello **Azure tároló beállításjegyzék** panelen adja meg a következő információ hello.</span><span class="sxs-lookup"><span data-stu-id="41c31-115">In hello **Azure Container Registry** blade, enter hello following information.</span></span> <span data-ttu-id="41c31-116">Amikor elkészült, kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="41c31-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="41c31-117">a.</span><span class="sxs-lookup"><span data-stu-id="41c31-117">a.</span></span> <span data-ttu-id="41c31-118">**Beállításjegyzék neve**: egy globálisan egyedi legfelső szintű tartománynév az adott beállításjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="41c31-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="41c31-119">Ebben a példában hello beállításkulcs értéke *myRegistry01*, de helyettesítse a saját, egyedi nevét.</span><span class="sxs-lookup"><span data-stu-id="41c31-119">In this example, hello registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="41c31-120">hello név csak betűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="41c31-120">hello name can contain only letters and numbers.</span></span>

    <span data-ttu-id="41c31-121">b.</span><span class="sxs-lookup"><span data-stu-id="41c31-121">b.</span></span> <span data-ttu-id="41c31-122">**Erőforráscsoport**: Válasszon ki egy létező [erőforráscsoport](../azure-resource-manager/resource-group-overview.md#resource-groups) vagy egy új hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="41c31-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type hello name for a new one.</span></span>

    <span data-ttu-id="41c31-123">c.</span><span class="sxs-lookup"><span data-stu-id="41c31-123">c.</span></span> <span data-ttu-id="41c31-124">**Hely**: válassza ki az Azure-adatközpontban helyet, ahol hello szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/), például a **déli középső Régiójában**.</span><span class="sxs-lookup"><span data-stu-id="41c31-124">**Location**: Select an Azure datacenter location where hello service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="41c31-125">d.</span><span class="sxs-lookup"><span data-stu-id="41c31-125">d.</span></span> <span data-ttu-id="41c31-126">**A rendszergazdai jogú felhasználó**: Ha azt szeretné, egy rendszergazda felhasználó tooaccess hello beállításjegyzék engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="41c31-126">**Admin user**: If you want, enable an admin user tooaccess hello registry.</span></span> <span data-ttu-id="41c31-127">Ez a beállítás hello beállításkulcs létrehozása után módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="41c31-127">You can change this setting after creating hello registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="41c31-128">Ezenkívül tooproviding rendszergazdai felhasználói fiókkal keresztül férnek hozzá, a tároló nyilvántartó támogatja az Azure Active Directory szolgáltatás rendszerbiztonsági tagok által támogatott hitelesítési.</span><span class="sxs-lookup"><span data-stu-id="41c31-128">In addition tooproviding access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="41c31-129">További információkat és szempontokat [a tároló-beállításjegyzékkel való hitelesítéssel kapcsolatos cikkben](container-registry-authentication.md) találhat.</span><span class="sxs-lookup"><span data-stu-id="41c31-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="41c31-130">e.</span><span class="sxs-lookup"><span data-stu-id="41c31-130">e.</span></span> <span data-ttu-id="41c31-131">**A tárfiók**: hello alapértelmezett beállítás toocreate használja egy [tárfiók](../storage/common/storage-introduction.md), vagy jelöljön ki egy meglévő tárfiók hello ugyanazon a helyen.</span><span class="sxs-lookup"><span data-stu-id="41c31-131">**Storage account**: Use hello default setting toocreate a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in hello same location.</span></span> <span data-ttu-id="41c31-132">A Premium Storage jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="41c31-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="41c31-133">Beállításjegyzék beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="41c31-133">Manage registry settings</span></span>
<span data-ttu-id="41c31-134">Miután létrehozta a hello beállításjegyzék, hello beállításjegyzék-beállítások szerinti keresése hello kezdődő **tároló nyilvántartó** hello portál panel.</span><span class="sxs-lookup"><span data-stu-id="41c31-134">After creating hello registry, find hello registry settings by starting at hello **Container Registries** blade in hello portal.</span></span> <span data-ttu-id="41c31-135">Például előfordulhat, hogy hello beállítások toolog tooyour beállításjegyzékben kell, vagy előfordulhat, hogy szeretné, hogy tooenable, vagy tiltsa le a hello rendszergazdai jogú felhasználó.</span><span class="sxs-lookup"><span data-stu-id="41c31-135">For example, you might need hello settings toolog in tooyour registry, or you might want tooenable or disable hello admin user.</span></span>

1. <span data-ttu-id="41c31-136">A hello **tároló nyilvántartó** panelen kattintson a beállításjegyzék hello nevére.</span><span class="sxs-lookup"><span data-stu-id="41c31-136">On hello **Container Registries** blade, click hello name of your registry.</span></span>

    ![Tároló-beállításjegyzék panel](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="41c31-138">toomanage hozzáférési beállításait, kattintson a **hozzáférési kulcs**.</span><span class="sxs-lookup"><span data-stu-id="41c31-138">toomanage access settings, click **Access key**.</span></span>

    ![Hozzáférés a tároló-beállításjegyzékhez](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="41c31-140">Vegye figyelembe a következő beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="41c31-140">Note hello following settings:</span></span>

   * <span data-ttu-id="41c31-141">**Bejelentkezési kiszolgáló** -hello teljesen minősített név toolog toohello beállításjegyzékben használja.</span><span class="sxs-lookup"><span data-stu-id="41c31-141">**Login server** - hello fully qualified name you use toolog in toohello registry.</span></span> <span data-ttu-id="41c31-142">Ebben a példában ez `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="41c31-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="41c31-143">**A rendszergazdai jogú felhasználó** - tooenable bekapcsolására, vagy tiltsa le a hello beállításjegyzék rendszergazda felhasználói fiókot.</span><span class="sxs-lookup"><span data-stu-id="41c31-143">**Admin user** - Toggle tooenable or disable hello registry's admin user account.</span></span>
   * <span data-ttu-id="41c31-144">**Felhasználónév** és **jelszó** -hello hello rendszergazdai felhasználói fiók hitelesítő adatait (Ha engedélyezve van) toolog toohello beállításjegyzék használhatja.</span><span class="sxs-lookup"><span data-stu-id="41c31-144">**Username** and **Password** - hello credentials of hello admin user account (if enabled) you can use toolog in toohello registry.</span></span> <span data-ttu-id="41c31-145">Opcionálisan újragenerálás hello jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="41c31-145">You can optionally regenerate hello passwords.</span></span> <span data-ttu-id="41c31-146">Két jelszavak jönnek létre, így fenntarthatja a kapcsolatokat toohello beállításjegyzék hello újragenerálja egy jelszó segítségével más jelszót.</span><span class="sxs-lookup"><span data-stu-id="41c31-146">Two passwords are created so that you can maintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="41c31-147">az egyszerű szolgáltatás tooauthenticate helyett, lásd: [hitelesítés egy titkos Docker-tároló rendszerleíró](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="41c31-147">tooauthenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="41c31-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="41c31-148">Next steps</span></span>
* [<span data-ttu-id="41c31-149">Leküldéses az első kép hello Docker parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="41c31-149">Push your first image using hello Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
