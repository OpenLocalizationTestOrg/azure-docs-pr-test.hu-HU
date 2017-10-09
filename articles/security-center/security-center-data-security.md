---
title: "Biztonsági központ adatbiztonság aaaAzure |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan kezeli az Azure Security Center az adatokat, és hogyan gondoskodik a védelmükről."
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 33f2c9f4-21aa-4f0c-9e5e-4cd1223e39d7
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/12/2017
ms.author: yurid
ms.openlocfilehash: 30f8b11272dc5df6d485608abdaa62ba57e63f23
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-security-center-data-security"></a>Az Azure Security Center által nyújtott adatbiztonság
toohelp felhasználók megakadályozása, észlelésében és kezelésében toothreats, az Azure Security Center gyűjt, és feldolgozza a biztonsággal kapcsolatos adatokat, például konfigurációs adatokat, metaadatokat, eseménynaplók, összeomlási memóriaképek és egyéb. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból.

Ez a cikk bemutatja, hogyan kezeli az Azure Security Center az adatokat, és hogyan gondoskodik a védelmükről.

>[!NOTE] 
>Korai. június 2017 verziótól kezdve a Security Center használnak hello Microsoft Monitoring Agent toocollect, illetve adatainak tárolásához. Lásd: [Azure Security Center Platform áttelepítési](security-center-platform-migration.md) további toolearn. a cikkben szereplő információkat hello Security Center funkció átmenet toohello Microsoft Monitoring Agent után jelöli.
>


## <a name="data-sources"></a>Adatforrások
Az Azure Security Center elemzi az adatokat a következő források tooprovide látható a biztonsági állapot hello biztonsági rések azonosítása és azok mérséklési javasoljuk és aktív fenyegetések észlelése:

- Azure-szolgáltatások: Kommunikál a service erőforrás-szolgáltató által telepített hello konfigurálása Azure-szolgáltatásokkal kapcsolatos információkat használja.
- Hálózati forgalom: a hálózati forgalom metaadataiból vett mintát használja a Microsoft infrastruktúrájából, például a forrás és a cél IP-címét és portját, a csomagméretet és a hálózati protokollt.
- Partnermegoldások: Az integrált partnermegoldásoktól, például tűzfalaktól és kártevőirtó megoldásoktól érkező biztonsági riasztásokat használja. 
- Saját virtuális gépek és kiszolgálók: Konfigurációs és biztonsági eseményekkel kapcsolatos információkat használ (például Windows-esemény- és auditnaplókat, IIS-naplókat, rendszernapló-üzeneteket és a virtuális gépekről származó összeomlási memóriaképeket). Továbbá ha riasztás jön létre, az Azure Security Center előfordulhat, hogy hello méretű lemez hatással pillanatkép készítése és gép összetevők kapcsolódó toohello riasztás kinyerése hello méretű lemez, például egy beállításjegyzék-fájlt, Törvényszéki célokra.


## <a name="data-protection"></a>Adatvédelem
**Az adatok elkülönítése**: adatok az egyes összetevők teljes hello szolgáltatás logikailag külön maradnak. Az összes adat szervezet szerint van megcímkézve. A címkézés továbbra is fennáll, hello adatok életciklus során, és van kényszerítve hello szolgáltatást minden egyes rétegben.

**Adat-hozzáférési**: A rendezés tooprovide biztonsági javaslatok és vizsgálja meg az esetleges biztonsági fenyegetéseket jelezhetnek, Microsoft személyzet hozzáférhet a gyűjtött adatok vagy folyamatlétrehozási eseményeket, a virtuális gép Azure-szolgáltatásokkal, beleértve a összeomlási memóriaképek, általi feldolgozása lemez pillanatfelvételek és összetevők, többek között lehet, hogy véletlenül ügyféladatok vagy a virtuális gépek személyes adatait. Toohello mérvadónak [Microsoft Online Services feltételei és adatvédelmi nyilatkozata](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31), amely adja meg, hogy a Microsoft nem ügyféladatok használja, vagy származnia hirdetési vagy hasonló kereskedelmi célú származó információkat. Csak vesszük ügyféladatok szükséges tooprovide, az Azure szolgáltatások, beleértve azokat a szolgáltatásokat nyújtó kompatibilis céljából. Minden jog tooCustomer adatok megőrzi.

**Adatok felhasználásával**: Microsoft minták használ, illetve több körében tapasztalható fenyegetésfelderítési adataival bérlők tooenhance a megelőzése és észlelési képességek, azt megteheti hello adatvédelmi kötelezettségvállalások leírtak szerint a [adatvédelmi Utasítás](https://www.microsoft.com/privacystatement/en-us/OnlineServices/Default.aspx).

## <a name="data-location"></a>Az adatok helye

**A munkaterületek**: munkaterület a következő Geos hello van megadva, és az Azure virtuális gépeken, beleértve összeomlási memóriaképeket, és bizonyos típusú riasztási adatokat gyűjtött adatokat a legközelebbi munkaterület hello vannak tárolva. 

| Virtuális gép geo                        | Munkaterület geo |
|-------------------------------|---------------|
| Egyesült Államok, Brazília, Kanada | Egyesült Államok |
| Európa, Egyesült Királyság        | Európa        |
| Ázsia és a Csendes-óceáni térség, Japán, India    | Ázsia és a Csendes-óceáni térség  |
| Ausztrália                     | Ausztrália     |

 
Virtuálisgép-lemez pillanatfelvételek tárolódnak hello hello VM lemezként ugyanazt a tárfiókot.
 
A virtuális gépek és a kiszolgálókra, más környezetekben például a helyszínen, megadhat hello munkaterületet, és az összegyűjtött adatokat tároló terület. 

**Az Azure Security Center tárolási**: biztonsági riasztásokat, a partner riasztások, beleértve a vonatkozó adatok tárolása – hello toohello helye szerint kapcsolódó Azure-erőforrás, mivel a biztonsági állapot kapcsolatos információkat és a javaslat tárolja központilag hello az Amerikai Egyesült Államok vagy Európa toocustomer tartozó hely szerint.
Az Azure Security Center ideiglenes másolatokat gyűjt az összeomlási memóriaképek fájljairól, és elemzi ezeket a biztonsági réseket kihasználó támadások és a sikeres feltörések nyomai után kutatva. Az Azure Security Center elvégzi az elemzés belül azonos földrajzi hello munkaterület hello, és a törlések hello elmúló másolja, amikor befejeződött.

Gép összetevők központilag a tárolt hello ugyanabban a régióban, mint a virtuális gép hello. 


## <a name="managing-data-collection-from-virtual-machines"></a>Az adatgyűjtés kezelése a virtuális gépeken

Ha bekapcsolja a Security Centert az Azure-ban, az adatgyűjtés bekapcsolódik minden előfizetésénél. Engedélyezheti az adatgyűjtést az Azure Security Center biztonsági házirend szakasza hello az előfizetésekhez. Adatgyűjtés be van kapcsolva, az Azure Security Center rendelkezések hello Microsoft Monitoring Agent összes meglévő támogatott az Azure virtuális gépek és a létrehozott újakat. a különböző biztonsági vizsgálja hello Microsoft Monitoring agent a kapcsolódó konfigurációk és események be azt [Windows esemény-nyomkövetése](https://msdn.microsoft.com/library/windows/desktop/bb968803.aspx) nyomkövetési (ETW). Hello operációs rendszer ezenkívül emeli Eseménynapló eseményeinek hello során hello gépet futtat. A gyűjtött adatok például a következők: az operációs rendszer típusa és verziója, az operációs rendszer naplói (Windows-eseménynaplók), a futó folyamatok, a gép neve, az IP-címek, a bejelentkezett felhasználó és a bérlő azonosítója. a Microsoft Monitoring Agent hello olvassa be az eseménynapló-bejegyzések és ETW követi, és másolja őket elemzésre tooyour munkaterületek. a Microsoft Monitoring Agent hello is összeomlási kiírt fájlok tooyour munkaterületek másolja.

Ha az Azure Security Center szabad használ, letilthatja hello biztonsági házirendet a virtuális gépek adatok gyűjtése. Adatgyűjtés hello Standard csomagra előfizetések szükség. A virtuálisgép-lemez pillanatképeinek és összetevőinek gyűjtése akkor is engedélyezve lesz, ha letiltotta az adatgyűjtést.


## <a name="see-also"></a>Lásd még:
Ebből a dokumentumból megtudta, hogyan kezeli az Azure Security Center az adatokat, és hogyan gondoskodik azok védelméről. További információ az Azure Security Center toolearn lásd:

* [Azure Security Center tervezéséhez és az üzemeltetési útmutatóban](security-center-planning-and-operations-guide.md) – további hogyan tooplan és hello kialakítási szempontok tooadopt az Azure Security Center ismertetése.
* [Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello állapotát az Azure-erőforrások
* [Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – további hogyan toomanage és válaszoljon toosecurity riasztások
* [Partnermegoldások figyelése az Azure Security Center](security-center-partner-solutions.md) – megtudhatja, hogyan toomonitor hello partneri megoldások biztonsági állapotát.
* [Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban
* [Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) – Blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.
