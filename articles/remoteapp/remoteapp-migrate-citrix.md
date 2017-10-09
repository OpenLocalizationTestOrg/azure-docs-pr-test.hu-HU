---
title: az Azure RemoteApp tooCitrix XenApp Essentials aaaMigrate |} Microsoft Docs
description: Hogyan toomigrate az Azure RemoteApp tooCitrix XenApp alapjai
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 695a8165-3454-4855-8e21-f2eb2c61201b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: aa3ce28bc5a86d5b1dd3408196d51935395f55c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-from-azure-remoteapp-toocitrix-xenapp-essentials"></a>Azure RemoteApp tooCitrix XenApp Essentials áttelepítése

Ha az Azure RemoteApp használ, és azt szeretné, hogy toomigrate tooCitrix XenApp Essentials, nincsenek néhány Előfeltételek tookeep szem előtt. Először, olvassa el a Citrix által [részletes műszaki üzembe helyezési útmutató az Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) és annak [technikai könyvtárában](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Áttelepítési Előfeltételek lépései

1. Hozzon létre egy új virtuális hálózatot, vagy felfedheti, mely Azure virtuális hálózat az Azure Resource Manager amelybe Citrix XenApp Essentials fogja telepíteni. Az Azure RemoteApp használja a klasszikus Azure portálon; hello Citrix XenApp Essentials csak az Azure Resource Manager támogatja.  
2. Győződjön meg arról, kiválasztott hello virtuális hálózatban rendelkezik hálózati hozzáférési tooyour tartományvezérlő, mivel a Citrix csak hibrid telepítések támogatja. Ha egy felhő üzembe helyezése az Azure RemoteApp használ, győződjön meg arról, hogy a virtuális hálózat rendelkezik-e a hálózati hozzáférés tooan Active Directory-tartományvezérlő. Azure Active Directory tartományi szolgáltatások (az Azure Active Directory tartományi szolgáltatások) is használhatja. 
3. Győződjön meg arról, hogy hello DNS helyesen van konfigurálva a hello virtuális hálózathoz, így az első kísérlet sikeres, hogy tartományhoz való csatlakozást. Hozzon létre egy virtuális gép (VM) hello kiválasztott virtuális hálózat, és hajtsa végre a manuális tartományhoz csatlakoztatás tooverify adott hello DNS és a tartományhoz való csatlakozást megfelelően működik-e. Ez biztosítja, hogy Ön sikeres hello először Citrix XenApp Essentials telepít. 
4. Szükség esetén hozzon létre egy virtuális hálózati társviszony-létesítés Azure klasszikus portál virtuális hálózat az Azure RemoteApp használ, és az Azure Resource Manager virtuális hálózat között. A társviszony-létesítési folyamat akkor működik, ha a két hello hálózatok találhatók hello azonos régióban. Ha nem, használja a pont-pont VPN tooconnect hello virtuális hálózatok a hálózatkezeléshez. 
5. Ha szükséges, olvassa el [hogyan toomigrate adatok Azure RemoteApp-](remoteapp-migrate.md). 
6. Frissítse a meglévő Azure RemoteApp kép tooinclude hello Citrix VDA összetevő (útmutatásért lásd a hello Citrix dokumentációt). 
7. Nyissa meg toohello Azure piactéren, és a Citrix XenApp Essentials telepítésének megkezdéséhez.

## <a name="other-considerations"></a>Egyéb szempontok

Vegye figyelembe a következő további szempontok áttelepítésekor hello:
- Citrix XenApp Essentials csak a hibrid telepítések támogatja. Más szóval ez igényli hálózati hozzáférési tooa tartományvezérlőjének rendelés tooperform tartományhoz való csatlakozást. Ha egy felhő üzembe helyezése az Azure RemoteApp használata esetén használja az Azure Active Directory tartományi szolgáltatások vagy ügyeljen arra, hogy a virtuális hálózati hozzáférési tooActive Directory a tartományhoz való csatlakozást. 
- Hogyan toomove felhasználói adatok tooCitrix XenApp Essentials: toolearn [hogyan toomigrate adatok Azure RemoteApp-](remoteapp-migrate.md). 
- Citrix XenApp Essentials csak az Active Directory-fiókokat támogatja. Microsoft-fiókok (például outlook.com, msn.com vagy hotmail.com) nem támogatja. 

## <a name="citrix-xenapp-essentials-billing"></a>Citrix XenApp Essentials számlázási

Az árakkal kapcsolatos teljes részletekért lásd: hello [gyakran ismételt kérdések](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) és [Citrix a cikk áttekintése](https://www.citrix.com/global-partners/microsoft/remote-app.html). Három számlázási összetevőből áll tooCitrix XenApp Essentials:

- hello Citrix szolgáltatás kell fizetni, amely 12 $ felhasználónként, havonta. Az összes Azure piactéren vásárlás, például ez az az Azure-előfizetéshez társított számlázott toohello fizetési módot. A nagyvállalati szerződés (EA) ügyfelek Azure-krediteket pénzügyi nem használható. 
- Távoli adatok szolgáltatások (RDS) ügyféllicenc (CAL). Jelenleg $6.25 hello van kötegelt távoli hozzáférési díj a Citrix XenApp Essentials fizetési hello lehet vásárolni. Ha Ön egy EA ügyfél, a pénzügyi Azure-krediteket toopay is használhatja. Ha azt szeretné, toouse a meglévő távoli asztali szolgáltatások ügyféllicenceit, írjon nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com), így a tooyour számlázási érvénybe lépni. 
- Az Azure számítási és tárolási. Ez hello Azure tárolási költségű és felhasznált számítási fogyasztás hello virtuális gépekhez. Ügyeljen rá, lehetőséget választva a virtuális gép méretét és a felhasználó sűrűség árképzési. Ha Ön egy EA ügyfél, a pénzügyi Azure-krediteket toopay is használhatja.

Ha további kérdései, akkor a következőket teheti:
- E-mailben nekünk az [ arainfo@microsoft.com ](mailto:arainfo@microsoft.com).
- [Kérje az Azure támogatási](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). Első lépésként [megnyitása az Azure támogatási esetet](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) toohelp az előfeltételként szükséges lépések 1-5. A lépéseket 6-7 lépjen kapcsolatba a Citrix hello Citrix felügyeleti portálon támogatási jegy megnyitásával. 
