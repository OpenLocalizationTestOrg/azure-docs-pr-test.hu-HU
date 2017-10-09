---
title: "aaaAzure felhő rendszerhéj (előzetes verzió) gyors üzembe helyezés |} Microsoft Docs"
description: "Az Azure felhőalapú rendszerhéj hello gyors üzembe helyezés"
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: juluk
ms.openlocfilehash: e60700b92c10c331910dd8bb3c627fe1a024091c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-using-hello-azure-cloud-shell"></a><span data-ttu-id="f1fb5-103">Gyors üzembe helyezés a hello Azure Cloud rendszerhéj használatával</span><span class="sxs-lookup"><span data-stu-id="f1fb5-103">Quickstart for using hello Azure Cloud Shell</span></span>

<span data-ttu-id="f1fb5-104">Ez a dokumentum részletesen hogyan toouse hello hello az Azure felhőalapú rendszerhéj [Azure-portálon](https://ms.portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-104">This document details how toouse hello Azure Cloud Shell in hello [Azure portal](https://ms.portal.azure.com/).</span></span>

## <a name="start-cloud-shell"></a><span data-ttu-id="f1fb5-105">Indítsa el a felhő rendszerhéj</span><span class="sxs-lookup"><span data-stu-id="f1fb5-105">Start Cloud Shell</span></span>
1. <span data-ttu-id="f1fb5-106">Indítsa el **felhő rendszerhéj** hello felső navigációs sávon a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="f1fb5-106">Launch **Cloud Shell** from hello top navigation of hello Azure portal</span></span> <br>
![](media/shell-icon.png)
2. <span data-ttu-id="f1fb5-107">Válasszon ki egy előfizetés toocreate egy tárfiókot és az Azure-fájlmegosztáshoz</span><span class="sxs-lookup"><span data-stu-id="f1fb5-107">Select a subscription toocreate a storage account and Azure file share</span></span>
3. <span data-ttu-id="f1fb5-108">Válassza a "Create a storage"</span><span class="sxs-lookup"><span data-stu-id="f1fb5-108">Select "Create storage"</span></span>

> [!TIP]
> <span data-ttu-id="f1fb5-109">Akkor automatikusan megtörténik az Azure CLI 2.0 minden munkamenetet a.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-109">You are automatically authenticated for Azure CLI 2.0 in every sesssion.</span></span>

### <a name="set-your-subscription"></a><span data-ttu-id="f1fb5-110">Az előfizetés beállítása</span><span class="sxs-lookup"><span data-stu-id="f1fb5-110">Set your subscription</span></span>
1. <span data-ttu-id="f1fb5-111">Lista előfizetések rendelkezik hozzáféréssel:</span><span class="sxs-lookup"><span data-stu-id="f1fb5-111">List subscriptions you have access to:</span></span> <br>
`az account list`
2. <span data-ttu-id="f1fb5-112">Állítsa be az előnyben részesített előfizetés:</span><span class="sxs-lookup"><span data-stu-id="f1fb5-112">Set your preferred subscription:</span></span> <br>
`az account set --subscription my-subscription-name`

> [!TIP]
> <span data-ttu-id="f1fb5-113">A rendszer tárolja a későbbi kapcsolatok használata az előfizetés `/home/<user>/.azure/azureProfile.json`.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-113">Your subscription will be remembered for future sessions using `/home/<user>/.azure/azureProfile.json`.</span></span>

### <a name="create-a-resource-group"></a><span data-ttu-id="f1fb5-114">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="f1fb5-114">Create a resource group</span></span>
<span data-ttu-id="f1fb5-115">Hozzon létre egy új erőforráscsoportot "MyRG" nevű WestUS:</span><span class="sxs-lookup"><span data-stu-id="f1fb5-115">Create a new resource group in WestUS named "MyRG":</span></span> <br>
`az group create -l westus -n MyRG` <br>

### <a name="create-a-linux-vm"></a><span data-ttu-id="f1fb5-116">Linux rendszerű virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="f1fb5-116">Create a Linux VM</span></span>
<span data-ttu-id="f1fb5-117">Ubuntu virtuális gép létrehozása az új erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-117">Create an Ubuntu VM in your new resource group.</span></span> <span data-ttu-id="f1fb5-118">hello Azure CLI 2.0 hoz létre SSH-kulcsok és a telepítő hello VM őket.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-118">hello Azure CLI 2.0 will create SSH keys and setup hello VM with them.</span></span> <br>
`az vm create -n my_vm_name -g MyRG --image UbuntuLTS --generate-ssh-keys`

> [!NOTE]
> <span data-ttu-id="f1fb5-119">a virtuális gép kerülnek használt nyilvános és titkos kulcsok tooauthenticate hello `/User/.ssh/id_rsa` és `/User/.ssh/id_rsa.pub` Azure CLI 2.0 alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-119">hello public and private keys used tooauthenticate your VM are placed in `/User/.ssh/id_rsa` and `/User/.ssh/id_rsa.pub` by Azure CLI 2.0 by default.</span></span> <span data-ttu-id="f1fb5-120">Az .ssh mappa a csatolt Azure fájlmegosztás 5 GB-os kép megőrződjenek.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-120">Your .ssh folder is persisted in your attached Azure file share's 5-GB image.</span></span>

<span data-ttu-id="f1fb5-121">A felhasználónév, a virtuális gép lesz a felhő rendszerhéj használt felhasználónév ($User@Azure:).</span><span class="sxs-lookup"><span data-stu-id="f1fb5-121">Your username on this VM will be your username used in Cloud Shell ($User@Azure:).</span></span>

### <a name="ssh-into-your-linux-vm"></a><span data-ttu-id="f1fb5-122">SSH a Linux virtuális gép be</span><span class="sxs-lookup"><span data-stu-id="f1fb5-122">SSH into your Linux VM</span></span>
1. <span data-ttu-id="f1fb5-123">Keresse meg a virtuális gép nevét, az Azure portál keresősávban hello</span><span class="sxs-lookup"><span data-stu-id="f1fb5-123">Search for your VM name in hello Azure portal search bar</span></span>
2. <span data-ttu-id="f1fb5-124">Kattintson a "Csatlakozás" gombra, és futtassa:`ssh username@ipaddress`</span><span class="sxs-lookup"><span data-stu-id="f1fb5-124">Click "Connect" and run: `ssh username@ipaddress`</span></span>

![](media/sshcmd-copy.png)

<span data-ttu-id="f1fb5-125">Hello SSH-kapcsolatot létesít, akkor meg kell jelennie hello Ubuntu üdvözli a parancssorba.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-125">Upon establishing hello SSH connection, you should see hello Ubuntu welcome prompt.</span></span>
![](media/ubuntu-welcome.png)

## <a name="cleaning-up"></a><span data-ttu-id="f1fb5-126">Takarítás</span><span class="sxs-lookup"><span data-stu-id="f1fb5-126">Cleaning up</span></span> 
<span data-ttu-id="f1fb5-127">Az erőforráscsoport és bármely erőforrása törlése:</span><span class="sxs-lookup"><span data-stu-id="f1fb5-127">Delete your resource group and any resources within it:</span></span> <br>
<span data-ttu-id="f1fb5-128">Futtassa a `az group delete -n MyRG` parancsot.</span><span class="sxs-lookup"><span data-stu-id="f1fb5-128">Run `az group delete -n MyRG`</span></span>

## <a name="next-steps"></a><span data-ttu-id="f1fb5-129">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f1fb5-129">Next Steps</span></span>
[<span data-ttu-id="f1fb5-130">A felhő rendszerhéj tárolásakor tárolás</span><span class="sxs-lookup"><span data-stu-id="f1fb5-130">Learn about persisting storage on Cloud Shell</span></span>](persisting-shell-storage.md) <br><span data-ttu-id="f1fb5-131">
[További tudnivalók az Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-131">
[Learn about Azure CLI 2.0](https://docs.microsoft.com/cli/azure/)</span></span> <br><span data-ttu-id="f1fb5-132">
[További tudnivalók az Azure File storage](../storage/files/storage-files-introduction.md)</span><span class="sxs-lookup"><span data-stu-id="f1fb5-132">
[Learn about Azure File storage](../storage/files/storage-files-introduction.md)</span></span> <br>