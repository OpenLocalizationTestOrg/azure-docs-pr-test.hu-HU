---
title: "DC/OS parancssori felület aaaInstall hello |} Microsoft Docs"
description: "Hello DC/OS parancssori felület telepítése."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, mikroszolgáltatások, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/10/2016
ms.author: rogardle
ms.openlocfilehash: b077c05beff9a5638486ea5efe9df31089e32701
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
> [!NOTE]
> <span data-ttu-id="6e702-104">Erre a DC/OS-alapú ACS-fürtökkel végzett munka esetén van szükség.</span><span class="sxs-lookup"><span data-stu-id="6e702-104">This is for working with DC/OS-based ACS clusters.</span></span> <span data-ttu-id="6e702-105">Nincs nincs szükség toodo a Swarm-alapú ACS-fürtökkel.</span><span class="sxs-lookup"><span data-stu-id="6e702-105">There is no need toodo this for Swarm-based ACS clusters.</span></span>
> 
> 

<span data-ttu-id="6e702-106">Első, [csatlakozzon a DC/OS-alapú ACS tooyour fürt](../articles/container-service/container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="6e702-106">First, [connect tooyour DC/OS-based ACS cluster](../articles/container-service/container-service-connect.md).</span></span> <span data-ttu-id="6e702-107">Miután ezt megtette, telepítheti a DC/OS parancssori felület hello az ügyfélszámítógép az alábbi parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="6e702-107">Once you have done this, you can install hello DC/OS CLI on your client machine with hello commands below:</span></span>

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

<span data-ttu-id="6e702-108">Ha a Python egy régebbi verzióját használja, előfordulhat, hogy „InsecurePlatformWarnings” figyelmeztetésekbe ütközik.</span><span class="sxs-lookup"><span data-stu-id="6e702-108">If you are using an old version of Python, you may notice some "InsecurePlatformWarnings".</span></span> <span data-ttu-id="6e702-109">Ezek biztonságosan figyelmen kívül hagyhatók.</span><span class="sxs-lookup"><span data-stu-id="6e702-109">You can safely ignore these.</span></span>

<span data-ttu-id="6e702-110">A parancskörnyezet újraindítása nélküli lépések sorrendben tooget futtassa:</span><span class="sxs-lookup"><span data-stu-id="6e702-110">In order tooget started without restarting your shell, run:</span></span>

```bash
source ~/.bashrc
```

<span data-ttu-id="6e702-111">Új parancskörnyezet indítása esetén ez a lépés elhagyható.</span><span class="sxs-lookup"><span data-stu-id="6e702-111">This step will not be necessary when you start new shells.</span></span>

<span data-ttu-id="6e702-112">Most is meggyőződhet arról, hogy telepítve van a CLI hello:</span><span class="sxs-lookup"><span data-stu-id="6e702-112">Now you can confirm that hello CLI is installed:</span></span>

```bash
dcos --help
```