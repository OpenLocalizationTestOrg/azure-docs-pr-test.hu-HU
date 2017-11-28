---
title: "egy Azure-tárolót rendszerleíró aaaAuthenticate |} Microsoft Docs"
description: "Hogyan toolog tooan Azure tároló beállításjegyzék használatával egy Azure Active Directory szolgáltatás egyszerű vagy egy rendszergazdai fiók"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: cristyg
tags: 
keywords: 
ms.assetid: 128a937a-766a-415b-b9fc-35a6c2f27417
ms.service: container-registry
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2017
ms.author: stevelas
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a0b0462e8432b2567689debca322e2426baa7fa2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-with-a-private-docker-container-registry"></a><span data-ttu-id="4a3b2-103">A hitelesítést egy titkos Docker-tároló beállításjegyzék</span><span class="sxs-lookup"><span data-stu-id="4a3b2-103">Authenticate with a private Docker container registry</span></span>
<span data-ttu-id="4a3b2-104">egy Azure-tárolót beállításjegyzék tároló képekkel toowork, jelentkezzen be hello `docker login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-104">toowork with container images in an Azure container registry, you log in using hello `docker login` command.</span></span> <span data-ttu-id="4a3b2-105">Segítségével bejelentkezhet egy  **[Azure Active Directory szolgáltatás egyszerű](../active-directory/active-directory-application-objects.md)**  vagy a beállításjegyzék-specifikus **rendszergazdai fiók**.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-105">You can log in using either an **[Azure Active Directory service principal](../active-directory/active-directory-application-objects.md)** or a registry-specific **admin account**.</span></span> <span data-ttu-id="4a3b2-106">A cikkben az identitások további információkra.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-106">This article provides more detail about these identities.</span></span>



## <a name="service-principal"></a><span data-ttu-id="4a3b2-107">Egyszerű szolgáltatásnév</span><span class="sxs-lookup"><span data-stu-id="4a3b2-107">Service principal</span></span>

<span data-ttu-id="4a3b2-108">Is [rendelje hozzá egy egyszerű](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour beállításjegyzék és a Docker egyszerű hitelesítést használni.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-108">You can [assign a service principal](container-registry-get-started-azure-cli.md#assign-a-service-principal) tooyour registry and use it for basic Docker authentication.</span></span> <span data-ttu-id="4a3b2-109">Legtöbb esetben egy egyszerű szolgáltatás használata ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-109">Using a service principal is recommended for most scenarios.</span></span> <span data-ttu-id="4a3b2-110">Hello alkalmazás Azonosítóját és jelszavát hello szolgáltatás egyszerű toohello `docker login` parancsban, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="4a3b2-110">Provide hello app ID and password of hello service principal toohello `docker login` command, as shown in hello following example:</span></span>

```
docker login myregistry.azurecr.io -u xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx -p myPassword
```

<span data-ttu-id="4a3b2-111">Miután bejelentkezett, Docker hello hitelesítő adatokat, gyorsítótárazza, így nem kell tooremember hello alkalmazás azonosítója.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-111">Once logged in, Docker caches hello credentials, so you don't need tooremember hello app ID.</span></span>

> [!TIP]
> <span data-ttu-id="4a3b2-112">Ha azt szeretné, újragenerálás hello jelszavát, amely egy egyszerű hello futtatásával `az ad sp reset-credentials` parancsot.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-112">If you want, you can regenerate hello password of a service principal by running hello `az ad sp reset-credentials` command.</span></span>
>


<span data-ttu-id="4a3b2-113">Engedélyezi szolgáltatás rendszerbiztonsági tagok [szerepkörön alapuló hozzáférés](../active-directory/role-based-access-control-configure.md) tooa beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-113">Service principals allow [role-based access](../active-directory/role-based-access-control-configure.md) tooa registry.</span></span> <span data-ttu-id="4a3b2-114">A következő szerepkörök választhatók:</span><span class="sxs-lookup"><span data-stu-id="4a3b2-114">Available roles are:</span></span>
  * <span data-ttu-id="4a3b2-115">(Csak hozzáférésre lekéréses)-olvasó.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-115">Reader (pull only access).</span></span>
  * <span data-ttu-id="4a3b2-116">Közreműködő (lekérési és leküldési).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-116">Contributor (pull and push).</span></span>
  * <span data-ttu-id="4a3b2-117">Tulajdonos (lekéréses leküldéses és hozzárendelése szerepkörökhöz tooother felhasználók).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-117">Owner (pull, push, and assign roles tooother users).</span></span>

<span data-ttu-id="4a3b2-118">Névtelen hozzáférés az Azure-tároló nyilvántartó nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-118">Anonymous access is not available on Azure Container Registries.</span></span> <span data-ttu-id="4a3b2-119">A nyilvános képek használhatják [Docker Hub](https://docs.docker.com/docker-hub/).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-119">For public images you can use [Docker Hub](https://docs.docker.com/docker-hub/).</span></span>

<span data-ttu-id="4a3b2-120">Több szolgáltatásnevekről tooa beállításkulcs, amely lehetővé teszi a különböző felhasználók vagy alkalmazások toodefine hozzáférési rendelhet hozzá.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-120">You can assign multiple service principals tooa registry, which allows you toodefine access for different users or applications.</span></span> <span data-ttu-id="4a3b2-121">Szolgáltatás rendszerbiztonsági tagoknak is engedélyezheti a "távfelügyeleti" kapcsolat tooa beállításjegyzék fejlesztői vagy DevOps forgatókönyvek használhatók, mint a következő példák hello:</span><span class="sxs-lookup"><span data-stu-id="4a3b2-121">Service principals also enable "headless" connectivity tooa registry in developer or DevOps scenarios such as hello following examples:</span></span>

  * <span data-ttu-id="4a3b2-122">A beállításjegyzék tooorchestration rendszerekből többek között a DC/OS, Docker Swarm és Kubernetes üzemelő tárolópéldányokat.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-122">Container deployments from a registry tooorchestration systems including DC/OS, Docker Swarm and Kubernetes.</span></span> <span data-ttu-id="4a3b2-123">Akkor is is lekéréses tároló nyilvántartó toorelated Azure-szolgáltatásokkal, mint [Tárolószolgáltatás](../container-service/index.yml), [App Service](../app-service/index.md), [kötegelt](../batch/index.md), [Service Fabric](/azure/service-fabric/), stb.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-123">You can also pull container registries toorelated Azure services such as [Container Service](../container-service/index.yml), [App Service](../app-service/index.md), [Batch](../batch/index.md), [Service Fabric](/azure/service-fabric/), and others.</span></span>

  * <span data-ttu-id="4a3b2-124">Folyamatos integrációt és telepítést megoldások (például a Visual Studio Team Services vagy Jenkins), amelyek tároló lemezképeket, és küldje le őket tooa beállításjegyzék.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-124">Continuous integration and deployment solutions (such as Visual Studio Team Services or Jenkins) that build container images and push them tooa registry.</span></span>





## <a name="admin-account"></a><span data-ttu-id="4a3b2-125">Rendszergazdai fiók</span><span class="sxs-lookup"><span data-stu-id="4a3b2-125">Admin account</span></span>
<span data-ttu-id="4a3b2-126">Az egyes beállításjegyzék hoz létre rendszergazdai fiók jön létre automatikusan.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-126">With each registry you create, an admin account gets created automatically.</span></span> <span data-ttu-id="4a3b2-127">Alapértelmezés szerint hello fiók le van tiltva, de engedélyezheti és beállíthatja hello hitelesítő adatok, például a hello [portal](container-registry-get-started-portal.md#manage-registry-settings) vagy hello segítségével [Azure CLI 2.0 parancsok](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-127">By default hello account is disabled, but you can enable it and manage hello credentials, for example through hello [portal](container-registry-get-started-portal.md#manage-registry-settings) or using hello [Azure CLI 2.0 commands](container-registry-get-started-azure-cli.md#manage-admin-credentials).</span></span> <span data-ttu-id="4a3b2-128">Minden rendszergazdai fiók kerül a két jelszavakkal, amelyek mindegyikét helyreállíthatja.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-128">Each admin account is provided with two passwords, both of which can be regenerated.</span></span> <span data-ttu-id="4a3b2-129">hello két jelszavak teszik toomaintain kapcsolatok toohello beállításjegyzék segítségével egy jelszó hello újragenerálja más jelszót.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-129">hello two passwords allow you toomaintain connections toohello registry by using one password while you regenerate hello other password.</span></span> <span data-ttu-id="4a3b2-130">Ha hello fiók engedélyezve van, hello felhasználói nevet és vagy jelszó toohello átadhatók `docker login` az egyszerű hitelesítés toohello beállításjegyzék parancsot.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-130">If hello account is enabled, you can pass hello user name and either password toohello `docker login` command for basic authentication toohello registry.</span></span> <span data-ttu-id="4a3b2-131">Példa:</span><span class="sxs-lookup"><span data-stu-id="4a3b2-131">For example:</span></span>

```
docker login myregistry.azurecr.io -u myAdminName -p myPassword1
```

> [!IMPORTANT]
> <span data-ttu-id="4a3b2-132">hello rendszergazdai fiókot egy felhasználói tooaccess hello beállításjegyzékbeli, főleg tesztelési célokra tervezték.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-132">hello admin account is designed for a single user tooaccess hello registry, mainly for test purposes.</span></span> <span data-ttu-id="4a3b2-133">Nem ajánlott tooshare hello rendszergazdai fiók hitelesítő adataival többi felhasználójával.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-133">It is not recommended tooshare hello admin account credentials among other users.</span></span> <span data-ttu-id="4a3b2-134">Minden felhasználó egyetlen felhasználó toohello beállításjegyzékbeli jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-134">All users appear as a single user toohello registry.</span></span> <span data-ttu-id="4a3b2-135">Módosítása vagy a fiók letiltása letiltja az összes felhasználó számára hello hitelesítő adatok használata a beállításjegyzék elérésének.</span><span class="sxs-lookup"><span data-stu-id="4a3b2-135">Changing or disabling this account disables registry access for all users who use hello credentials.</span></span>
>


### <a name="next-steps"></a><span data-ttu-id="4a3b2-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4a3b2-136">Next steps</span></span>
* <span data-ttu-id="4a3b2-137">[Az első kép hello Docker parancssori felület használatával leküldéses](container-registry-get-started-docker-cli.md).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-137">[Push your first image using hello Docker CLI](container-registry-get-started-docker-cli.md).</span></span>
* <span data-ttu-id="4a3b2-138">Hello tároló beállításjegyzék Preview hitelesítéssel kapcsolatos további információkért lásd: hello [blogbejegyzés](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span><span class="sxs-lookup"><span data-stu-id="4a3b2-138">For more information about authentication in hello Container Registry preview, see hello [blog post](https://blogs.msdn.microsoft.com/stevelasker/2016/11/17/azure-container-registry-user-accounts/).</span></span>
