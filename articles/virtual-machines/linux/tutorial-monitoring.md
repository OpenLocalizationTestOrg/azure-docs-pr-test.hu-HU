---
title: "aaaMonitor Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor rendszerindítási diagnosztika és a metrikák egy Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a><span data-ttu-id="6fb2c-103">Hogyan toomonitor egy Linux virtuális gép az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6fb2c-103">How toomonitor a Linux virtual machine in Azure</span></span>

<span data-ttu-id="6fb2c-104">tooensure megfelelően fut a virtuális gépek (VM) az Azure-ban, a rendszerindítási diagnosztika és a metrikák tudja ellenőrizni.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-104">tooensure your virtual machines (VMs) in Azure are running correctly, you can review boot diagnostics and performance metrics.</span></span> <span data-ttu-id="6fb2c-105">Eben az oktatóanyagban az alábbiakkal fog megismerkedni:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fb2c-106">Rendszerindítási diagnosztika a virtuális gép hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-106">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="6fb2c-107">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-107">View boot diagnostics</span></span>
> * <span data-ttu-id="6fb2c-108">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-108">View host metrics</span></span>
> * <span data-ttu-id="6fb2c-109">A virtuális gép hello diagnosztika bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-109">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="6fb2c-110">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="6fb2c-110">View VM metrics</span></span>
> * <span data-ttu-id="6fb2c-111">A diagnosztikai mérőszámok alapján figyelmeztetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-111">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="6fb2c-112">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-112">Set up advanced monitoring</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="6fb2c-113">Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-113">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="6fb2c-114">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="6fb2c-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-vm"></a><span data-ttu-id="6fb2c-116">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-116">Create VM</span></span>

<span data-ttu-id="6fb2c-117">toosee diagnosztika és a művelet a metrikák kell egy virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-117">toosee diagnostics and metrics in action, you need a VM.</span></span> <span data-ttu-id="6fb2c-118">Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-118">First, create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="6fb2c-119">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupMonitor* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-119">hello following example creates a resource group named *myResourceGroupMonitor* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

<span data-ttu-id="6fb2c-120">Most létrehozza a virtuális gép és [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-120">Now create a VM with [az vm create](https://docs.microsoft.com/cli/azure/vm#create).</span></span> <span data-ttu-id="6fb2c-121">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-121">hello following example creates a VM named *myVM*:</span></span>

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a><span data-ttu-id="6fb2c-122">Rendszerindítási diagnosztika engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-122">Enable boot diagnostics</span></span>

<span data-ttu-id="6fb2c-123">Linux virtuális gépek rendszerindító, mivel a hello rendszerindítási diagnosztikai bővítmény rendszerindító kimeneti rögzíti, és azt az Azure storage tárolja.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-123">As Linux VMs boot, hello boot diagnostic extension captures boot output and stores it in Azure storage.</span></span> <span data-ttu-id="6fb2c-124">Ezek az adatok használt tootroubleshoot virtuális gép rendszerindítási problémák lehetnek.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-124">This data can be used tootroubleshoot VM boot issues.</span></span> <span data-ttu-id="6fb2c-125">Rendszerindítási diagnosztika nem automatikusan engedélyezve lesznek hello Azure CLI használata Linux virtuális gép létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-125">Boot diagnostics are not automatically enabled when you create a Linux VM using hello Azure CLI.</span></span>

<span data-ttu-id="6fb2c-126">Ahhoz, hogy a rendszerindítási diagnosztika, egy tárfiókot meg kell toobe létrehozott rendszerindító naplók tárolásához.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-126">Before enabling boot diagnostics, a storage account needs toobe created for storing boot logs.</span></span> <span data-ttu-id="6fb2c-127">Storage-fiókok 3 és 24 karakter között lehet globálisan egyedi névvel kell rendelkeznie, és csak számokat és kisbetűket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-127">Storage accounts must have a globally unique name, be between 3 and 24 characters, and must contain only numbers and lowercase letters.</span></span> <span data-ttu-id="6fb2c-128">Hozzon létre egy tárfiókot hello [az storage-fiók létrehozása](/cli/azure/storage/account#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-128">Create a storage account with hello [az storage account create](/cli/azure/storage/account#create) command.</span></span> <span data-ttu-id="6fb2c-129">Ebben a példában a véletlenszerű karakterlánc használt toocreate egy egyedi tárfiók neve.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-129">In this example, a random string is used toocreate a unique storage account name.</span></span> 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

<span data-ttu-id="6fb2c-130">Ha engedélyezve van a rendszerindítási diagnosztika, hello URI toohello blob storage tárolóban van szükség.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-130">When enabling boot diagnostics, hello URI toohello blob storage container is needed.</span></span> <span data-ttu-id="6fb2c-131">hello következő parancs lekérdezések hello tárolási fiók tooreturn ezt az URI.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-131">hello following command queries hello storage account tooreturn this URI.</span></span> <span data-ttu-id="6fb2c-132">hello URI azonosítóját tárolja a változók nevében *bloburi*, amellyel hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-132">hello URI value is stored in a variable names *bloburi*, which is used in hello next step.</span></span>

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

<span data-ttu-id="6fb2c-133">Most már engedélyezheti a rendszerindítási diagnosztika a [az virtuális gép rendszerindítási-diagnosztika engedélyezése](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-133">Now enable boot diagnostics with [az vm boot-diagnostics enable](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable).</span></span> <span data-ttu-id="6fb2c-134">Hello `--storage` értéke hello blob URI gyűjtött hello előző lépésben.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-134">hello `--storage` value is hello blob URI collected in hello previous step.</span></span>

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a><span data-ttu-id="6fb2c-135">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-135">View boot diagnostics</span></span>

<span data-ttu-id="6fb2c-136">Rendszerindítási diagnosztika, ha engedélyezve vannak minden alkalommal, állítsa le és indítsa el a virtuális gép, hello hello rendszerindítási folyamat információ íródik tooa naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-136">When boot diagnostics are enabled, each time you stop and start hello VM, information about hello boot process is written tooa log file.</span></span> <span data-ttu-id="6fb2c-137">Ebben a példában az első felszabadítani a hello a virtuális gép hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate) parancsot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-137">For this example, first deallocate hello VM with hello [az vm deallocate](/cli/azure/vm#deallocate) command as follows:</span></span>

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="6fb2c-138">Most hello VM kezdődnie hello [az vm indítása]( /cli/azure/vm#stop) parancsot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-138">Now start hello VM with hello [az vm start]( /cli/azure/vm#stop) command as follows:</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

<span data-ttu-id="6fb2c-139">Hello rendszerindítási diagnosztikai adatokat a kaphat *myVM* hello a [az virtuális gép rendszerindítási-diagnosztika get-rendszerindítási-naplófájl](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) parancsot a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-139">You can get hello boot diagnostic data for *myVM* with hello [az vm boot-diagnostics get-boot-log](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) command as follows:</span></span>

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a><span data-ttu-id="6fb2c-140">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-140">View host metrics</span></span>

<span data-ttu-id="6fb2c-141">A Linux virtuális gépek dedikált-állomással rendelkezik, amely hatással van az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-141">A Linux VM has a dedicated host in Azure that it interacts with.</span></span> <span data-ttu-id="6fb2c-142">Metrikák automatikusan begyűjtött hello állomás, és megtekinthetők az Azure-portálon hello az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-142">Metrics are automatically collected for hello host and can be viewed in hello Azure portal as follows:</span></span>

1. <span data-ttu-id="6fb2c-143">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroupMonitor**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-143">In hello Azure portal, click **Resource Groups**, select **myResourceGroupMonitor**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="6fb2c-144">toosee hogyan működik-e hello állomást a virtuális Gépet, kattintson a **metrikák** hello VM panelen, majd válassza ki valamelyik hello *[állomás]* mérőszámok alapján **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-144">toosee how hello host VM is performing, click **Metrics** on hello VM blade, then select any of hello *[Host]* metrics under **Available metrics**.</span></span>

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a><span data-ttu-id="6fb2c-146">Diagnosztika-kiterjesztés telepítése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-146">Install diagnostics extension</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6fb2c-147">Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény, amely elavult 2.3 verzióját.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-147">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="6fb2c-148">2.3 verziója lesz támogatott 2018. június 30-ig.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-148">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="6fb2c-149">Linux diagnosztikai bővítmény helyette engedélyezhető hello 3.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-149">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="6fb2c-150">További információkért lásd: [dokumentáció hello](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-150">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

<span data-ttu-id="6fb2c-151">hello alapvető gazdagép metrikák érhetők el, de a részletesebb toosee, és a Virtuálisgép-specifikus metrika, akkor tooneed tooinstall hello Azure diagnostics bővítményt a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-151">hello basic host metrics are available, but toosee more granular and VM-specific metrics, you tooneed tooinstall hello Azure diagnostics extension on hello VM.</span></span> <span data-ttu-id="6fb2c-152">hello Azure diagnostics bővítmény lehetővé teszi, hogy további felügyeleti és diagnosztikai adatok toobe hello VM lekért.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-152">hello Azure diagnostics extension allows additional monitoring and diagnostics data toobe retrieved from hello VM.</span></span> <span data-ttu-id="6fb2c-153">A metrikák megtekintheti, és hozzon létre a riasztások alapján hogyan hello VM hajt végre.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-153">You can view these performance metrics and create alerts based on how hello VM performs.</span></span> <span data-ttu-id="6fb2c-154">hello diagnosztikai telepítve van a bővítmény hello Azure-portálon keresztül az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-154">hello diagnostic extension is installed through hello Azure portal as follows:</span></span>

1. <span data-ttu-id="6fb2c-155">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-155">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="6fb2c-156">Kattintson a **diagnosztikai beállítások**.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-156">Click **Diagnosis settings**.</span></span> <span data-ttu-id="6fb2c-157">hello lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakasz hello.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-157">hello list shows that *Boot diagnostics* are already enabled from hello previous section.</span></span> <span data-ttu-id="6fb2c-158">Kattintson a megfelelő jelölőnégyzet hello *alapvető metrikák*.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-158">Click hello check box for *Basic metrics*.</span></span>
1. <span data-ttu-id="6fb2c-159">A hello *tárfiók* területen válassza hello tooand Tallózás *mydiagdata [1234]* hello előző szakaszban létrehozott fiók.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-159">In hello *Storage account* section, browse tooand select hello *mydiagdata[1234]* account created in hello previous section.</span></span>
1. <span data-ttu-id="6fb2c-160">Kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-160">Click hello **Save** button.</span></span>

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a><span data-ttu-id="6fb2c-162">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="6fb2c-162">View VM metrics</span></span>

<span data-ttu-id="6fb2c-163">Hello VM metrikák hello megtekintheti, hogy megtekinthetők-e hello gazdagép VM metrikák ugyanúgy:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-163">You can view hello VM metrics in hello same way that you viewed hello host VM metrics:</span></span>

1. <span data-ttu-id="6fb2c-164">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-164">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
1. <span data-ttu-id="6fb2c-165">toosee hogyan hello VM hajt végre, kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello diagnosztika mérőszámok alapján **elérhető**.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-165">toosee how hello VM is performing, click **Metrics** on hello VM blade, and then select any of hello diagnostics metrics under **Available metrics**.</span></span>

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a><span data-ttu-id="6fb2c-167">Riasztások létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-167">Create alerts</span></span>

<span data-ttu-id="6fb2c-168">Riasztások adott mérőszámok alapján hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-168">You can create alerts based on specific performance metrics.</span></span> <span data-ttu-id="6fb2c-169">Riasztás például akkor, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet egy adott értékre, alá süllyed használt toonotify.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-169">Alerts can be used toonotify you when average CPU usage exceeds a certain threshold or available free disk space drops below a certain amount, for example.</span></span> <span data-ttu-id="6fb2c-170">Riasztások jelennek meg hello Azure-portálon, vagy e-mailben küldhetők el.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-170">Alerts are displayed in hello Azure portal or can be sent via email.</span></span> <span data-ttu-id="6fb2c-171">A válasz tooalerts generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is kiválthatják.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-171">You can also trigger Azure Automation runbooks or Azure Logic Apps in response tooalerts being generated.</span></span>

<span data-ttu-id="6fb2c-172">hello alábbi példa riasztást hoz létre az átlagos CPU-használat.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-172">hello following example creates an alert for average CPU usage.</span></span>

1. <span data-ttu-id="6fb2c-173">Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-173">In hello Azure portal, click **Resource Groups**, select **myResourceGroup**, and then select **myVM** in hello resource list.</span></span>
2. <span data-ttu-id="6fb2c-174">Kattintson a **riasztási szabályok** hello VM panelen, majd kattintson a **metrika riasztás hozzáadása** keresztül hello hello riasztások panel tetején.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-174">Click **Alert rules** on hello VM blade, then click **Add metric alert** across hello top of hello alerts blade.</span></span>
4. <span data-ttu-id="6fb2c-175">Adjon meg egy **neve** a riasztás például *myAlertRule*</span><span class="sxs-lookup"><span data-stu-id="6fb2c-175">Provide a **Name** for your alert, such as *myAlertRule*</span></span>
5. <span data-ttu-id="6fb2c-176">tootrigger riasztást, amikor a processzor 1.0 meghaladja 5 percig, hagyja összes hello többi kijelölt alapértelmezett értéket.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-176">tootrigger an alert when CPU percentage exceeds 1.0 for five minutes, leave all hello other defaults selected.</span></span>
6. <span data-ttu-id="6fb2c-177">Szükség esetén jelölje be a hello jelölőnégyzetet *E-mail-tulajdonosok, közreműködőknek és olvasóknak* toosend e-mailben értesítést.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-177">Optionally, check hello box for *Email owners, contributors, and readers* toosend email notification.</span></span> <span data-ttu-id="6fb2c-178">hello alapértelmezett művelete toopresent hello portálon értesítést.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-178">hello default action is toopresent a notification in hello portal.</span></span>
7. <span data-ttu-id="6fb2c-179">Kattintson a hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-179">Click hello **OK** button.</span></span>


## <a name="advanced-monitoring"></a><span data-ttu-id="6fb2c-180">Speciális figyelés</span><span class="sxs-lookup"><span data-stu-id="6fb2c-180">Advanced monitoring</span></span> 

<span data-ttu-id="6fb2c-181">Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span><span class="sxs-lookup"><span data-stu-id="6fb2c-181">You can do more advanced monitoring of your VM by using [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview).</span></span> <span data-ttu-id="6fb2c-182">Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-182">If you haven't already done so, you can sign up for a [free trial](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) of Operations Management Suite.</span></span>

<span data-ttu-id="6fb2c-183">Ha a hozzáférési toohello OMS-portálon, hello munkaterület kulcs és a munkaterület-azonosítót a hello-beállítások panelen található.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-183">When you have access toohello OMS portal, you can find hello workspace key and workspace identifier on hello Settings blade.</span></span> <span data-ttu-id="6fb2c-184">A név felülírandó < munkaterület-kulcs > és < munkaterület-azonosítója > értékekkel hello az OMS a munkaterületet, és ezután használhatja **az virtuálisgép-bővítmény készlet** tooadd hello OMS bővítmény toohello VM:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-184">Replace <workspace-key> and <workspace-id> with hello values for from your OMS workspace and then you can use **az vm extension set** tooadd hello OMS extension toohello VM:</span></span>

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

<span data-ttu-id="6fb2c-185">Hello OMS-portálon hello napló keresése paneljén láthatja *myVM* például hello az alábbi képen látható:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-185">On hello Log Search blade of hello OMS portal, you should see *myVM* such as what is shown in hello following picture:</span></span>

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a><span data-ttu-id="6fb2c-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6fb2c-187">Next steps</span></span>

<span data-ttu-id="6fb2c-188">Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-188">In this tutorial, you configured and reviewed VMs with Azure Security Center.</span></span> <span data-ttu-id="6fb2c-189">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="6fb2c-189">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="6fb2c-190">Rendszerindítási diagnosztika a virtuális gép hello engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-190">Enable boot diagnostics on hello VM</span></span>
> * <span data-ttu-id="6fb2c-191">Rendszerindítási diagnosztika megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-191">View boot diagnostics</span></span>
> * <span data-ttu-id="6fb2c-192">Gazdagép-metrikák megtekintése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-192">View host metrics</span></span>
> * <span data-ttu-id="6fb2c-193">A virtuális gép hello diagnosztika bővítmény engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-193">Enable diagnostics extension on hello VM</span></span>
> * <span data-ttu-id="6fb2c-194">Nézet VM metrikák</span><span class="sxs-lookup"><span data-stu-id="6fb2c-194">View VM metrics</span></span>
> * <span data-ttu-id="6fb2c-195">A diagnosztikai mérőszámok alapján figyelmeztetések létrehozása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-195">Create alerts based on diagnostic metrics</span></span>
> * <span data-ttu-id="6fb2c-196">Speciális figyelés beállítása</span><span class="sxs-lookup"><span data-stu-id="6fb2c-196">Set up advanced monitoring</span></span>

<span data-ttu-id="6fb2c-197">Az Azure security Centerrel kapcsolatos útmutató toolearn következő toohello előzetes.</span><span class="sxs-lookup"><span data-stu-id="6fb2c-197">Advance toohello next tutorial toolearn about Azure security center.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="6fb2c-198">A virtuális gépek biztonságának kezelése</span><span class="sxs-lookup"><span data-stu-id="6fb2c-198">Manage VM security</span></span>](./tutorial-azure-security.md)