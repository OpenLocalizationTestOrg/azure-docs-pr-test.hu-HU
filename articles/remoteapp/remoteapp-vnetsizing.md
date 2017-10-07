---
title: aaaSizing adatai egy VNETET az Azure Remoteappban |} Microsoft Docs
description: "Hello IP-cím követelmények a virtuális hálózaton futó Azure RemoteApp megismerése"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b6e1c4ba-0236-42b2-bced-69353bf211be
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: f98b831af32c41740b258d122b3e18765be08d97
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sizing-information-for-a-vnet-in-azure-remoteapp"></a>Egy VNETET az Azure Remoteappban méretezési információi
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Azure RemoteApp virtuális hálózatot (VNET) használja, használja a RemoteApp és az hello alhálózaton belüli IP-címek. A RemoteApp szolgáltatás hello méretezési, meg kell, hogy rendelkezik-e elegendő RemoteApp virtuális gépeknél érhető el IP-cím az alhálózat tooensure. A olvasható méretezési útmutató nem tökéletes megoldás adott hogyan forog fel dinamikusan a RemoteApp és forog le egy gyűjteményen belül virtuális gépeket, miközben nyújt segítséget az alhálózat tartományának becslése. Ez különösen fontos, mivel a RemoteApp szolgáltatás a VNETEN belül helyezkedik el, miután RemoteApp eltávolítása nélkül nem növelhető hello alhálózat méretét.

Mindegyik RemoteApp-gyűjteményt, amelyet az toorun maximális kapacitással 100 elérhető IP-címek kell rendelkeznie. Például ha hello Standard csomag egy RemoteApp-gyűjteménnyel rendelkezik, és azt szeretné, hogy toohave hello legfeljebb 500 felhasználónak, kell gyűjteménynek 100 IP-címet. Ehhez hasonlóan kell 100 IP-címek egy RemoteApp-gyűjtemény, amely rendelkezik 800 felhasználók alapszintű hello. Ha azt tervezi, toohave kevesebb felhasználók (kisebb maximális hello), csökkentheti a gyűjteményenként szükséges hello IP-címeket. hello alhálózati minimális méretkövetelményt 30 IP-címek (/ 27).

Tekintse meg a következő információk toomake meg arról, hogy a virtuális hálózat van konfigurálva, és működik-e propertly hello:

* [Személyes virtuális hálózat tooan Azure virtuális hálózat áttelepítése](remoteapp-migratevnet.md)
* [Az Azure RemoteApp hello Azure VNET toouse ellenőrzése](remoteapp-vnet.md)

