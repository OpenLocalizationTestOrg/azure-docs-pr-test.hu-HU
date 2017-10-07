---
title: "aaaWhat Azure hálózati hozzáférés-vezérlési listaként?"
description: "További tudnivalók az Azure-ban a hozzáférés-vezérlési listák"
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 83d66c84-8f6b-4388-8767-cd2de3e72d76
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: a2681af035d162362771e6df9864bbe838f16352
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-an-endpoint-access-control-list"></a>Mi az a végpont hozzáférés-vezérlési listaként?

> [!IMPORTANT]
> Azure két különböző rendelkezik [üzembe helyezési modellel](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) az erőforrások létrehozására és kezelésére vonatkozó: Resource Manager és klasszikus. Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello erőforrás-kezelő telepítési modellt használja. 

Végpont hozzáférés-vezérlési listaként (ACL) egy biztonsági fejlesztés az Azure-telepítés érhető el. Hozzáférés-vezérlési Listában lehetővé teszi a hello képességét tooselectively engedély, vagy letilthatja a forgalmat a virtuális gép végpont. Ez a csomag szűrési lehetőség egy további biztonsági réteget biztosít. Csak hálózati ACL-listát is megadhat. Nem adhat meg egy hozzáférés-vezérlési listája egy virtuális hálózathoz vagy egy bizonyos alhálózat virtuális hálózat található. Ajánlott toouse hálózati biztonsági csoportokkal (NSG-k) helyett a hozzáférés-vezérlési listákat, amikor csak lehetséges. toolearn NSG-ket, kapcsolatos további információkért lásd: [hálózati biztonsági csoport – áttekintés](virtual-networks-nsg.md)

Hozzáférés-vezérlési listákat a PowerShell vagy a hello Azure-portál használatával konfigurálható. a hálózati hozzáférés-vezérlési lista PowerShell,-lel tooconfigure lásd: [kezelése hozzáférés-vezérlési lista PowerShell-lel végpontok](virtual-networks-acl-powershell.md). a hálózati hozzáférés-vezérlési lista használatával tooconfigure hello Azure portál, lásd: [hogyan végpontok tooa virtuális gép tooset](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Használja a hálózati hozzáférés-vezérlési listákat, mindent hello következő:

* Választhatóan engedélyezheti vagy letilthatja a távoli alhálózati IPv4 cím tartomány tooa virtuális gép bemeneti végpontja alapján bejövő forgalmat.
* Blacklist IP-címek
* Virtuális gép végpontonként több szabály létrehozása
* Szabály tooensure hello megfelelő szabályok egy adott virtuális gép végpontjának (legalacsonyabb toohighest) alkalmazzák rendezés használata
* Adjon meg egy hozzáférés-vezérlési listája egy adott távoli alhálózat IPv4-címet.

Lásd: hello [Azure korlátozza](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#networking-limits) ACL korlátok a cikkben találhat.

## <a name="how-acls-work"></a>Hozzáférés-vezérlési listák működése
Az ACL szabályok listáját tartalmazó objektum. Hozzáférés-vezérlési Listában létrehozásakor, és alkalmazza azt tooa virtuális gép végpontjának, csomag történjen jogcímszűrés csomópontján hello gazdagépre a virtuális Gépet. Ez azt jelenti, hogy távoli IP-címek hello forgalmát hello fogadó csomópont a virtuális Gépet a megfelelő ACL-szabályok ahelyett, hogy szűrve van. Ez megakadályozza, hogy a virtuális gép beszállítói költségeit hello értékes CPU-ciklusok hálózaticsomag-szűrés.

A virtuális gép létrehozásakor egy alapértelmezett hozzáférés-vezérlési lista kerül sor tooblock minden bejövő forgalom. Azonban ha a végpont (3389-es port) hoz létre, majd hello alapértelmezett hozzáférés-vezérlési lista módosítani tooallow használó minden bejövő forgalom. Bármely távoli alhálózati érkező bejövő forgalom majd engedélyezett toothat végpontot, és nincs tűzfal kiépítése szükség. Más portok blokkolják a bejövő forgalom, kivéve, ha azokat a portokat végpontokat hoz létre. Kimenő forgalom alapértelmezés szerint engedélyezett.

**Példa alapértelmezett hozzáférés-vezérlési lista tábla**

| **# Szabály** | **Távoli alhálózati** | **Végpont** | **Engedélyezési vagy megtagadási** |
| --- | --- | --- | --- |
| 100 |0.0.0.0/0 |3389 |Engedély |

## <a name="permit-and-deny"></a>Engedélyezi és elutasítása
Akkor is szelektív módon engedélyezheti vagy megtagadhatja a hálózati forgalom egy virtuális gép bemeneti végpont szabályokat, amelyek adja meg az "Engedélyezés" vagy "Elutasítás" létrehozásával. Fontos, hogy alapértelmezés szerint a végpont jön létre, amikor az összes forgalom engedélyezett toohello végpont toonote. Ezért fontos, toounderstand hogyan toocreate engedélyezési vagy megtagadási szabályoknak és helyezze el őket a megfelelő sorrendben hello Ha azt szeretné, hogy részletes szabályozását hello hálózati forgalmat, amely tooallow tooreach hello virtuális gép végpontjának választja.

Pontok tooconsider:

1. **Nincs hozzáférés-vezérlési lista –** alapértelmezés szerint a végpont jön létre, ha azt teszi lehetővé minden hello végponthoz tartozó.
2. **Lehetővé teszik -** egy vagy több "Engedélyezés" tartományok hozzáadásakor megtagadása az összes tartomány alapértelmezés szerint. Csak az engedélyezett IP-címtartomány hello csomagok lesz képes toocommunicate hello virtuálisgép-végponthoz.
3. **Megtagadási -** ad hozzá egy vagy több "Elutasítás" tartományokhoz, ha engedélyezi az összes többi forgalom tartomány alapértelmezés szerint.
4. **Engedélyezési és megtagadási -** "engedély" kombinációját is használja, és a engedélyezett vagy megtagadott toobe "Elutasítás" Ha azt szeretné, hogy egy adott IP-cím kimenő toocarve között.

## <a name="rules-and-rule-precedence"></a>Szabályok és a szabály prioritását
Hálózati hozzáférés-vezérlési listákat az adott virtuális gép végpontokon is beállítható. Megadhatja például, hogy egy RDP-végpontot létrehozni egy virtuális gépen, hogy mely zárolások hozzáférés az egyes IP-címek a hálózati hozzáférés-vezérlési lista. hello az alábbi táblázat egy módon toogrant hozzáférés toopublic virtuális IP-címek (VIP) egy bizonyos tartomány toopermit hozzáférési RDP. Minden más távoli IP-címeket a rendszer megtagadja. Hajtsa végre az azt egy *legalacsonyabb elsőbbséget* szabály sorrendje.

### <a name="multiple-rules"></a>Több szabály
Hello az alábbi példában az Ha azt szeretné, hogy tooallow hozzáférés toohello RDP-végpontot csak a két nyilvános IPv4-címtartományokat (65.0.0.0/8, és 159.0.0.0/8), a el két megadó *engedélyezése* szabályok. Ebben az esetben mivel az RDP alapértelmezés szerint a virtuális gép jön létre, érdemes lehet toolock le a hozzáférési toohello RDP-portjára egy távoli alhálózat alapján. hello az alábbi példában látható egy módon toogrant hozzáférés toopublic virtuális IP-címek (VIP) egy bizonyos tartomány toopermit hozzáférési RDP. Minden más távoli IP-címeket a rendszer megtagadja. Ez működik, mert a hálózati hozzáférés-vezérlési listák egy adott virtuális gép végpont állítható be, és alapértelmezés szerint a hozzáférés megtagadva.

**Példa – több szabály**

| **# Szabály** | **Távoli alhálózati** | **Végpont** | **Engedélyezési vagy megtagadási** |
| --- | --- | --- | --- |
| 100 |65.0.0.0/8 |3389 |Engedély |
| 200 |159.0.0.0/8 |3389 |Engedély |

### <a name="rule-order"></a>A szabály sorrendje
Több szabály adható meg a végpont, mert szerepelnie kell egy módon tooorganize szabályok rendelés toodetermine melyik élvez elsőbbséget. hello szabály rendelés sorrend határozza meg. Hálózati hozzáférés-vezérlési listák hajtsa végre a *legalacsonyabb elsőbbséget* szabály sorrendje. Hello az alábbi példában a 80-as porton hello végpont szelektív kap hozzáférést tooonly bizonyos IP-címtartományok. tooconfigure, olyan megtagadási szabályt kell (szabály \# 100) hello 175.1.0.1/24 hely a címeknél. Egy második szabály majd prioritással 200, hogy engedélyezi-e érni tooall más címek 175.0.0.0/8 alatt van megadva.

**Példa – szabály sorrendje**

| **# Szabály** | **Távoli alhálózati** | **Végpont** | **Engedélyezési vagy megtagadási** |
| --- | --- | --- | --- |
| 100 |175.1.0.1/24 |80 |Megtagadás |
| 200 |175.0.0.0/8 |80 |Engedély |

## <a name="network-acls-and-load-balanced-sets"></a>Hálózati hozzáférés-vezérlési listákat, és betölti az elosztott terhelésű készletek
Hálózati hozzáférés-vezérlési listák egy elosztott terhelésű készlet végpont lehet megadni. Ha hozzáférés-vezérlési Listában elosztott terhelésű készlet van megadva, a hello hálózati hozzáférés-vezérlési lista nem alkalmazott tooall virtuális gépeket, hogy elosztott terhelésű készlet. Például ha elosztott terhelésű készlet jön létre a "Port a 80-as" és hello elosztott terhelésű készlet tartalmaz 3 virtuális gép, hello hálózati hozzáférés-vezérlési lista létrehozott végponton "80-as Port" egy virtuális gép lesz automatikusan alkalmazza a toohello más virtuális gépek.

![Hálózati hozzáférés-vezérlési listákat, és betölti az elosztott terhelésű készletek](./media/virtual-networks-acl/IC674733.png)

## <a name="next-steps"></a>Következő lépések
[Hozzáférés-vezérlési listák PowerShell-lel végpontok kezelése](virtual-networks-acl-powershell.md)

