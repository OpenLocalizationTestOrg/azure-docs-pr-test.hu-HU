---
title: "aaaEnable vagy letiltását Azure VM figyelése"
description: "Ismerteti, hogyan tooEnable vagy tiltsa le a Azure VM figyelése"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 6ce366d2-bd4c-4fef-a8f5-a3ae2374abcc
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/08/2016
ms.author: kmouss
ms.openlocfilehash: 39c2211e4e5f3ad364513b52c5acc70c00b432fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-or-disable-azure-vm-monitoring"></a><span data-ttu-id="b7407-103">Engedélyezheti vagy tilthatja le, Azure virtuális gép figyelése</span><span class="sxs-lookup"><span data-stu-id="b7407-103">Enable or Disable Azure VM Monitoring</span></span>

<span data-ttu-id="b7407-104">Ez a szakasz ismerteti, hogyan tooenable vagy a figyelés letiltásakor a virtuális gépek Azure-on futó.</span><span class="sxs-lookup"><span data-stu-id="b7407-104">This section describes how tooenable or disable monitoring on Virtual machines running on Azure.</span></span> <span data-ttu-id="b7407-105">Engedélyezheti vagy letilthatja a figyelési hello portál vagy az Azure parancssori felület Mac, Linux és Windows (hello Azure CLI) használatával.</span><span class="sxs-lookup"><span data-stu-id="b7407-105">You can enable or disable monitoring using hello portal or Azure Command-line Interface for Mac, Linux, and Windows (hello Azure CLI).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b7407-106">Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény, amely elavult 2.3 verzióját.</span><span class="sxs-lookup"><span data-stu-id="b7407-106">This document describes version 2.3 of hello Linux Diagnostic Extension, which has been deprecated.</span></span> <span data-ttu-id="b7407-107">2.3 verziója lesz támogatott 2018. június 30-ig.</span><span class="sxs-lookup"><span data-stu-id="b7407-107">Version 2.3 will be supported until June 30, 2018.</span></span>
>
> <span data-ttu-id="b7407-108">Linux diagnosztikai bővítmény helyette engedélyezhető hello 3.0-s verziója.</span><span class="sxs-lookup"><span data-stu-id="b7407-108">Version 3.0 of hello Linux Diagnostic Extension can be enabled instead.</span></span> <span data-ttu-id="b7407-109">További információkért lásd: [dokumentáció hello](./diagnostic-extension.md).</span><span class="sxs-lookup"><span data-stu-id="b7407-109">For more information, see [hello documentation](./diagnostic-extension.md).</span></span>

## <a name="enable--disable-monitoring-through-hello-azure-portal"></a><span data-ttu-id="b7407-110">Engedélyezi / letiltja a figyelt hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="b7407-110">Enable / Disable Monitoring through hello Azure portal</span></span>

<span data-ttu-id="b7407-111">Az Azure virtuális gép, amely biztosít a példány adatait 1 perces időszakokban figyelését is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="b7407-111">You can enable  monitoring of your Azure VM, which provides data about your instance in 1-minute periods.</span></span> <span data-ttu-id="b7407-112">(a tároló módosításai érvényesek).</span><span class="sxs-lookup"><span data-stu-id="b7407-112">(storage changes apply).</span></span> <span data-ttu-id="b7407-113">Részletes diagnosztikai adatokat hello VM hello portál diagramjait vagy azon keresztül hello API majd érhető el.</span><span class="sxs-lookup"><span data-stu-id="b7407-113">Detailed diagnostics data is then available for hello VM in hello portal graphs or through hello API.</span></span> <span data-ttu-id="b7407-114">Alapértelmezés szerint az Azure portál lehetővé teszi a gazdagép alapú felügyelete, metrikák korlátozott számú.</span><span class="sxs-lookup"><span data-stu-id="b7407-114">By default, Azure portal enables host-based monitoring of a limited set of metrics.</span></span> <span data-ttu-id="b7407-115">Engedélyezheti, hogy egy virtuális Gépet, miközben hello virtuális gép fut vagy leállított állapotban belül a metrikák figyelését.</span><span class="sxs-lookup"><span data-stu-id="b7407-115">You can enable monitoring of metrics from within a VM while hello VM is running or in stopped state.</span></span>

* <span data-ttu-id="b7407-116">Nyissa meg hello Azure portál, [https://portal.azure.com](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b7407-116">Open hello Azure portal at [https://portal.azure.com](https://portal.azure.com).</span></span>
* <span data-ttu-id="b7407-117">A bal oldali navigációs hello kattintson a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="b7407-117">In hello left navigation, click Virtual machines.</span></span>
* <span data-ttu-id="b7407-118">Hello lista virtuális gépeken válasszon ki egy fut vagy leállt példányt.</span><span class="sxs-lookup"><span data-stu-id="b7407-118">In hello list Virtual machines, select a running or stopped instance.</span></span> <span data-ttu-id="b7407-119">hello "Virtuális gép" panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b7407-119">hello "Virtual machine" blade opens.</span></span>
* <span data-ttu-id="b7407-120">Kattintson az összes beállítást.</span><span class="sxs-lookup"><span data-stu-id="b7407-120">Click All settings.</span></span>
* <span data-ttu-id="b7407-121">Kattintson a diagnosztika.</span><span class="sxs-lookup"><span data-stu-id="b7407-121">Click Diagnostics.</span></span>
* <span data-ttu-id="b7407-122">Állapot tooOn módosítása vagy ki.</span><span class="sxs-lookup"><span data-stu-id="b7407-122">Change status tooOn or Off.</span></span> <span data-ttu-id="b7407-123">Ezen a panelen hello szinten tooenable szeretné a virtuális gép részletei figyelést is kiválaszthat.</span><span class="sxs-lookup"><span data-stu-id="b7407-123">You can also pick in this blade hello level of monitoring details you would like tooenable for your virtual machine.</span></span>

![Engedélyezi / letiltja a figyelés hello Azure-portálon keresztül.][1]

## <a name="enable--disable-monitoring-with-azure-cli"></a><span data-ttu-id="b7407-125">Engedélyezi / letiltja a megfigyelés az Azure parancssori felület</span><span class="sxs-lookup"><span data-stu-id="b7407-125">Enable / Disable Monitoring with Azure CLI</span></span>

<span data-ttu-id="b7407-126">egy Azure virtuális gép tooenable figyelését.</span><span class="sxs-lookup"><span data-stu-id="b7407-126">tooenable monitoring for an Azure VM.</span></span>

* <span data-ttu-id="b7407-127">Hozzon létre egy fájlt (például PrivateConfig.json):</span><span class="sxs-lookup"><span data-stu-id="b7407-127">Create a file (named such as PrivateConfig.json):</span></span>

```json
{
        "storageAccountName":"hello storage account tooreceive data",
        "storageAccountKey":"hello key of hello account"
}
```

* <span data-ttu-id="b7407-128">Azure parancssori felületen keresztül hello bővítmény engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b7407-128">Enable hello extension via Azure CLI.</span></span>

```azurecli
azure vm extension set myvm LinuxDiagnostic Microsoft.OSTCExtensions 2.3 --private-config-path PrivateConfig.json
```

<span data-ttu-id="b7407-129">További információkért lásd: [Linux diagnosztikai bővítmény használatával tooMonitor Linux virtuális gép teljesítmény- és diagnosztikai adatokat](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b7407-129">For more information, see [Using Linux Diagnostic Extension tooMonitor Linux VM’s performance and diagnostic data](classic/diagnostic-extension-v2.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).</span></span>

<!--Image references-->
[1]: ./media/vm-monitoring/portal-enable-disable.png
