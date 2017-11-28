---
title: "A DC/OS parancssori felület telepítése | Microsoft Docs"
description: "A DC/OS parancssori felület telepítése."
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
ms.openlocfilehash: a8ea47f158c0d666340815d2e039995c7483257f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
> [!NOTE]
> <span data-ttu-id="751b8-104">Erre a DC/OS-alapú ACS-fürtökkel végzett munka esetén van szükség.</span><span class="sxs-lookup"><span data-stu-id="751b8-104">This is for working with DC/OS-based ACS clusters.</span></span> <span data-ttu-id="751b8-105">A Swarm-alapú ACS-fürtök esetén erre a lépésre nincs szükség.</span><span class="sxs-lookup"><span data-stu-id="751b8-105">There is no need to do this for Swarm-based ACS clusters.</span></span>
> 
> 

<span data-ttu-id="751b8-106">Először is, [csatlakozzon a DC/OS-alapú ACS-fürthöz](../articles/container-service/container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="751b8-106">First, [connect to your DC/OS-based ACS cluster](../articles/container-service/container-service-connect.md).</span></span> <span data-ttu-id="751b8-107">Ezután az alábbi parancsok használatával telepítheti a DC/OS parancssori felületet az ügyfélgépre:</span><span class="sxs-lookup"><span data-stu-id="751b8-107">Once you have done this, you can install the DC/OS CLI on your client machine with the commands below:</span></span>

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

<span data-ttu-id="751b8-108">Ha a Python egy régebbi verzióját használja, előfordulhat, hogy „InsecurePlatformWarnings” figyelmeztetésekbe ütközik.</span><span class="sxs-lookup"><span data-stu-id="751b8-108">If you are using an old version of Python, you may notice some "InsecurePlatformWarnings".</span></span> <span data-ttu-id="751b8-109">Ezek biztonságosan figyelmen kívül hagyhatók.</span><span class="sxs-lookup"><span data-stu-id="751b8-109">You can safely ignore these.</span></span>

<span data-ttu-id="751b8-110">A parancskörnyezet újraindítása nélküli indításhoz futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="751b8-110">In order to get started without restarting your shell, run:</span></span>

```bash
source ~/.bashrc
```

<span data-ttu-id="751b8-111">Új parancskörnyezet indítása esetén ez a lépés elhagyható.</span><span class="sxs-lookup"><span data-stu-id="751b8-111">This step will not be necessary when you start new shells.</span></span>

<span data-ttu-id="751b8-112">Most meggyőződhet arról, hogy a parancssori felület telepítve van:</span><span class="sxs-lookup"><span data-stu-id="751b8-112">Now you can confirm that the CLI is installed:</span></span>

```bash
dcos --help
```