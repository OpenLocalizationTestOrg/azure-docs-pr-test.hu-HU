---
title: "irányelvek - Linux elnevezési aaaAzure infrastruktúra |} Microsoft Docs"
description: "Ismerje meg hello legfontosabb tervezési és megvalósítási irányelvek elnevezéséhez az Azure infrastruktúra-szolgáltatásokat."
documentationcenter: 
services: virtual-machines-linux
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 18ee4fb1-8297-49a1-8d3f-097880be67c7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 06/26/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 333146e7b2071e43527a5d7dc2ec02ebfb316eb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-infrastructure-naming-guidelines-for-linux-vms"></a>Linux virtuális gépek elnevezési irányelvek Azure-infrastruktúra 

[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-intro](../../../includes/virtual-machines-linux-infrastructure-guidelines-intro.md)]

Ez a cikk foglalkozik a megértése, hogyan tooapproach elnevezési szabályai minden a különböző Azure-erőforrások toobuild a logikai és könnyen azonosítható állíthatja az erőforrások e a környezetben.

## <a name="implementation-guidelines-for-naming-conventions"></a>Implementációs segédlet az elnevezési konvenciók
Döntéseket:

* Mik az Azure-erőforrások elnevezési szabályai?

Feladatok:

* Adja meg a hello elő-/ utótagok toouse az erőforrások toomaintain konzisztencia között.
* Adja meg a hello számukra követelmény toobe globálisan egyedi nevek a megadott tárfiók.
* A dokumentum hello elnevezési egyezmény toobe használt, és ossza ki tooall Felek érintett tooensure konzisztencia központi telepítések egységességét.

## <a name="naming-conventions"></a>Elnevezési konvenciók
Helyen jó elnevezési semmit az Azure-ban létrehozása előtt kell rendelkeznie. Elnevezési biztosítja, hogy minden hello erőforrások egy előre jelezhető nevét, amely segít a alacsonyabb hello felügyeleti terheket társított erőforrások kezelése.

Akkor érdemes választania toofollow elnevezési konvenciókat meghatározása a teljes szervezet számára, vagy egy adott Azure-előfizetés vagy a fiók egy adott készletét. Bár a szervezetek tooestablish implicit szabályok egyéni felhasználók számára könnyen az Azure-erőforrások használatakor, toobe képes tooscale szüksége csoportjai működik együtt az Azure-ban.

Az elnevezési egyezmény előre egyezniük. Nincsenek szabályok elnevezési konvenciókat, amelyek között Kivágás megadó kapcsolatos szempontokat.

## <a name="affixes"></a>Kell
Toodefine elnevezési tekinti meg, mert egy döntési az e hello utótag:

* hello elejére hello neve (előtag)
* hello végét hello neve (utótag)

Például az alábbiakban a két lehetséges nevek használatával hello erőforráscsoport `rg` elhelyezi:

* Rg-webalkalmazást (előtag)
* Webalkalmazás-Rg (utótag)

Elő-/ utótagok hivatkozhat toodifferent szempontjait ismertető hello adott erőforrásokhoz. hello következő táblázatban néhány példa látható általában akkor használható.

| Aspektusa | Példák | Megjegyzések |
|:--- |:--- |:--- |
| Környezet |fejlesztői, stg, termék |Attól függően, hogy hello célját és minden környezet nevét. |
| Hely |usw (USA nyugati régiója), használjon (USA keleti régiója 2) |Attól függően, hogy hello régió hello datacenter vagy hello terület hello szervezet. |
| Az Azure összetevő, szolgáltatás vagy termék |Az erőforráscsoport, a virtuális hálózat virtuális hálózat rg |Attól függően, hogy mely hello erőforrás biztosít hello termék támogatja. |
| Szerepkör |DB, alkalmazást, webes |Attól függően, hogy hello szerepkör hello virtuális gép. |
| Példány |01, 02, 03, stb. |Egynél több példánnyal rendelkező erőforrások. Például az elosztott terhelésű webkiszolgálók felhőszolgáltatásban. |

Amikor az elnevezési konvenciókat létrehozó, győződjön meg arról, hogy azok egyértelműen megállapítják amely kell toouse az egyes erőforrás, és melyik helyen (előtag vs utótag).

## <a name="dates"></a>dátum
Ez a legtöbbször fontos toodetermine hello erőforrás neve alapján hello létrehozásának dátuma. Azt javasoljuk, hogy hello ÉÉÉÉHHNN dátumformátum. Ezt a formátumot biztosítja, hogy teljes dátum rögzítése, de az is, hogy két erőforrás mappanevek csak hello dátumon eltérőek rendezi a rendszer betűrendben és időrendben hello csak nem.

## <a name="naming-resources"></a>Elnevezési erőforrások
Adjon meg minden típusú erőforrás hello elnevezési konvenciót, amely kell rendelkeznie a szabályokat, amelyek meghatározzák, hogyan tooassign nevek tooeach erőforrás, amely jön létre. Ezek a szabályok vonatkozzon tooall típusú erőforrások, például:

* Előfizetések
* Fiókok
* Tárfiókok
* Virtuális hálózatok
* Alhálózatok
* Rendelkezésre állási csoportok
* Erőforráscsoportok
* Virtual machines (Virtuális gépek)
* Végpontok
* Network security groups (Hálózati biztonsági csoportok)
* Szerepkörök

tooensure, amelyek neve hello tartalmaz elég információt toodetermine toowhich erőforrás hivatkozik, leíró neveket kell használnia.

## <a name="computer-names"></a>Számítógép neve
Amikor létrehoz egy virtuális gép (VM), az Azure-nak nevet a virtuális Géphez a mentést too64 karakterek hello erőforrásnév használt. Az Azure által használt hello hello operációs rendszer van telepítve, a virtuális gép hello ugyanazt a nevet. Azonban ezeket a neveket előfordulhat, hogy nem minden esetben kell hello azonos.

Ha egy virtuális Gépet, amely már tartalmazza az operációs rendszer .vhd kép fájlból jön létre, hello VM nevét az Azure-ban eltérhet hello a virtuális gép operációs rendszerének számítógép nevét. Ebben az esetben is hozzáadhat egy bizonyos fokú nehezen tooVM felügyeletig, ezért nem javasoljuk. Rendelje hozzá a hello Azure VM erőforrás hello azonos hello számítógép neve, hogy rendelje toohello operációs rendszer, hogy a virtuális gép neve.

Azt javasoljuk, hogy hello Azure virtuális gép neve ugyanaz, mint az alapul szolgáló operációs rendszer számítógépnév hello van hello.

## <a name="storage-account-names"></a>Tárfiókok neve
Ez a szakasz nem érvényes túl[Azure felügyelt lemezek](../../storage/storage-managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hogy ne hozzon létre egy külön tárfiókot. Nem felügyelt lemezek storage-fiókok különleges szabályokat nevük rendelkezik. Csak kisbetűket és számokat használható. További információkért lásd: [hozzon létre egy tárfiókot](../../storage/storage-create-storage-account.md#create-a-storage-account). Tárfiók hello nevét, a core.windows.net, továbbá egy globálisan érvényes, egyedi DNS-nevet kell lennie. Például ha hello tárfiók neve mystorageaccount, hello következő eredményül kapott DNS-nevek egyedinek kell lennie:

* mystorageaccount.BLOB.Core.Windows.NET
* mystorageaccount.TABLE.Core.Windows.NET
* mystorageaccount.Queue.Core.Windows.NET

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [virtual-machines-linux-infrastructure-guidelines-next-steps](../../../includes/virtual-machines-linux-infrastructure-guidelines-next-steps.md)]

