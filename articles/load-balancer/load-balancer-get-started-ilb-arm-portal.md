---
title: "egy belső terheléselosztó - Azure-portálon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy belső terheléselosztó az erőforrás-kezelőben hello Azure-portál használatával"
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 1ac14fb9-8d14-4892-bfe6-8bc74c48ae2c
ms.service: load-balancer
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: kumud
ms.openlocfilehash: 80124217a84857b542eb41cb814ec97234176dd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-internal-load-balancer-in-hello-azure-portal"></a>Hozzon létre egy belső elosztott terhelésű hello Azure-portálon

> [!div class="op_single_selector"]
> * [Azure Portal](../load-balancer/load-balancer-get-started-ilb-arm-portal.md)
> * [PowerShell](../load-balancer/load-balancer-get-started-ilb-arm-ps.md)
> * [Azure CLI](../load-balancer/load-balancer-get-started-ilb-arm-cli.md)
> * [Sablon](../load-balancer/load-balancer-get-started-ilb-arm-template.md)

[!INCLUDE [load-balancer-get-started-ilb-intro-include.md](../../includes/load-balancer-get-started-ilb-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../azure-resource-manager/resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](load-balancer-get-started-ilb-classic-ps.md).

[!INCLUDE [load-balancer-get-started-ilb-scenario-include.md](../../includes/load-balancer-get-started-ilb-scenario-include.md)]

## <a name="get-started-creating-an-internal-load-balancer-using-azure-portal"></a>Bevezetés belső terheléselosztó Azure Portalon történő létrehozásába

Hello lépéseket toocreate belső terheléselosztót követően – hello Azure portál használata.

1. Nyisson meg egy böngészőt, keresse meg a toohello [Azure-portálon](http://portal.azure.com), és jelentkezzen be az Azure-fiókjával.
2. Hello felső bal oldalán üdvözlő képernyőt, kattintson **új** > **hálózati** > **terheléselosztó**.
3. A hello **létrehozás terheléselosztó** panelen adjon meg egy **neve** a terheléselosztóhoz.
4. A **Séma** alatt kattintson a **Belső** elemre.
5. Kattintson a **virtuális hálózati**, majd válassza ki hello virtuális hálózaton toocreate hello terheléselosztó és.

   > [!NOTE]
   > Ha azt szeretné, hogy toouse hello virtuális hálózati nem látható, ellenőrizze a hello **hely** hello terheléselosztót használja, és ennek megfelelően módosítsa azt.

6. Kattintson a **alhálózati**, majd válassza ki a kívánt toocreate hello terheléselosztó hello alhálózat.
7. A **IP-cím hozzárendelése**, jelölje be az **dinamikus** vagy **statikus**, attól függően, hogy használni szeretne hello IP-cím hello load balancer toobe rögzített (statikus) vagy sem.

   > [!NOTE]
   > Ha toouse egy statikus IP-címet, hogy tooprovide hello terheléselosztó címét.

8. A **erőforráscsoport** adja meg a terheléselosztóhoz hello hello új erőforráscsoport neve, vagy kattintson a **válasszon meglévő** , és válasszon ki egy meglévő erőforráscsoportot.
9. Kattintson a **Create** (Létrehozás) gombra.

## <a name="configure-load-balancing-rules"></a>Terheléselosztási szabályok konfigurálása

Hello után betöltése a terheléselosztó létrehozása, és keresse meg a toohello load balancer erőforrás tooconfigure azt.
Szüksége tooconfigure először egy háttér címkészletet, és egy mintavételt tartozó terheléselosztási szabályok konfigurálása előtt.

### <a name="step-1-configure-a-back-end-pool"></a>1. lépés: Háttérkészlet konfigurálása

1. Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.
2. A hello **beállítások** panelen kattintson a **háttérkészletek**.
3. A hello **háttércímkészletek** panelen kattintson a **Hozzáadás**.
4. A hello **háttérkészlet hozzáadása** panelen adjon meg egy **neve** hello háttérkészlet, és kattintson a **OK**.

### <a name="step-2-configure-a-probe"></a>2. lépés: Mintavétel konfigurálása

1. Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.
2. A hello **beállítások** panelen kattintson a **vizsgálat**.
3. A hello **vizsgálat** panelen kattintson a **Hozzáadás**.
4. A hello **Hozzáadás mintavételi** panelen adjon meg egy **neve** hello mintavételhez.
5. A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.
6. A **Port**, adja meg a hello port toouse hello mintavételi való hozzáféréskor.
7. A **elérési** (a HTTP mintavételt csak), egy mintavételt hello elérési toouse meg.
8. A **időköz** adja meg, hogy milyen gyakran tooprobe hello alkalmazás.
9. A **sérült küszöbérték**, adja meg, hány kísérletet amennyiben nem, mielőtt hello háttér virtuális gép megfelelő állapotúként van megjelölve.
10. Kattintson a **OK** toocreate mintavétel.

### <a name="step-3-configure-load-balancing-rules"></a>3. lépés: Terheléselosztási szabályok konfigurálása

1. Hello Azure-portálon, kattintson **Tallózás** > **Terheléselosztók**, majd kattintson a fenti létrehozott hello terheléselosztóhoz.
2. A hello **beállítások** panelen kattintson a **terheléselosztási szabályok**.
3. A hello **terheléselosztási szabályok** panelen kattintson a **Hozzáadás**.
4. A hello **terheléselosztási szabály hozzáadása** panelen adjon meg egy **neve** hello szabályhoz.
5. A **Protokoll** alatt válassza ki a **HTTP** (webhelyekhez) vagy a **TCP** (más TCP-alapú alkalmazásokhoz) elemet.
6. A **Port**, adja meg a hello port ügyfelek csatlakozni tooin hello terheléselosztót.
7. A **háttérportot**, adja meg a hello port toobe hello háttérkészlet használt (általában hello load balancer port és a háttérportnak hello vannak hello ugyanaz).
8. A **háttérkészlet**, jelölje be a fenti létrehozott hello háttér alkalmazáskészletnek.
9. A **munkamenet megőrzését**, válassza ki, hogyan szeretné a munkamenetek toopersist.
10. A **üresjárati időkorlátja (perc)**, adja meg a hello üresjárati időkorlátját.
11. A **Nem fix IP-cím (közvetlen kiszolgálói válasz)** alatt kattintson a **Letiltva** vagy az **Engedélyezve** elemre.
12. Kattintson az **OK** gombra.

## <a name="next-steps"></a>Következő lépések

[A terheléselosztó elosztási módjának konfigurálása](load-balancer-distribution-mode.md)

[A terheléselosztó üresjárati TCP-időtúllépési beállításainak konfigurálása](load-balancer-tcp-idle-timeout.md)

