---
title: "aaaRestrict hozzáférés a Internet felé néző végpontok az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslat ** internetre irányuló végpont **-en keresztüli hozzáférés korlátozása."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 727d88c9-163b-4ea0-a4ce-3be43686599f
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/03/2017
ms.author: terrylan
ms.openlocfilehash: ee72497088618d4db29b5ae4183f4fe77b498423
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="restrict-access-through-internet-facing-endpoints-in-azure-security-center"></a>Az Azure Security Centerben Internet felé néző végpontok-en keresztüli hozzáférés korlátozása
Azure Security Center javasolni fogja, az Internet felé néző végpontok-en keresztüli hozzáférés korlátozása, ha bármely, a hálózati biztonsági csoportok (NSG-k) egy vagy több bejövő szabályok, amelyek lehetővé teszik a hozzáférést a "bármely" forrás IP-cím. Túl "any" megnyitása access lehetővé teheti a támadók tooaccess az erőforrások. Security Center javasolni fogja, hogy szerkesztenie kell a bejövő szabályok toorestrict hozzáférés toosource IP-címek, amelyek ténylegesen hozzá kell férniük.

Ez a javaslat bármely nem webes porthoz, amely rendelkezik "a" forrás jön létre.

> [!NOTE]
> Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be. A dokumentum nem tartalmaz lépésenkénti útmutatót.
>
>

## <a name="implement-hello-recommendation"></a>Hello javaslat megvalósítása
1. A hello **javaslatok panel**, jelölje be **internetes végpont-en keresztüli hozzáférés korlátozása**.

   ![Internetről elérhető végponton keresztüli hozzáférés korlátozása][1]
2. Ekkor megnyílik hello panel **internetes végpont-en keresztüli hozzáférés korlátozása**. Ezen a panelen hello virtuális gépek (VM) rendelkező bejövő szabályok létrehozása a potenciális biztonsági kockázatot listáját tartalmazza. Jelöljön ki egy virtuális Gépet.

   ![Jelöljön ki egy virtuális Gépet][2]
3. Hello **NSG** panel információk jelennek meg a hálózati biztonsági csoport, kapcsolódó bejövő szabályok, és a hello kapcsolódó virtuális gép. Válassza ki **bejövő szabályok szerkesztése** tooproceed rendelkező egy bejövő forgalomra vonatkozó szabály szerkesztése.

   ![Hálózati biztonsági csoport panel][3]
4. A hello **bejövő biztonsági szabályok** panelen válassza ki a bejövő forgalomra vonatkozó szabály tooedit hello. Ebben a példában most válasszon **AllowWeb**.

   ![Bejövő biztonsági szabályok][4]

   Vegye figyelembe, igény szerint kiválaszthatja **alapértelmezett szabályok** toosee hello készletét, alapértelmezett szabályokat minden NSG-k által tartalmazott. hello alapértelmezett szabályokat nem lehet törölni, de hozzárendeli őket egy alacsonyabb prioritású virtuális gép, mert azok által létrehozott szabályok hello felülbírálható. További információ [alapértelmezett szabályok](../virtual-network/virtual-networks-nsg.md#default-rules).

   ![Alapértelmezett szabályok][5]
5. A hello **AllowWeb** panelen hello bejövő forgalomra vonatkozó szabály, amely hello hello tulajdonságainak szerkesztése **forrás** blokkot az IP-címek vagy IP-címet. toolearn hello tulajdonságainak hello bejövő forgalomra vonatkozó szabály, bővebben lásd: [NSG-szabályok](../virtual-network/virtual-networks-nsg.md#nsg-rules).

   ![Bejövő forgalomra vonatkozó szabály szerkesztése][6]

## <a name="see-also"></a>Lásd még:
Ez a cikk bemutatta, hogyan tooimplement hello Security Center ajánlás "hozzáférés korlátozása keresztül internetre irányuló végpont." További információ az NSG-ket és a szabályok, toolearn hello következő lásd:

* [Mi az a hálózati biztonsági csoport (NSG)?](../virtual-network/virtual-networks-nsg.md)
* [Hogyan toomanage NSG-k használatával hello Azure-portálon](../virtual-network/virtual-networks-create-nsg-arm-pportal.md)

További információ a Security Center toolearn hello következő lásd:

* [Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md)– megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.
* [Biztonsági javaslatok kezelése az Azure Security Centerben](security-center-recommendations.md) – Miként könnyítik meg a javaslatok az Azure-erőforrások védelmét?
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md)– megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md)– megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md)– gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.
* [Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/)– hello legújabb Azure biztonsági hírek és információ lekérése.

<!--Image references-->
[1]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/restrict-access-thru-internet-facing-endpoint.png
[2]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/select-a-vm.png
[3]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/network-security-group-blade.png
[4]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/inbound-security-rules.png
[5]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/default-rules.png
[6]: ./media/security-center-restrict-access-thru-internet-facing-endpoint/edit-inbound-rule.png
