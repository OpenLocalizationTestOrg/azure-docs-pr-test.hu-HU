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
> Erre a DC/OS-alapú ACS-fürtökkel végzett munka esetén van szükség. Nincs nincs szükség toodo a Swarm-alapú ACS-fürtökkel.
> 
> 

Első, [csatlakozzon a DC/OS-alapú ACS tooyour fürt](../articles/container-service/container-service-connect.md). Miután ezt megtette, telepítheti a DC/OS parancssori felület hello az ügyfélszámítógép az alábbi parancsok hello:

```bash
sudo pip install virtualenv
mkdir dcos && cd dcos
wget https://raw.githubusercontent.com/mesosphere/dcos-cli/master/bin/install/install-optout-dcos-cli.sh
chmod +x install-optout-dcos-cli.sh
./install-optout-dcos-cli.sh . http://localhost --add-path yes
```

Ha a Python egy régebbi verzióját használja, előfordulhat, hogy „InsecurePlatformWarnings” figyelmeztetésekbe ütközik. Ezek biztonságosan figyelmen kívül hagyhatók.

A parancskörnyezet újraindítása nélküli lépések sorrendben tooget futtassa:

```bash
source ~/.bashrc
```

Új parancskörnyezet indítása esetén ez a lépés elhagyható.

Most is meggyőződhet arról, hogy telepítve van a CLI hello:

```bash
dcos --help
```