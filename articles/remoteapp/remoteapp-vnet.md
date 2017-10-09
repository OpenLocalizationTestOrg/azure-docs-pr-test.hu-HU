---
title: aaaValidate hello Azure VNET toouse az Azure RemoteApp |} Microsoft Docs
description: "Ismerje meg, hogyan toomake meg arról, hogy az Azure virtuális hálózat készen áll az Azure RemoteApp toouse"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 5587556c264356e6ab6039b983a38cb2b95ed268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="validate-hello-azure-vnet-toouse-with-azure-remoteapp"></a>Az Azure RemoteApp hello Azure VNET toouse ellenőrzése
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp egy Azure virtuális Hálózatot használ, érdemes lehet toovalidate hello virtuális hálózat. Ez segít a kapcsolatban előforduló problémák megelőzéséhez.

toovalidate az Azure virtuális hálózat, a következő hello:

1. Hozzon létre egy Azure virtuális gépen belüli hello Azure VNET azt szeretné, hogy az Azure RemoteApp toouse hello alhálózata.
2. Hello segítségével csatlakozzon a virtuális gép toothat **Connect** beállítás hello felügyeleti portálon.
3. Csatlakozás a virtuális gép toohello hello ugyanabban a tartományban, hogy szeretné-e az Azure RemoteApp toouse. A hibrid gyűjteményt, amely a tooyour a helyszíni hálózat létrehozásakor, csatlakozzon a hello virtuális gép tooyour helyi tartományhoz.

Ha ez sikeres, hello Azure VNET is készen toouse RemoteApp.

Hello végpont hibrid gyűjtemény munkafolyamat kapcsolatos további információkért tekintse meg a következő cikkek hello:

* [Hogyan tooplan a virtuális hálózat az Azure RemoteApp](remoteapp-planvnet.md)
* [Hibrid gyűjtemény létrehozása](remoteapp-create-hybrid-deployment.md)
* [Azure RemoteApp gyűjtemény tooyour Azure-beli virtuális hálózathoz (az ExpressRoute támogatása) telepítése](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

