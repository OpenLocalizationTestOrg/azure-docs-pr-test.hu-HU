---
title: "Linux virtuális gép aaaTroubleshooting pufferallokációs hibák |} Microsoft Docs"
description: "Hozzon létre, újraindításakor vagy átméretezésekor egy Linux virtuális Gépet az Azure-ban fellépő lefoglalási hibák elhárítása"
services: virtual-machines-linux, azure-resource-manager
documentationcenter: 
author: JiangChen79
manager: felixwu
editor: 
tags: top-support-issue,azure-resourece-manager,azure-service-management
ms.assetid: 1ef41144-6dd6-4a56-b180-9d8b3d05eae7
ms.service: virtual-machines-linux
ms.workload: na
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cjiang
ms.openlocfilehash: 502fbb406b0b4acf086c2586795f69a44cc1a004
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-allocation-failures-when-you-create-restart-or-resize-linux-vms-in-azure"></a>Hozzon létre, újraindításakor vagy átméretezésekor Linux virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása
Hozzon létre egy virtuális Gépet, indítsa újra a leállított (felszabadított) virtuális gépek vagy átméretezni egy virtuális Gépet, a Microsoft Azure számítási erőforrásokat tooyour előfizetés foglal le. Alkalmanként jelenhet hibák ezeket a műveleteket--még mielőtt hello Azure előfizetési korlátozásait. Ez a cikk ismerteti a hello okok egyes hello közös pufferallokációs hibák, és lehetséges javítási javasol. hello információkat is lehetnek hasznosak, ha azt tervezi, hogy a szolgáltatások hello telepítését. Emellett [létrehozása, újraindításakor vagy átméretezésekor Windows-alapú virtuális gépek Azure-ban fellépő lefoglalási hibák elhárítása](../windows/allocation-failure.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

[!INCLUDE [virtual-machines-common-allocation-failure](../../../includes/virtual-machines-common-allocation-failure.md)]

