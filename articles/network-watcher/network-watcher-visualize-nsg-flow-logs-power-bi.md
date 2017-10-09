---
title: "a Power BI naplózza aaaVisualizing Azure hálózati biztonsági csoport folyamata |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan naplózza az toovisualize NSG folyamata a Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a>Visualizing hálózati biztonsági csoport folyamata naplók és a Power bi-ban

Hálózati biztonsági csoport folyamata naplók tooview információkat bemenő és kimenő IP-forgalom hálózati biztonsági csoportok lehetővé teszik. A folyamat naplók megjelenítése kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, hello folyamata (forrás vagy a cél IP, forrás vagy a cél Port Protocol), és ha hello forgalom lett engedélyezett vagy megtagadott 5 rekordos információt.

Ez lehet a naplózási adatok hello naplófájlok manuálisan keresve folyamata bonyolult toogain betekintést. Ebben a cikkben nyújtunk a megoldás toovisualize a legutóbbi folyamata naplózza, és a hálózati forgalom megismerése.

## <a name="scenario"></a>Forgatókönyv

A forgatókönyv a következő hello azt a Power BI asztali toohello tárfiók jelenleg konfigurált hello fogadó, az NSG Flow naplózási adatok csatlakozzon. Jelenleg a tárfiók tooour kapcsolódás után a Power BI letölti és elemez hello naplók tooprovide hello forgalmat hálózati biztonsági csoportok által naplózott vizuális ábrázolását.

Ellenőrizheti a hello sablonban megadott hello látványelemek használatával:

* Felső Talkers
* Adatsorozat Flow időadatok irányt vagy szabály döntése alapján
* Hálózati illesztő MAC-cím alapján adatfolyamok
* NSG-t és a szabály által adatfolyamok
* Folyamatok által célport

hello sablon nem szerkeszthető, ezért a tooadd új adatokat, a látványelemek, módosítsa vagy a lekérdezések toosuit szerkesztése az igényeinek.

## <a name="setup"></a>Beállítás

Mielőtt hozzákezd, rendelkeznie kell hálózati biztonsági csoport Flow naplózás engedélyezve van egy vagy több hálózati biztonsági csoportok a fiókjában. Hálózati biztonsági engedélyezésével kapcsolatos flow a naplókat, tekintse meg a következő cikket toohello: [bemutatása tooflow naplózási a hálózati biztonsági csoportok](network-watcher-nsg-flow-logging-overview.md).

Hello Power BI Desktop ügyfél telepítve a számítógépre, és elég szabadítson fel lemezterületet a gép toodownload és a betöltés hello naplóadatokat, hogy létezik-e a tárfiókban lévő is rendelkeznie kell.

![Visio diagram][1]

### <a name="steps"></a>Lépések

1. A letöltés, és nyissa meg a Power BI Desktop alkalmazás hello Power BI-sablon a következő hello [hálózati figyelő Power bi folyamata naplózza sablon](https://aka.ms/networkwatcherpowerbiflowlogstemplate)
1. Adja meg a szükséges hello lekérdezési paraméterekről
    1. **StorageAccountName** – hello tárfiók tartalmazó hello NSG folyamata naplózza, hogy megadja annak a toohello nevére kívánja tooload, majd jelenítheti meg.
    1. **NumberOfLogFiles** – naplófájlokat, hogy kívánja toodownload, majd a Power bi-ban megjelenítheti hello számát adja meg. Például ha 50 meg van adva, hello 50 legfrissebb naplófájlokat. FF 2 NSG-ket van engedélyezve, és toosend NSG folyamata naplók toothis fiók konfigurálva, majd hello naplók az elmúlt 25 óra lehet megtekinteni.

    ![a Power BI fő][2]

1. Írja be a hozzáférési kulcsot a tárfiók hello. Érvényes elérési kulcsok tooyour storage-fiókot az Azure portál és kiválasztásával hello útvonalon található **hívóbetűk** hello-beállítások menüjében. Kattintson a **Connect** majd a módosítások alkalmazásához.

    ![Tárelérési kulcsok][3]

    ![hozzáférési kulcs 2][4]

4.  A naplók letöltéséhez és értelmezni, és most már használhatja hello előre létrehozott látványelemek.

## <a name="understanding-hello-visuals"></a>Hello látványelemek ismertetése

Feltéve, hogy a hello sablon olyan készlete, amelyek segítenek látványelemek ellenőrizze értelmében hello NSG Flow naplóadatokat. hello következő ábrákon láthatók milyen hello irányítópult néz amikor feltöltve adatokkal mintáját. Az alábbiakban azt vizsgálja meg az egyes visual nagyobb részletességgel 

![Power BI][5]
 
hello felső Talkers visual látható hello IP-címek, amelyek kezdeményezett többsége hello hello megadott időszak alatt. hello hello listamezők összhangban toohello relatív kapcsolatok száma. 

![toptalkers][6]

hello következő idő adatsorozat diagramjait megjelenítése hello időszakban adatfolyamok hello száma. hello felső grafikon által hello folyamat iránya van szegmentált, valamint alacsonyabb hello van szegmentált hello döntési (engedélyezése vagy megtagadása) által. A Megjelenítés választhat vizsgálja meg a forgalom trendek az idő múlásával és bármely rendellenes igényeiben jelentkező direkt vagy elutasítja a forgalom vagy forgalom szegmentálását.

![flowsoverperiod][7]

hello következő diagramjait megjelenítése hello adatfolyamok hálózati adapterenként, hello felső folyamat iránya által szegmentált és hello alacsonyabb által végrehajtott döntési szegmentált. Az információ, amely a virtuális gépek közölt hello legtöbb relatív tooothers, és ha a forgalom tooa speciális virtuális Gépet folyamatban van férhet engedélyez vagy tilt.

![flowspernic][8]

a következő fánk kerék diagram hello által célport adatfolyamok részletes információkat jeleníti meg. Az információ, megtekintheti a leggyakrabban használt hello célport megadott hello belül használt időszak.

![Fánk][9]

hello következő sávdiagram látható hello folyamata NSG-t és a szabály által. Az információ megtekintheti hello NSG-ket hello felelős legtöbb forgalom, valamint a forgalom felosztása hello egy NSG-szabály által.

![barchart][10]
 
hello következő tájékoztató diagramok megjelenített információkat hello NSG-ket hello naplók szerepelnek, hello adatfolyamok rögzített hello időszak, és hello dátum hello legkorábbi napló rögzített száma. Ezt az információt biztosít egy meghatározni, hogy milyen NSG-ket naplózásának és hello flow dátumtartományt.

![infochart1][11]

![infochart2][12]

Ez a sablon tartalmazza hello következő szeletelők tooallow meg tooview csak hello adatok tervezheti meg. Az erőforráscsoportok, NSG-ket és szabályok alapján szűrhet. 5 rekordos információt, döntési és hello napló írása történt hello idő is végezhet.

![a szeletelők][13]

## <a name="conclusion"></a>Összegzés

Azt mutatott ebben a forgatókönyvben, hogy hálózati figyelőt, és a Power BI által biztosított hálózati biztonsági csoport Flow naplók segítségével azt tudja toovisualize és hello forgalom ismertetése. Hello megadott sablont használ, a Power BI hello naplók letölti közvetlenül a tárolási, és helyileg feldolgozza azokat. Igénybe vett idő tooload hello sablon kért fájlok hello száma attól függ, és a fájlok összesített mérete le.

Érzi, hogy szabad toocustomize Ez a sablon az igényeinek. Számos módon számos használható a Power BI és hálózati biztonsági csoport Flow naplófájlokat. 

## <a name="notes"></a>Megjegyzések

* Alapértelmezés szerint naplója`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`

    * Ha más adatok már létezik egy másik címtárban azok a lekérdezések toopull hello és hello adatfeldolgozásra módosítani kell.

* a megadott hello sablon nem ajánlott 1 GB-nál több naplók való használatra.

* Ha nagy mennyiségű naplókat, azt javasoljuk, hogy a használatával, például a Data Lake vagy SQL server egy másik adattároló megoldás vizsgálata.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a hello Elastick verem ellátogatva [hálózati forgalom minták tooand a virtuális gépek nyílt forráskódú eszközökkel megjelenítése](network-watcher-using-open-source-tools.md)

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
