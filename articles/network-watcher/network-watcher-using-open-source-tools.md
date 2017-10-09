---
title: "aaaVisualize hálózati forgalmának mintáit nyílt forráskódú eszközök és az Azure hálózati figyelőt |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan toouse hálózati figyelőt csomag rögzítése a Capanalysis toovisualize forgalmi minták tooand a virtuális gépek."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a>Hálózati forgalmi minták tooand a virtuális gépek nyílt forráskódú eszközökkel megjelenítése

Csomag rögzíti, amelyek lehetővé teszik a tooperform hálózati vizsgálatokhoz és a mély Csomagvizsgálat hálózati-adatokat tartalmaznak. Nincsenek sok megnyílik a hálózattal kapcsolatos tooanalyze csomag rögzítésekre toogain insights használható forrás-eszközöket. Egy ilyen eszköz CapAnalysis, egy nyílt forráskódú csomag rögzítési képi megjelenítés eszköz. Csomagadatok rögzítési megjelenítése módja a értékes tooquickly célosztályából insights megoldások és rendellenességek észlelését, a hálózaton belül. Képi megjelenítések is biztosít egy ilyen insights megosztási egyszerűen felhasználhatóvá módon.

Azure hálózati figyelőt biztosítja, hogy rögzíti az értékes adatok tooperform csomag lehetővé teszi toocapture hello a hálózaton. Ez a cikk azt ismertetik a hogyan toovisualize és nyereség információkat kaphat a csomag rögzíti a CapAnalysis használata a hálózati figyelőt.

## <a name="scenario"></a>Forgatókönyv

Egy egyszerű webalkalmazást telepítve van a virtuális gép az Azure kívánt toouse nyílt forráskódú eszközök toovisualize a hálózati forgalom tooquickly azonosítása folyamata mintákat és minden lehetséges rendellenességek észlelését. A hálózati figyelőt szerezzen be hálózati környezete csomag rögzítőeszközt, és közvetlenül a tárfiók tárolja. CapAnalysis majd hello csomagrögzítéssel közvetlenül hello tárolási blobból betöltési, és megjelenítheti annak tartalmát.

![forgatókönyv][1]

## <a name="steps"></a>Lépések

### <a name="install-capanalysis"></a>CapAnalysis telepítése

tooinstall CapAnalysis virtuális gépen, olvassa el a toohello hivatalos utasításokat itt https://www.capanalysis.net/ca/how-to-install-capanalysis.
Rendelés távolról fér hozzá CapAnalysis, igazolnia kell tooopen portjához 9877 a virtuális Gépet egy új bejövő szabály hozzáadásával. A hálózati biztonsági szabályok létrehozásával kapcsolatban bővebben lásd túl[egy meglévő NSG-szabályok létrehozására](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg). Miután hello szabály sikeresen hozzá lett adva, képes tooaccess CapAnalysis kell-e a`http://<PublicIP>:9877`

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a>Használja az Azure hálózati figyelőt toostart csomagot munkamenet rögzítése

Hálózati figyelő lehetővé teszi toocapture csomagok tootrack forgalom mindkét virtuális gép. Olvassa el a toohello található utasítások segítségével: [kezelése csomagrögzítéseket hálózati figyelőt](network-watcher-packet-capture-manage-portal.md) toostart csomag rögzítési munkamenet. A csomagrögzítéssel tárolhatja a tárolási blob toobe CapAnalysis érhetők el.

### <a name="upload-a-packet-capture-toocapanalysis"></a>A csomag rögzítési tooCapAnalysis feltöltése
A hálózati figyelőt hello "Importálása az URL" lapon és a tárolási blob hivatkozás toohello által végrehajtott hello csomagrögzítéssel tároló csomagrögzítéssel közvetlenül is feltölthet.

Ha egy hivatkozás tooCapAnalysis biztosít, győződjön meg arról, hogy tooappend SAS-token toohello tárolási blob URL-címet.  toodo, keresse meg a tooShared hozzáférésű jogosultságkódot hello tárfiókból, engedélyezett engedélyek hello kijelölni, majd nyomja meg hello SAS létrehozása gomb toocreate jogkivonat. Majd fűzze hozzá a SAS token toohello csomag rögzítési tárolási blob URL-CÍMÉT.

hello eredményül kapott URL-cím fog megjelenni ehhez hasonló: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere


### <a name="analyzing-packet-captures"></a>Rögzíti a elemzésekor csomag

CapAnalysis kínál különböző beállítások toovisualize a csomagrögzítéssel, minden olyan elemzés különböző szempontjából. A vizuális összesítések a hálózati forgalom trendjeinek megértését, és gyorsan direkt szokatlan tevékenység. Ezek a szolgáltatások néhány hello a következő listában láthatók:

1. Attribútumfolyam táblák

    A táblázat által biztosított hello hello csomagadatok forgalmának listája, hello adatfolyamok társított időbélyeg hello és hello különböző protokollok társított hello folyamat, valamint a forrás és cél IP.

    ![capanalysis folyamat lap][5]

1. Protokoll – áttekintés

    Ezen az ablaktáblán lehetővé teszi a tooquickly tekintse meg a hálózati forgalom hello terjesztési hello keresztül, különböző protokollok és földrajzi.

    ![capanalysis protokoll – áttekintés][6]

1. Statisztika

    Ezen az ablaktáblán lehetővé teszi, hogy tooview hálózati forgalom statisztika – bájt küldött és protokollt használja a forrás és cél IP-címek, a folyamatok egyes hello forrás és cél IP-címek, kapott különböző tranzakciók, és az adatfolyamok hello időtartama.

    ![capanalysis statisztikák][7]

1. geomap

    Ezen az ablaktáblán biztosít a nézet a hálózati forgalom az egyes országok forgalom mennyiségét toohello skálázás színeivel. Kiválaszthatja a kijelölt országok tooview további folyamata statisztikákról, például adatok küldése és fogadása szükséges a IP-címeket az adott ország hello részét.

    ![geomap][8]

1. Szűrők

    CapAnalysis biztosít az adott csomagok gyors elemzés szűrők. Toofilter hello adatok például protokoll toogain adott áttekinthetik a forgalom részhalmaz szerint is választhat.

    ![szűrők][11]

    Látogasson el [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) összes CapAnalysis képességeivel kapcsolatos további toolearn.

## <a name="conclusion"></a>Összegzés

Hálózati figyelőt csomag rögzítése funkció lehetővé teszi a toocapture hello adatok szükséges tooperform hálózati Törvényszéki, és jobb megértése érdekében a hálózati forgalmat. Ebben a forgatókönyvben azt lehet bemutatta, hogyan hálózati figyelőt csomagok készítését könnyen integrálható a nyílt forráskódú képi megjelenítés eszközök. A nyílt forráskódú eszközök, például CapAnalysis toovisualize csomagok rögzíti, mély Csomagvizsgálat, és gyorsan azonosíthatja a trendeket belül a hálózati forgalmat.

## <a name="next-steps"></a>Következő lépések

További információ az NSG-folyamat naplók toolearn látogasson el [naplózza az NSG-folyamat](network-watcher-nsg-flow-logging-overview.md)

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
