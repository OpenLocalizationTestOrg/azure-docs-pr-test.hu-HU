---
title: "Privát Docker-beállításjegyzék létrehozása – Azure Portal | Microsoft Docs"
description: "Bevezetés a privát Docker-beállításjegyzékek létrehozásába és kezelésébe az Azure Portalon"
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
ms.openlocfilehash: 7fbbb56d775ee96c9a44363a4e41d4fc3c630582
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a><span data-ttu-id="75aba-103">Privát Docker-tárolójegyzék létrehozása az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="75aba-103">Create a private Docker container registry using the Azure portal</span></span>
<span data-ttu-id="75aba-104">Az Azure Portalon létrehozhat egy tároló-beállításjegyzéket, és kezelheti a beállításait.</span><span class="sxs-lookup"><span data-stu-id="75aba-104">Use the Azure portal to create a container registry and manage its settings.</span></span> <span data-ttu-id="75aba-105">A tárolóregisztrációs adatbázisokat létrehozhatja és kezelheti az [Azure CLI 2.0 parancsaival](container-registry-get-started-azure-cli.md), az [Azure PowerShell](container-registry-get-started-powershell.md) használatával vagy programozott módon a tárolóregisztrációs adatbázis [REST API-jával](https://go.microsoft.com/fwlink/p/?linkid=834376) is.</span><span class="sxs-lookup"><span data-stu-id="75aba-105">You can also create and manage container registries using the [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md), [Azure PowerShell](container-registry-get-started-powershell.md) or programmatically with the Container Registry [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376).</span></span>

<span data-ttu-id="75aba-106">Háttér-információkért és a fogalmakkal kapcsolatban lásd [az áttekintést](container-registry-intro.md).</span><span class="sxs-lookup"><span data-stu-id="75aba-106">For background and concepts, see [the overview](container-registry-intro.md).</span></span>

## <a name="create-a-container-registry"></a><span data-ttu-id="75aba-107">Tároló-beállításjegyzék létrehozása</span><span class="sxs-lookup"><span data-stu-id="75aba-107">Create a container registry</span></span>
1. <span data-ttu-id="75aba-108">Az [Azure Portalon](https://portal.azure.com) kattintson az **+Új** elemre.</span><span class="sxs-lookup"><span data-stu-id="75aba-108">In the [Azure portal](https://portal.azure.com), click **+ New**.</span></span>
2. <span data-ttu-id="75aba-109">Keresse meg a piactéren az **Azure Container Registry-t**.</span><span class="sxs-lookup"><span data-stu-id="75aba-109">Search the marketplace for **Azure container registry**.</span></span>
3. <span data-ttu-id="75aba-110">Válassza ki az **Azure Container Registry-t**, amelynek közzétevője a **Microsoft**.</span><span class="sxs-lookup"><span data-stu-id="75aba-110">Select **Azure Container Registry**, with publisher **Microsoft**.</span></span>
    <span data-ttu-id="75aba-111">![Container Registry szolgáltatás az Azure Marketplace-en](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span><span class="sxs-lookup"><span data-stu-id="75aba-111">![Container Registry service in Azure Marketplace](./media/container-registry-get-started-portal/container-registry-marketplace.png)</span></span>
4. <span data-ttu-id="75aba-112">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="75aba-112">Click **Create**.</span></span> <span data-ttu-id="75aba-113">Megjelenik az **Azure Container Registry** panel.</span><span class="sxs-lookup"><span data-stu-id="75aba-113">The **Azure Container Registry** blade appears.</span></span>

    ![A tároló-beállításjegyzékek beállításai](./media/container-registry-get-started-portal/container-registry-settings.png)
5. <span data-ttu-id="75aba-115">Az **Azure Container Registry** panelen adja meg a következő információkat.</span><span class="sxs-lookup"><span data-stu-id="75aba-115">In the **Azure Container Registry** blade, enter the following information.</span></span> <span data-ttu-id="75aba-116">Amikor elkészült, kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="75aba-116">Click **Create** when you are done.</span></span>

    <span data-ttu-id="75aba-117">a.</span><span class="sxs-lookup"><span data-stu-id="75aba-117">a.</span></span> <span data-ttu-id="75aba-118">**Beállításjegyzék neve**: egy globálisan egyedi legfelső szintű tartománynév az adott beállításjegyzékhez.</span><span class="sxs-lookup"><span data-stu-id="75aba-118">**Registry name**: A globally unique top-level domain name for your specific registry.</span></span> <span data-ttu-id="75aba-119">Ebben a példában a beállításjegyzék neve *myRegistry01*, de helyettesítsen be egy saját, egyedi nevet.</span><span class="sxs-lookup"><span data-stu-id="75aba-119">In this example, the registry name is *myRegistry01*, but substitute a unique name of your own.</span></span> <span data-ttu-id="75aba-120">A név csak betűket és számokat tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="75aba-120">The name can contain only letters and numbers.</span></span>

    <span data-ttu-id="75aba-121">b.</span><span class="sxs-lookup"><span data-stu-id="75aba-121">b.</span></span> <span data-ttu-id="75aba-122">**Erőforráscsoport**: Válasszon egy meglévő [erőforráscsoportot](../azure-resource-manager/resource-group-overview.md#resource-groups), vagy adja meg egy új csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="75aba-122">**Resource group**: Select an existing [resource group](../azure-resource-manager/resource-group-overview.md#resource-groups) or type the name for a new one.</span></span>

    <span data-ttu-id="75aba-123">c.</span><span class="sxs-lookup"><span data-stu-id="75aba-123">c.</span></span> <span data-ttu-id="75aba-124">**Hely**: Válasszon ki egy Azure-adatközpontot, ahol a szolgáltatás [elérhető](https://azure.microsoft.com/regions/services/), például az **USA déli középső régióját**.</span><span class="sxs-lookup"><span data-stu-id="75aba-124">**Location**: Select an Azure datacenter location where the service is [available](https://azure.microsoft.com/regions/services/), such as **South Central US**.</span></span>

    <span data-ttu-id="75aba-125">d.</span><span class="sxs-lookup"><span data-stu-id="75aba-125">d.</span></span> <span data-ttu-id="75aba-126">**Rendszergazdai felhasználó**: Ha szeretné, engedélyezze egy rendszergazdai felhasználó számára a beállításjegyzék elérését.</span><span class="sxs-lookup"><span data-stu-id="75aba-126">**Admin user**: If you want, enable an admin user to access the registry.</span></span> <span data-ttu-id="75aba-127">Ezt a beállítást a beállításjegyzék létrehozását követően módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="75aba-127">You can change this setting after creating the registry.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="75aba-128">A tároló-beállításjegyzékek, amellett, hogy hozzáférést biztosítanak egy rendszergazdai felhasználói fiókon keresztül, támogatják az Azure Active Directory egyszerű szolgáltatásaira épülő hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="75aba-128">In addition to providing access through an admin user account, container registries support authentication backed by Azure Active Directory service principals.</span></span> <span data-ttu-id="75aba-129">További információkat és szempontokat [a tároló-beállításjegyzékkel való hitelesítéssel kapcsolatos cikkben](container-registry-authentication.md) találhat.</span><span class="sxs-lookup"><span data-stu-id="75aba-129">For more information and considerations, see [Authenticate with a container registry](container-registry-authentication.md).</span></span>
      >

    <span data-ttu-id="75aba-130">e.</span><span class="sxs-lookup"><span data-stu-id="75aba-130">e.</span></span> <span data-ttu-id="75aba-131">**Storage-fiók**: Hozzon létre egy [Storage-fiókot](../storage/common/storage-introduction.md) az alapértelmezett beállítással, vagy válasszon egy meglévő tárfiókot ugyanezen a helyen.</span><span class="sxs-lookup"><span data-stu-id="75aba-131">**Storage account**: Use the default setting to create a [storage account](../storage/common/storage-introduction.md), or select an existing storage account in the same location.</span></span> <span data-ttu-id="75aba-132">A Premium Storage jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="75aba-132">Currently Premium Storage is not supported.</span></span>

## <a name="manage-registry-settings"></a><span data-ttu-id="75aba-133">Beállításjegyzék beállításainak kezelése</span><span class="sxs-lookup"><span data-stu-id="75aba-133">Manage registry settings</span></span>
<span data-ttu-id="75aba-134">A beállításjegyzék létrehozását követően a beállításjegyzék-beállításokat a portál **Tároló-beállításjegyzékek** paneljéről kiindulva találja meg.</span><span class="sxs-lookup"><span data-stu-id="75aba-134">After creating the registry, find the registry settings by starting at the **Container Registries** blade in the portal.</span></span> <span data-ttu-id="75aba-135">Például szüksége lehet a beállításjegyzékbe való bejelentkezés beállításaira, vagy esetleg szeretné engedélyezni vagy letiltani a rendszergazdai felhasználót.</span><span class="sxs-lookup"><span data-stu-id="75aba-135">For example, you might need the settings to log in to your registry, or you might want to enable or disable the admin user.</span></span>

1. <span data-ttu-id="75aba-136">A **Tároló-beállításjegyzékek** panelen kattintson a beállításjegyzék nevére.</span><span class="sxs-lookup"><span data-stu-id="75aba-136">On the **Container Registries** blade, click the name of your registry.</span></span>

    ![Tároló-beállításjegyzék panel](./media/container-registry-get-started-portal/container-registry-blade.png)
2. <span data-ttu-id="75aba-138">A hozzáférési beállítások kezeléséhez kattintson a **Hozzáférési kulcs** elemre.</span><span class="sxs-lookup"><span data-stu-id="75aba-138">To manage access settings, click **Access key**.</span></span>

    ![Hozzáférés a tároló-beállításjegyzékhez](./media/container-registry-get-started-portal/container-registry-access.png)
3. <span data-ttu-id="75aba-140">Ügyeljen a következő beállításokra:</span><span class="sxs-lookup"><span data-stu-id="75aba-140">Note the following settings:</span></span>

   * <span data-ttu-id="75aba-141">**Bejelentkezési kiszolgáló** – A teljes név, amelyet a beállításjegyzékbe való bejelentkezéshez használ.</span><span class="sxs-lookup"><span data-stu-id="75aba-141">**Login server** - The fully qualified name you use to log in to the registry.</span></span> <span data-ttu-id="75aba-142">Ebben a példában ez `myregistry01.azurecr.io`.</span><span class="sxs-lookup"><span data-stu-id="75aba-142">In this example, it is `myregistry01.azurecr.io`.</span></span>
   * <span data-ttu-id="75aba-143">**Rendszergazdai felhasználó** – Ki- és bekapcsolhatja a beállításjegyzék rendszergazdai felhasználói fiókját.</span><span class="sxs-lookup"><span data-stu-id="75aba-143">**Admin user** - Toggle to enable or disable the registry's admin user account.</span></span>
   * <span data-ttu-id="75aba-144">**Felhasználó** és **Jelszó** – A rendszergazdai felhasználói fiók (ha engedélyezve van) hitelesítő adatai, amelyek használatával bejelentkezhet a beállításjegyzékbe.</span><span class="sxs-lookup"><span data-stu-id="75aba-144">**Username** and **Password** - The credentials of the admin user account (if enabled) you can use to log in to the registry.</span></span> <span data-ttu-id="75aba-145">Szükség esetén újból létrehozhatja a jelszavakat.</span><span class="sxs-lookup"><span data-stu-id="75aba-145">You can optionally regenerate the passwords.</span></span> <span data-ttu-id="75aba-146">Két jelszó jön létre, így az egyik jelszó ismételt létrehozása esetén a másikkal közben fenntarthatja a beállításjegyzék kapcsolatait.</span><span class="sxs-lookup"><span data-stu-id="75aba-146">Two passwords are created so that you can maintain connections to the registry by using one password while you regenerate the other password.</span></span> <span data-ttu-id="75aba-147">Ha ehelyett egyszerű szolgáltatást szeretne használni a hitelesítéshez, tekintse meg a [privát Docker-tároló beállításjegyzékével történő hitelesítést leíró szakaszt](container-registry-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="75aba-147">To authenticate with a service principal instead, see [Authenticate with a private Docker container registry](container-registry-authentication.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="75aba-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75aba-148">Next steps</span></span>
* [<span data-ttu-id="75aba-149">Az első rendszerkép leküldése a Docker parancssori felületével</span><span class="sxs-lookup"><span data-stu-id="75aba-149">Push your first image using the Docker CLI</span></span>](container-registry-get-started-docker-cli.md)
