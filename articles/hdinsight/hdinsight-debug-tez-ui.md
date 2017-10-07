---
title: "Tez felhasználói felület Windows-alapú HDInsight - Azure aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Tez felhasználói felület toodebug Tez a Windows-alapú HDInsight HDInsight feladatokat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: a55bccb9-7c32-4ff2-b654-213a2354bd5c
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/17/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 7ae21242ee1f8dc34a8501bed1ca995480885540
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-tez-ui-toodebug-tez-jobs-on-windows-based-hdinsight"></a>A Windows-alapú HDInsight Tez felhasználói felület toodebug Tez feladatokhoz hello használata
Tez felhasználói felület hello egy weblap, amelyen a Tez használják hello végrehajtó motor a Windows-alapú HDInsight-fürtökön használt toounderstand és hibakeresési feladatokat is lehet. hello Tez felhasználói felület lehetővé teszi toovisualize hello feladat csatlakoztatott elemek grafikon elemezze az egyes elemek, valamint statisztikák és naplózási információk beolvasása.

> [!IMPORTANT]
> hello jelen dokumentumban leírt lépések egy HDInsight-fürt által használt Windows igényelnek. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="prerequisites"></a>Előfeltételek
* Egy Windows-alapú HDInsight-fürtöt. Az új fürt létrehozásának lépései: [Ismerkedés a Windows-alapú HDInsight eszközzel](hdinsight-hadoop-tutorial-get-started-windows.md).

  > [!IMPORTANT]
  > hello Tez felhasználói felület a 2016. február 8. után létrehozott Windows-alapú HDInsight-fürtök csak érhető el.
  >
  >
* A Windows-alapú távoli asztali ügyfél.

## <a name="understanding-tez"></a>Tez ismertetése
Tez egy bővíthető keretrendszer, amely a hagyományos MapReduce feldolgozási-nál nagyobb sebesség biztosítja az adatok feldolgozásához. A Windows-alapú HDInsight-fürtök egy nem kötelező motor, amely a Hive engedélyezheti a következő parancsot a Hive-lekérdezés részeként hello esetén:

    set hive.execution.engine=tez;

Elküldött tooTez munka esetén létrehoz egy irányított aciklikus diagramhoz (DAG), amely leírja a hello sorrendjének hello feladat által igényelt hello műveletek végrehajtása. Egyes műveletek csúcsban nevezik, és hajtható végre egy hello adat általános feladat. hello tényleges csúcspont szerint hello munkaelem végrehajtásának feladat neve és hello fürt több csomópontja között szétoszthatók.

### <a name="understanding-hello-tez-ui"></a>Understanding hello Tez felhasználói felület
Tez felhasználói felület hello egy weblap nyújt információkat folyamatok futó, és rendelkezik a korábban futtatott Tez használatával. Tez, által generált DAG tooview hello lehetővé teszi például a feladatok és a csúcsban és a hibainformációk által használt memória hogyan oszlik más fürtökre, teljesítményszámlálók. Felajánlhatja, hogy a következő forgatókönyvek hello hasznos információkat:

* Hosszan futó folyamatok figyelése, megtekintése hello térkép előrehaladását, és csökkentheti a feladatokat.
* Sikeres vagy sikertelen folyamatok toolearn előzményadatainak elemzése, hogyan lehet javítani, feldolgozás, vagy okát.

## <a name="generate-a-dag"></a>Egy dag-csoport létrehozása
hello Tez felhasználói felület csak fog adatokat tartalmazni, ha egy feladat által használt hello Tez motor éppen fut, vagy rendelkezik már futott az elmúlt hello. Egyszerű Hive-lekérdezések általában feloldható Tez, azonban összetett lekérdezések, amelyek szűrés, csoportosítás, rendezés, illesztéseket, stb. általában szükséges Tez használata nélkül.

A következő lépéseket toorun, amely végrehajtja a használatával Tez Hive-lekérdezések hello használata.

1. Egy böngészőben nyissa meg a toohttps://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello neve, a HDInsight-fürthöz.
2. Hello oldal hello tetején hello menüben válassza ki a hello **Hive szerkesztő**. Ekkor megjelenik egy lap a következő példalekérdezés hello.

        Select * from hivesampletable

    Hello példalekérdezés törlésére, és cserélje le a következő hello.

        set hive.execution.engine=tez;
        select market, state, country from hivesampletable where deviceplatform='Android' group by market, country, state;
3. Jelölje be hello **Submit** gombra. Hello **feladat munkamenet** szakasz alján hello hello hello lekérdezés hello állapotát jeleníti meg. Egyszer hello túl állapotmódosítások**befejezve**, jelölje be hello **részleteinek megtekintése** tooview hello eredmények hivatkozásra. Hello **Job Output** hasonló toohello következő legyen:

        en-GB   Hessen      Germany
        en-GB   Kingston    Jamaica
        en-GB   Nairobi Area    Kenya

## <a name="use-hello-tez-ui"></a>Hello Tez felhasználói felület használata
> [!NOTE]
> hello Tez felhasználói felület csak akkor hello asztali hello központi fürtcsomópont, így a távoli asztal tooconnect toohello átjárócsomópontokkal kell használni.
>
>

1. A hello [Azure-portálon](https://portal.azure.com), válassza ki a HDInsight-fürthöz. Hello hello HDInsight panel felső részén jelölje ki hello **távoli asztal** ikonra. Megjelenik az hello távoli asztali panel

    ![Távoli asztali ikon](./media/hdinsight-debug-tez-ui/remotedesktopicon.png)
2. Hello távoli asztal panelen válassza ki **Connect** tooconnect toohello fürt átjárócsomópontjából. Amikor a rendszer kéri, hello fürt távoli asztali felhasználói nevet és jelszót tooauthenticate hello kapcsolat használata.

    ![Távoli asztali kapcsolat ikon](./media/hdinsight-debug-tez-ui/remotedesktopconnect.png)

   > [!NOTE]
   > Ha nem engedélyezte a távoli asztali kapcsolat, adjon meg egy felhasználónevet, jelszót és a lejárati dátumot, majd válasszon **engedélyezése** tooenable távoli asztal. Ha engedélyezve van, a hello előző lépéseket tooconnect.
   >
   >
3. A csatlakozás után nyissa meg az Internet Explorer hello távoli asztal, hello jobb felső sarkában hello böngésző válassza hello fogaskerék ikonra, és válassza **kompatibilitási nézet beállításai**.
4. Hello aljáról **kompatibilitási nézet beállításai**, törölje a jelet hello jelölőnégyzetét **kompatibilitási nézetben jeleníti meg az intranetes helyek** és **használata Microsoft kompatibilitási lista**, Válassza ki és **Bezárás**.
5. Az Internet Explorerben, keresse meg a toohttp://headnodehost:8188/tezui / #/. Megjelenik az hello Tez felhasználói felület

    ![Tez felhasználói felület](./media/hdinsight-debug-tez-ui/tezui.png)

    Tez felhasználói felület hello betöltésekor látni fogja hello fürt futott, amely jelenleg fut, vagy dag listáját. hello az alapértelmezett nézet tartozik hello adatbázis-elérhetőségi csoport neve, azonosítója, küldő, állapot, Kezdés időpontja, befejezési idő, időtartam, Alkalmazásazonosító, és a várólista. További oszlopok is hozzáadhatók hello sarkában hello lap segítségével hello fogaskerék ikonra.

    Ha csak egy bejegyzést, az előző szakaszban hello futtatott hello lekérdezés lesz. Ha több bejegyzés, továbbra is beírva a keresési feltételek fent hello dag hello mezőket, majd kattintson a **Enter**.
6. Jelölje be hello **Dag neve** hello legutóbbi DAG bejegyzéshez. Ez megjeleníti hello DAG, valamint hello beállítás toodownload hello DAG információkat tartalmazó JSON-fájlok zip kapcsolatos információkat.

    ![DAG-részletek](./media/hdinsight-debug-tez-ui/dagdetails.png)
7. Hello fent **DAG részletek** több hivatkozást, amely lehet hello DAG használt toodisplay információ.

   * **DAG-számlálók** a DAG számlálók adatait jeleníti meg.
   * **Grafikus nézetének** jeleníti meg az adatbázis-elérhetőségi csoport grafikus ábrázolása.
   * **Minden csúcsban** a DAG hello csúcsban listáját jeleníti meg.
   * **Minden feladat** a DAG összes csúcsban hello feladatok listáját jeleníti meg.
   * **Minden TaskAttempts** hello részletes információit jeleníti meg az adatbázis-elérhetőségi csoport kísérletek toorun feladatok.

     > [!NOTE]
     > Ha hello oszlop megjelenítési csúcsban, feladatok és TaskAttempts, figyelje meg, hogy nincsenek hivatkozásait tooview **számlálók** és **megtekintése vagy naplók letöltéséhez** minden egyes sorára.
     >
     >

     Hiba történt a hello feldolgozással, ha a hello DAG részletek együtt hivatkozások tooinformation hello sikertelen feladathoz sikertelen, az állapot jelenik meg. Diagnosztikai adatok hello DAG Részletek alatt jelenik meg.
8. Válassza ki **grafikus nézetének**. Ez megjeleníti a hello DAG grafikus ábrázolása. Minden csomópont hello toodisplay adatainak megtekintése, a hello egér is helyezi.

    ![Grafikus megtekintése](./media/hdinsight-debug-tez-ui/dagdiagram.png)
9. Kattintson a csúcspont betölti a hello **csúcspont részletek** , a cikk. Kattintson a hello **térkép 1** csúcspont toodisplay részletek ehhez az elemhez. Válassza ki **megerősítése** tooconfirm hello navigációs.

    ![Csomópont részletei](./media/hdinsight-debug-tez-ui/vertexdetails.png)
10. Vegye figyelembe, hogy most már rendelkezik, amelyek kapcsolódó toovertices és feladatok hivatkozások hello oldal hello tetején.

    > [!NOTE]
    > Is is megérkezik a ezen a lapon térhet vissza túl**DAG részletek**választ, **csúcspont részletek**, és jelölje be hello **térkép 1** csúcspont.
    >
    >

    * **Csúcspont számlálók** a csúcspont számláló adatait jeleníti meg.
    * **Feladatok** a csúcspont feladatokat jeleníti meg.
    * **Feladat kísérletek** kísérletek toorun feladatok a csúcspont információit jeleníti meg.
    * **Adatforrások & fogadók esetében** adatforrások jeleníti meg, és ez a csomópont a fogadók esetében.

      > [!NOTE]
      > Hello előző menüben is görgetésekor hello oszlop megjelenítési feladatokhoz, a feladat kísérletet, és a források & Sinks__ toodisplay minden elem toomore adatai hivatkozásokat tartalmaz.
      >
      >
11. Válassza ki **feladatok**, majd jelölje be hello elem nevű és **00_000000**. Ez megjeleníti **feladat részletei** ehhez a feladathoz. Ez a képernyő megtekintheti **feladat számlálók** és **feladat kísérletek**.

    ![Feladat részletei](./media/hdinsight-debug-tez-ui/taskdetails.png)

## <a name="next-steps"></a>Következő lépések
Most, hogy megtanulta, hogyan toouse hello Tez meg, további információ [használata a HDInsight Hive](hdinsight-use-hive.md).

Részletesebb műszaki információkat Tez, lásd: hello [Hortonworks Tez lapon](http://hortonworks.com/hadoop/tez/).
