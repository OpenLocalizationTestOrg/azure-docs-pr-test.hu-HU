---
title: "során HDInsight Hive-könyvtárakhoz aaaAdd fürtlétrehozás - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooadd Hive-könyvtárakhoz (jar-fájlok), tooan HDInsight fürt fürt létrehozása során."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/12/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 2e028a07c3248205def0789af2c262a0774a8f19
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Adja hozzá az egyedi Hive-könyvtárakhoz, ha a HDInsight-fürt létrehozása

Ha a HDInsight Hive a gyakran használt könyvtárak, ez a dokumentum ismerteti a parancsfájlművelet toopre terhelésű hello könyvtárak használata a fürt létrehozása során. Ebben a dokumentumban hello lépések segítségével hozzáadott szalagtárak világszerte elérhető a Hive - van nincs szükség toouse [hozzáadása JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) tooload őket.

## <a name="how-it-works"></a>Működés

Amikor egy fürtöt hoz létre, opcionálisan megadhat egy parancsfájlt futtató művelet parancsfájl hello fürtcsomópontokon azok létrehozása közben. Ebben a dokumentumban hello parancsfájl egyetlen paramétert, és előre betöltött hello (tárolt jar-fájlok formájában) szalagtárak toobe tartalmazó WASB hely fogad el.

Fürt létrehozása során hello parancsfájl hello fájlok enumerálása, másolja őket toohello `/usr/lib/customhivelibs/` directory head és a munkavégző csomópont, majd hozzáadja őket toohello `hive.aux.jars.path` hello tulajdonság `core-site.xml` fájlt. Linux-alapú fürtökön is frissíti hello `hive-env.sh` hello fájlok hello helye fájlt.

> [!NOTE]
> Ebben a cikkben hello Parancsfájlműveletek segítségével hello szalagtárak elérhetővé teszi a következő forgatókönyvek hello:
>
> * **Linux-alapú HDInsight** – Ha egy Hive ügyfél használatával hello **WebHCat**, és **hiveserver2-n**.
> * **Windows-alapú HDInsight** – hello Hive ügyfél használatakor és **WebHCat**.

## <a name="hello-script"></a>hello parancsfájl

**Parancsfájl helyét**

A **Linux-alapú fürtökön**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

A **Windows-alapú fürtök**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).

**Követelmények**

* hello parancsfájlok kell alkalmazott tooboth hello **Átjárócsomópontokat** és **munkavégző csomópontokhoz**.

* hello JAR-fájlok kivételével tooinstall Azure Blob Storage-ban kell tárolni kívánja a **egyetlen tároló**.

* hello tárfiókot tartalmazó jar-fájlok hello könyvtár **kell** kell csatolt toohello HDInsight-fürt létrehozása során. Vagy hello alapértelmezett tárfiókot kell lennie, vagy a fiók hozzáadva __opcionális konfigurációs__.

* a paraméter toohello parancsfájlművelet hello WASB elérési toohello tárolót kell megadni. Például ha hello JAR-fájlok kivételével tárolódnak nevű tárolót **függvénytárak** egy tárfiókon nevű **mystorage**, hello paraméter lenne  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Jelen dokumentum céljából feltételezzük, hogy rendelkezik már hozzon létre egy tárfiókot, a blob-tároló és a feltöltött hello fájlok tooit.
  >
  > Ha nem hozott létre egy tárfiókot, ehhez keresztül hello [Azure-portálon](https://portal.azure.com). Ezután egy segédprogramot használhatja például a [Azure Tártallózó](http://storageexplorer.com/) toocreate egy tároló hello fiók- és feltöltése az fájlok tooit.

## <a name="create-a-cluster-using-hello-script"></a>Hozzon létre egy fürtöt hello parancsfájl használatával

> [!NOTE]
> a lépéseket követve hello hozzon létre egy Linux-alapú HDInsight-fürtöt. egy Windows-alapú fürt toocreate kiválasztása **Windows** , hello fürt operációs rendszer hello fürt létrehozásakor, és hello (PowerShell) a Windows parancsfájl használata helyett hello bash parancsfájlok.
>
> Azure PowerShell vagy hello HDInsight .NET SDK toocreate ezt a parancsfájlt a fürt is használja. A következő módszerekkel további információkért lásd: [testreszabása HDInsight-fürtök parancsfájlműveletekkel](hdinsight-hadoop-customize-cluster-linux.md).

1. Fürt elkezdhessen a hello lépések segítségével [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md), de kiépítése nem fejeződött be.

2. A hello **opcionális konfigurációs** panelen válassza **Parancsfájlműveletek**, és adja meg a következő információ hello:

   * **NÉV**: Adjon meg egy rövid nevet hello parancsfájlművelet.

   * **PARANCSFÁJL URI azonosítója**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: ezt a beállítást.

   * **MUNKAVÉGZŐ**: ezt a beállítást.

   * **ZOOKEEPER**: hagyja üresen a mezőt.

   * **PARAMÉTEREK**: Adja meg a hello WASB cím toohello tároló és a tárolási fiók, amely tartalmazza a hello JAR-fájlok kivételével. Például  **wasb://libs@mystorage.blob.core.windows.net/** .

3. Hello hello alján **Parancsfájlműveletek**, hello használata **válasszon** gombok toosave hello beállítása.

4. A hello **opcionális konfigurációs** panelen válassza **a társított Tárfiókokban** és select hello **tárolási kulcs hozzáadása** hivatkozásra. Jelöljön ki, amely tartalmazza a hello JAR-fájlok kivételével hello tárfiók, és aztán hello **válasszon** gombok toosave beállításokat és a visszatérési hello **opcionális konfigurációs** panelen.

5. Használja hello **kiválasztása** hello hello alján gomb **opcionális konfigurációs** panel toosave hello opcionális konfigurációs adatait.

6. Kiépítés hello fürt, a folytatáshoz [Provision HDInsight-fürtök Linux rendszeren](hdinsight-hadoop-provision-linux-clusters.md).

Miután befejeződött a fürt létrehozása,-e képes toouse hello JAR-fájlok kivételével Hive hozzá ezt a parancsfájlt keresztül anélkül, hogy toouse hello `ADD JAR` utasítást.

## <a name="next-steps"></a>Következő lépések

Hive munkavégzés további információkért lásd: [hdinsight Hive használata](hdinsight-use-hive.md)
