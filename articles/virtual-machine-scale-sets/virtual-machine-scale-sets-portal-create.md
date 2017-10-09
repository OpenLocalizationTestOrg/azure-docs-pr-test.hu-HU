---
title: "a virtuálisgép-méretezési készlet használatával aaaCreate hello Azure portálon |} Microsoft Docs"
description: "Telepítse a méretezési csoportok Azure-portál használatával."
keywords: "Virtuálisgép-méretezési csoportok"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 05/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 23c88f4b1ba99994a38f8886f60735da74e5c17e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-virtual-machine-scale-set-with-hello-azure-portal"></a>Hogyan toocreate a virtuálisgép-méretezési készlet rendelkező hello Azure-portálon
Az oktatóanyag bemutatja, hogyan könnyen egy virtuálisgép-méretezési csoportban toocreate csak néhány percet, hello Azure-portál használatával. Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/) a virtuális gép létrehozásának megkezdése előtt.

## <a name="choose-hello-vm-image-from-hello-marketplace"></a>Válassza ki a Virtuálisgép-lemezkép hello hello piactérről
Hello portálról könnyedén telepítheti egy méretezési CentOS, CoreOS, Debian, nyissa meg a Suse, Red Hat Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server vagy Windows Server-lemezképekben állítható be.

Első lépésként nyissa meg a toohello [Azure-portálon](https://portal.azure.com) egy webböngészőben. Kattintson a `New`, keressen `scale set`, majd válassza ki a hello `Virtual machine scale set` bejegyzést:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-hello-scale-set"></a>Hello méretezési készlet létrehozása
Most hello alapértelmezett beállításokat használja, és gyorsan létrehozhat hello méretezési készlet.

* A hello `Basics` panelen adjon meg egy nevet hello méretezési készlet. Ez a név lesz alapszintű hello hello hello terheléselosztó elé hello méretezési teljes Tartományneve, a ezért győződjön meg arról, hogy az összes Azure hello neve nem egyedi.
* Válassza ki a kívánt operációs rendszer írja be, adja meg a kívánt felhasználónevet, és válassza ki, mely hitelesítési írja be, akkor inkább. Ha úgy dönt, hogy a jelszó, kell lennie legalább 12 karakter hosszú, és három hello négy következő bonyolultsági megfeleljen: egy kisbetű, egy nagybetű, egy számot és egy speciális karaktert. További információk a [felhasználónév- és jelszókövetelményekről](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm). Ha úgy dönt, `SSH public key`, akkor kell, hogy tooonly illessze be a nyilvános kulcsot, nem a titkos kulcsot:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Válassza ki, hogy szeretné-e például toolimit hello méretezési tooa egyetlen elhelyezési csoport, vagy ha több elhelyezési csoportot kell foglal magába. Engedélyezi a hello méretezési toospan elhelyezési csoportok engedélyezi a skálázási több mint 100 virtuális gépek készletekben kapacitás (felfelé too1, 000) – bizonyos korlátozásokkal. További információkért lásd: [ebben a dokumentációban](./virtual-machine-scale-sets-placement-groups.md).
* Adja meg a kívánt erőforrás csoport nevét és helyét, és kattintson `OK`.
* A hello `Virtual machine scale set service settings` panel: Adja meg a kívánt tartománynév-címke (hello alapját jelentő hello FQDN hello terheléselosztóhoz hello méretezési előtt). Ezt a címkét az összes Azure egyedinek kell lenni.
* Válassza ki a kívánt operációs rendszer lemezképét, a példányszám és a gép méretét.
* Válassza ki a kívánt lemez típusa: kezelni vagy sem. További információkért lásd: [ebben a dokumentációban](./virtual-machine-scale-sets-managed-disks.md). Ha úgy dönt, hogy toohave hello méretezési több elhelyezési csoportok span, ez a beállítás nem lesz elérhető mert felügyelt lemezes skálázási készletek toospan elhelyezési csoportok szükséges.
* Engedélyezze vagy tiltsa le az automatikus skálázási és konfigurálása, ha engedélyezve:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* A hello `Summary` érvényesítési történik, amikor paneljén kattintson `OK` toostart hello méretezési központi telepítés.


## <a name="connect-tooa-vm-in-hello-scale-set"></a>Csatlakozás hello méretezési csoportban lévő virtuális gép tooa
Ha úgy dönt, hogy toolimit a méretezési tooa egyetlen elhelyezési csoportot, majd hello méretezési NAT konfigurált szabályok toolet toohello méretezési készletben könnyen csatlakoztatja a következőkkel (Ha nem, tooconnect toohello virtuális gépek hello méretezési állít be, valószínűleg kell toocreate egy a hello jumpbox hello méretezési megegyező virtuális hálózatban). toosee, keresse meg a toohello `Inbound NAT Rules` hello terheléselosztót hello méretezési lapján:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

VM hello méretezési beállítani a NAT-szabályok használata tooeach is elérheti. Például a Windows-méretezési csoport, ha a bejövő portot 50000, a NAT-szabály kapcsolódhat toothat gép RDP-kapcsolaton keresztül a `<load-balancer-ip-address>:50000`. A Linux-méretezési csoport, akkor kapcsolódnának hello paranccsal `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Következő lépések
Hogyan toodeploy méretezési hello CLI sorozata dokumentációja, lásd: [ebben a dokumentációban](virtual-machine-scale-sets-cli-quick-create.md).

Hogyan toodeploy méretezési PowerShell sorozata dokumentációja, lásd: [ebben a dokumentációban](virtual-machine-scale-sets-windows-create.md).

Hogyan toodeploy-skálázási készletekben, a Visual Studio dokumentációja, lásd: [ebben a dokumentációban](virtual-machine-scale-sets-vs-create.md).

Általános dokumentáció, tekintse meg a hello [dokumentációjának áttekintése lapon a méretezési készlet](virtual-machine-scale-sets-overview.md).

Általános információkért tekintse meg a hello [méretezési csoportok fő kezdőlapjának](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

