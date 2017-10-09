---
title: "aaaPublish HDInsight-alkalmazások – Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate és a HDInsight-alkalmazások közzététele."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 7da0aa53828563e50ef372df901e1ba541fb40be
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-hdinsight-applications-into-hello-azure-marketplace"></a>Hello Azure piactér HDInsight-alkalmazások közzététele
A HDInsight-alkalmazások olyan alkalmazások, amelyeket a felhasználók egy Linux-alapú HDInsight-fürtre telepíthetnek. Ezek az alkalmazások lehetnek a Microsoft, független szoftvergyártók (ISV-k) vagy a felhasználók fejlesztései. Ebből a cikkből megismerheti, hogyan toopublish hello Azure piactér HDInsight-alkalmazásokat.  Hello Azure piactéren történő közzétételre vonatkozó általános információkért lásd: [közzététele egy ajánlat toohello Azure piactér](../marketplace-publishing/marketplace-publishing-getting-started.md).

A HDInsight-alkalmazások használata hello *kapcsolja a saját licenc használata (BYOL)* modell, ahol alkalmazás szolgáltató hello alkalmazás tooend-felhasználók licencelési felelős és a végfelhasználók csak felszámított az Azure-erőforrások hello azok Hozzon létre, például a HDInsight-fürt hello és annak virtuális Gépekért/csomópontokért. Számlázási magának hello alkalmazásnak Azure szolgáltatáson keresztül nem történik.

Más HDInsight alkalmazással kapcsolatos cikk:

* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.
* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan tooinstall és tesztelési egyéni HDInsight-alkalmazások.

## <a name="prerequisites"></a>Előfeltételek
toosubmit az egyéni alkalmazás toohello piactér kell létrehozta és tesztelte az alkalmazást. Tekintse meg a következő cikkek hello:

* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan tooinstall és tesztelési egyéni HDInsight-alkalmazások.

Kell is regisztrálta a fejlesztői fiókját. Lásd: [közzététele egy ajánlat toohello Azure piactér](../marketplace-publishing/marketplace-publishing-getting-started.md) és [Microsoft Developer-fiók létrehozása](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-application"></a>Alkalmazás meghatározása
Nincsenek két lépésben tehetők közzé alkalmazásokat toohello Azure piactéren.  A meghatározásához először egy **createUiDef.json** fájl tooindicate, amely az alkalmazás-fürtök legyen kompatibilis, és tegye közzé a hello sablon hello Azure-portálon. a következő szakasz hello egy createUiDef.json mintafájl látható.

    {
        "handler": "Microsoft.HDInsight",
        "version": "0.0.1-preview",
        "clusterFilters": {
            "types": ["Hadoop", "HBase", "Storm", "Spark"],
            "tiers": ["Standard", "Premium"],
            "versions": ["3.4"]
        }
    }


| Mező | Leírás | Lehetséges értékek |
| --- | --- | --- |
| types |hello fürttípusok, amelyekkel hello alkalmazás kompatibilis. |Hadoop, HBase, Storm, Spark (vagy ezek kombinációi) |
| tiers |hello fürtrétegek, amelyekkel hello alkalmazás kompatibilis. |Standard, Premium (vagy mindkettő) |
| versions |hello HDInsight-fürttípusok, amelyekkel hello alkalmazás kompatibilis. |3.4 |

## <a name="application-install-script"></a>Alkalmazás telepítési parancsfájlja
Amikor az alkalmazás telepítve van (vagy egy meglévő vagy új) egy fürt egy élcsomópontot jön létre, és hello alkalmazás telepítési parancsfájl fut rajta.
  > [!IMPORTANT]
  > hello neve hello alkalmazás telepítési parancsfájl nevét egy adott fürtben egyedinek kell lennie a formátum a következő hello rendelkező.
  > 
  > name": "[concat('hue-install-v0','-' ,uniquestring(‘applicationName’)]"
  > 
  > Vegye figyelembe, hogy nincsenek a parancsfájl nevét három részből toohello:
  > 
  > 1. A parancsfájl nevének előtagja, amelynek tartalmaznia kell vagy hello alkalmazás neve, vagy a név megfelelő toohello alkalmazás.
  > 2. Egy „-” kötőjel az olvashatóság kedvéért.
  > 3. Egy egyedi karakterláncfüggvény, amelynek paraméter hello hello alkalmazás neve.
  > 
  > Példa: a fenti hello említi váljon: hue-install-v0-4wkahss55hlas hello a megőrzött Parancsfájlműveletek listáján. A JSON hasznos adatokra a következő helyen talál példát: [https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 
hello telepítési parancsfájl hello a következő jellemzőkkel kell rendelkeznie:
1. Győződjön meg arról, hogy hello parancsfájl idempotent. Több hívások toohello parancsfájl kell előállítania hello ugyanazt az eredményt.
2. hello parancsfájl verziószámmal kell lennie. Amikor frissíti vagy módosítások tesztelését, hogy az ügyfelek számára, tooinstall hello alkalmazás próbál, nem érintettek használható hello parancsfájl egy másik helyre. 
3. Adja hozzá a megfelelő naplózási toohello parancsfájlok minden ponton. Általában a parancsfájl naplók hello hello csak úgy toodebug alkalmazás telepítési hibák vannak.
4. Győződjön meg arról, hogy hívások tooexternal services vagy az erőforrások megfelelő újrapróbálkozások, hogy hello telepítési átmeneti hálózati probléma nem érinti.
5. Ha a parancsfájl szolgáltatások hello csomóponton indul, győződjön meg arról, hogy hello szolgáltatások figyeli toostart csomópont újraindítások esetén automatikusan konfigurálva.

## <a name="package-application"></a>Alkalmazás becsomagolása
Hozzon létre egy zip fájlt, amely tartalmazza a HDInsight-alkalmazások telepítéséhez szükséges összes fájlt. A zip-fájl szükséges hello [alkalmazás közzététele](#publish-application).

* [createUiDefinition.json](#define-application).
* mainTemplate.json. Az [Install custom HDInsight applications](hdinsight-apps-install-custom-applications.md) (Egyéni HDInsight-alkalmazások telepítése) részben megtekinthet egy mintát.
* Minden kötelező parancsfájl.

> [!NOTE]
> hello az alkalmazás fájljai (ideértve webalkalmazás fájljait, ha van ilyen) bármely nyilvánosan elérhető végponton található.
> 

## <a name="publish-application"></a>Az alkalmazás közzététele
Hajtsa végre a következő lépéseket toopublish HDInsight-alkalmazások hello:

1. Jelentkezzen be toohello [Azure közzétételi portáljára](https://publish.windowsazure.com/).
2. Kattintson a **megoldás sablonok** hello bal oldali toocreate egy új megoldássablon az.
3. Adjon meg egy címet, és kattintson **Create a new solution template** (Új megoldássablon létrehozása) elemre.
4. Kattintson a **létrehozás fejlesztői központban regisztrált fiókjában és a csatlakozás hello Azure program** tooregister a vállalat, ha még nem tette meg.  Lásd: [Create a Microsoft Developer account](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md) (Microsoft Developer-fiók létrehozása).
5. Kattintson a **határozza meg az egyes topológiák tooget elindítva**. A megoldássablon van egy "szülőeleme" tooall továbbítja. Egy ajánlat/megoldássablonban több topológiát is megadhat. Amikor egy ajánlatot a rendszer előkészítésre toostaging, topológiájával együtt az összes hozzá tartozó topológia. 
6. Adja meg a topológia nevét, és kattintson a hello plusz jel.
7. Adjon meg egy új verziója, és kattintson a plusz jelre hello.
8. Feltöltés zip-fájl hello előkészített [alkalmazás becsomagolása](#package-application).  
9. Kattintson a **Request Certification** (Tanúsítvány kérése) gombra. hello Microsoft hitelesítő csapata áttekinti a hello fájlokat, és igazolja hello topológia.

## <a name="next-steps"></a>Következő lépések
* [HDInsight-alkalmazások telepítése](hdinsight-apps-install-applications.md): megtudhatja, hogyan tooinstall egy HDInsight-alkalmazás tooyour fürtök.
* [Egyéni HDInsight-alkalmazások telepítése](hdinsight-apps-install-custom-applications.md): megtudhatja, hogyan toodeploy egy közzé nem tett HDInsight-alkalmazás tooHDInsight.
* [Parancsfájlművelet Linux-alapú HDInsight-fürtök testreszabása](hdinsight-hadoop-customize-cluster-linux.md): megtudhatja, hogyan toouse parancsfájlművelet tooinstall további alkalmazásokat.
* [Linux-alapú Hadoop-fürtök létrehozása a Hdinsightban Azure Resource Manager-sablonok használatával](hdinsight-hadoop-create-linux-clusters-arm-templates.md): megtudhatja, hogyan toocall Resource Manager sablonok toocreate HDInsight-fürtök.
* [Üres peremhálózati csomópontok használata a Hdinsightban](hdinsight-apps-use-edge-node.md): megtudhatja, hogyan toouse egy üres él HDInsight-fürthöz való hozzáférés, a HDInsight-alkalmazások tesztelése és üzemeltetési csomópont HDInsight-alkalmazások.

