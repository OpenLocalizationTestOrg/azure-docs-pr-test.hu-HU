---
title: "aaaUse Apache Phoenix és a Windows-alapú Azure HDInsight SQuirreL |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Apache Phoenix a Hdinsightban, és hogyan tooinstall és a munkaállomás tooconnect tooan hdinsight HBase fürt SQuirreL konfigurálása."
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 1a756e98-75c1-44cd-a178-c5119683b7b7
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 147ac35fa882fd1bedbc5361ac804c36a4d56de1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a>Apache Phoenix és az SQuirreL használata a Hdinsightban Windows-alapú HBase-fürtökkel
Megtudhatja, hogyan toouse [Apache Phoenix](http://phoenix.apache.org/) a Hdinsightban, és hogyan tooinstall és a munkaállomás tooconnect tooan hdinsight HBase fürt SQuirreL konfigurálása. Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html). Hello Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

> [!NOTE]
> Hello Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md).
>

> [!IMPORTANT]
> Ez a dokumentum a Windows-alapú HDInsight-fürtök csak munkahelyi hello szükséges lépések. HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához. Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója. További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement). Phoenix a Linux-alapú HDInsight használatáról további információért lásd: [használata Apache Phoenix a Linux-alapú HBase a HDInsight clusters](hdinsight-hbase-phoenix-squirrel-linux.md).
>



## <a name="use-sqlline"></a>SQLLine használata
[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram tooexecute SQL.

### <a name="prerequisites"></a>Előfeltételek
SQLLine használata előtt hello következő kell rendelkeznie:

* **A hdinsight HBase-fürtöt**. HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].
* **Csatlakozás toohello HBase-fürtöt hello távoli asztal protokollal**. Útmutatásért lásd: [hello klasszikus Azure portál használatával hdinsight fürtök kezelése Hadoop][hdinsight-manage-portal].

**kimenő hello állomásnév toofind**

1. Nyissa meg **Hadoop parancssori** hello asztalról.
2. Futtassa a következő parancs tooget hello DNS-utótag hello:

        ipconfig

    Írja le **kapcsolatspecifikus DNS-utótag**. Például *myhbasecluster.f5.internal.cloudapp.net*. Tooan HBase-fürtöt csatlakoztatásakor szüksége lesz a teljes tartománynév használatával hello Zookeepers tooconnect tooone. Minden egyes HDInsight-fürt 3 Zookeepers rendelkezik. Ezek *zookeeper0*, *zookeeper1*, és *zookeeper2*. hello FQDN lesz hasonlót *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.

**toouse SQLLine**

1. Nyissa meg **Hadoop parancssori** hello asztalról.
2. Futtassa a következő parancsok tooopen SQLLine hello:

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    hello mintában használt hello parancsok:

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).

## <a name="use-squirrel"></a>SQuirreL használata
[SQL-ügyfél sQuirreL](http://squirrel-sql.sourceforge.net/) grafikus Java-program hozhat létre, amely lehetővé teszi olyan JDBC-kompatibilis adatbázisokba tooview hello szerkezete, keresse meg a táblázatok hello adatait, adja ki az SQL-parancsok stb. A HDInsight Phoenix használt tooconnect tooApache lehet.

Ez a szakasz bemutatja, hogyan tooinstall és SQuirreL konfigurálása a munkaállomás tooconnect tooan HBase fürt a Hdinsightban VPN-en keresztül.

### <a name="prerequisites"></a>Előfeltételek
A következő hello eljárásokat, mielőtt hello következő kell rendelkeznie:

* HBase-fürtöt telepített tooan Azure virtuális hálózat DNS-virtuális géphez.  Útmutatásért lásd: [hozzon létre HBase-fürtök Azure virtuális hálózat][hdinsight-hbase-provision-vnet].

* Hello HBase fürt fürt kapcsolatspecifikus DNS-utótag első. tooget az RDP hello fürtbe, majd futtassa az IPConfig.  hello DNS-utótag hasonlít:

        myhbase.b7.internal.cloudapp.net
* Töltse le és telepítse [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) a munkaállomáson. Be kell szereznie a hello csomag toocreate makecert a tanúsítványt.  
* Töltse le és telepítse [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) a munkaállomáson.  SQuirreL SQL ügyfélverzió 3.0-s és újabb rendszer JRE 1.6 vagy újabb verziója szükséges.  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a>Egy pont – hely típusú VPN-kapcsolat toohello Azure-beli virtuális hálózat konfigurálása
Nincsenek érintett pont-pont VPN-kapcsolat konfigurálása 3 lépést:

1. [Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [A tanúsítványok létrehozása](#Create-your-certificates)
3. [A VPN-ügyfél konfigurálása](#Configure-your-VPN-client)

Lásd: [egy pont – hely típusú VPN-kapcsolat tooan Azure virtuális hálózat konfigurálása](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a>Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása
Biztosítja a HBase-fürtöt egy Azure virtuális hálózatban ellátta (lásd az ebben a szakaszban hello előfeltételei). következő lépés hello tooconfigure egy pont – hely kapcsolat.

**tooconfigure hello pont – hely kapcsolat**

1. Jelentkezzen be toohello [klasszikus Azure portál][azure-portal].
2. Hello bal oldalon kattintson **hálózatok**.
3. Kattintson a létrehozott virtuális hálózat hello (lásd: [rendelkezés HBase clusters Azure virtuális hálózat][hdinsight-hbase-provision-vnet]).
4. Kattintson a **KONFIGURÁLÁSA** hello elejéről.
5. A hello **pont – hely kapcsolat** szakaszban jelölje be **pont – hely kapcsolat konfigurálása**.
6. Konfigurálása **KEZDŐ IP-** és **CIDR** toospecify hello IP-címtartomány, amelyből a VPN-ügyfelek olyan IP-címet kap cím csatlakozáskor. hello tartomány bármely hello a helyszíni található tartományokat a hálózati és hello Azure virtuális hálózatra való kapcsolódás nem lehet átfedésben. Például. Ha 10.0.0.0/20 hello virtuális hálózat, kiválaszthatja a hello ügyfél címterület 10.1.0.0/24. Lásd: hello [pont – hely kapcsolat] [ vnet-point-to-site-connectivity] olvashat.
7. Hello virtuális hálózati cím szóközöket szakaszban kattintson **átjáró alhálózatának hozzáadása**.
8. Kattintson a **mentése** a hello hello lap alján.
9. Kattintson a **Igen** tooconfirm hello módosítása. Várjon, amíg hello rendszer befejezte, így módosíthatja, mielőtt továbblépne a következő eljárással toohello hello.

**a dinamikus útválasztó átjáró toocreate**

1. A klasszikus Azure portál hello, kattintson az **IRÁNYÍTÓPULT** hello lap hello elejéről.
2. Kattintson a **ÁTJÁRÓ létrehozása** hello lap hello lista aljáról.
3. Kattintson a **Igen** tooconfirm. Várjon, amíg hello átjáró létrehozása.
4. Kattintson a **IRÁNYÍTÓPULT** hello elejéről.  Hello virtuális hálózati visual ábrázoló diagram jelenik meg:

    ![Azure-beli virtuális hálózat pont-pont virtuális diagramja][img-vnet-diagram]

    hello ábrán 0 ügyfélkapcsolatokat. Miután elvégezte a kapcsolat toohello virtuális hálózat, hello lesz frissített tooone.

#### <a name="create-your-certificates"></a>A tanúsítványok létrehozása
Egy X.509 tanúsítvány használatával van egyirányú toocreate hello tanúsítvány létrehozási eszköz (makecert.exe) a [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).

**toocreate önaláírt legfelső szintű tanúsítvány**

1. A munkaállomáson nyisson meg egy parancssori ablakot.
2. Keresse meg a toohello Visual Studio eszközök mappában.
3. hello hello az alábbi példában lévő parancs a következő hozzon létre és telepítsen egy legfelső szintű tanúsítványt hello személyes tanúsítványtárolójába a munkaállomáson és a megfelelő .cer fájlt, hogy később fogja feltölteni toohello klasszikus Azure portálon is létrehozhat.

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    Módosítsa a használni kívánt .cer fájl toobe található, ahol HBaseVnetVPNRootCertificate az hello nevét, amelyet az toouse hello tanúsítvány hello toohello-könyvtárat.

    Ne zárja be a hello parancssort.  Szüksége lesz rájuk a következő eljárással hello.

   > [!NOTE]
   > Létrehozott egy legfelső szintű tanúsítványt, amelyből ügyféltanúsítványok jön létre, mert tooexport szeretné ezt a tanúsítványt és annak titkos kulcsát, és tooa biztonságos helyre, ahol lehetséges, hogy helyre mentse.
   >
   >

**toocreate ügyféltanúsítványt**

* A hello azonos parancssort (toobe rendelkezzen hello ugyanazon a számítógépen, amelyen létrehozta hello legfelső szintű tanúsítvány. hello ügyféltanúsítvány generálása származó hello főtanúsítvány), futtatási hello következő parancsot:

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    HBaseVnetVPNRootCertificate hello legfelső szintű tanúsítvány neve.  Toomatch hello legfelső szintű tanúsítvány névvel rendelkezik.  

    A számítógép személyes tanúsítványtárolójában hello legfelső szintű tanúsítvány és a hello ügyféltanúsítvány vannak tárolva. Certmgr.msc tooverify használja.

    ![Azure-beli virtuális hálózat pont-pont VPN-tanúsítványának][img-certificate]

    Ügyféltanúsítvány telepítenie kell minden olyan számítógépre, amelyet az tooconnect toohello virtuális hálózat. Azt javasoljuk, hogy hozzon létre egyedi ügyfél számítógépenként, amelyet az tooconnect toohello virtuális hálózati tanúsítványok. tooexport hello ügyféltanúsítványok certmgr.msc használja.

**tooupload hello legfelső szintű tanúsítvány toohello klasszikus Azure portálon**

1. A klasszikus Azure portál hello, kattintson az **hálózati** hello bal oldalon.
2. Kattintson a hello virtuális hálózat, amelyen üzembe van helyezve a HBase-fürtöt a.
3. Kattintson a **tanúsítványok** hello elejéről.
4. Kattintson a **FELTÖLTÉSE** hello az alsó, és adja meg a hello főtanúsítványfájlt hello eljárás utolsó előtt létrehozott. Várjon, amíg hello tanúsítvány lett importálva.
5. Kattintson a **IRÁNYÍTÓPULT** hello felül.  hello virtuális diagram hello állapotát jeleníti meg.

#### <a name="configure-your-vpn-client"></a>A VPN-ügyfél konfigurálása
**toodownload és a telepítés hello ügyfél VPN-csomag**

1. Az IRÁNYÍTÓPULT-oldalon hello hello virtuális hálózat hello gyors áttekintő területen jelölje be az **letöltési hello 64 bites ügyfél VPN-csomag** vagy **letöltési hello 32-bit-es VPN ügyfélcsomag** alapján a munkaállomást az operációs rendszer verziója.
2. Kattintson a **futtatása** tooinstall hello csomag.
3. A parancssorból hello biztonsági kattintson **további információ**, és kattintson a **mégis futtatni**.
4. Kattintson a **Igen** kétszer.

**tooconnect tooVPN**

1. A munkaállomás hello asztalon kattintson a tálcán hello hello hálózatok ikonjára. A virtuális hálózati név látni fog egy VPN-kapcsolatot.
2. Kattintson a hello VPN-kapcsolat neve.
3. Kattintson a **Connect** (Csatlakozás) gombra.

**VPN-kapcsolat és a tartomány névfeloldás tootest hello**

* Hello munkaállomásról, nyisson meg egy parancssort, és ping a következő nevekre hello HBase fürt DNS-utótag hello myhbase.b7.internal.cloudapp.net:

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a>Telepítse és konfigurálja az SQuirreL a munkaállomáson
**tooinstall SQuirreL**

1. Töltse le a hello SQuirreL SQL ügyfél jar-fájlra a [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).
2. Megnyitás/futtatás hello jar-fájlra. Hello igényel [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).
3. Kattintson a **következő** kétszer.
4. Adjon meg egy elérési utat, melyekben hello írási engedélye, és kattintson a **következő**.

  > [!NOTE]
  > hello alapértelmezett telepítési mappa hello C:\Program Files\squirrel-sql-3.6 mappában van.  Az order toowrite toothis elérési út hello telepítő joggal kell hello rendszergazda. Nyisson meg egy parancssort rendszergazdaként, keresse meg a tooJava a bin mappát, és futtassa:
  >
  >     Java.exe-jar [hello elérési útja hello SQuirreL jar-fájlt]
5. Kattintson a **OK** tooconfirm hello cél könyvtár létrehozása.
6. hello alapértéke tooinstall hello kiinduló és a szabványos csomagok.  Kattintson a **Tovább** gombra.
7. Kattintson a **következő** kétszer, majd **végzett**.

**tooinstall hello Phoenix illesztőprogram**

hello phoenix illesztőprogram jar-fájlra található hello HBase-fürtöt. elérési út hello hasonló toohello következő hello verziók alapján:

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
Toocopy kell azt tooyour munkaállomás alatt hello [SQuirreL telepítési mappa] / lib elérési útja.  hello legegyszerűbb módja a tooRDP hello fürt, majd a fájlt másolja és illessze be (CTRL + C és CTRL + V) toocopy használja azt tooyour munkaállomásra.

**a Phoenix illesztőprogram tooSQuirreL tooadd**

1. Nyissa meg a SQuirreL SQL ügyfél a munkaállomáson.
2. Kattintson a hello **illesztőprogram** hello bal oldali lapon.
3. A hello **illesztőprogramok** menüben kattintson a **új illesztőprogram**.
4. Adja meg a következő információ hello:

   * **Név**: Phoenix
   * **Példa URL-cím**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net
   * **Osztálynév**: org.apache.phoenix.jdbc.PhoenixDriver

     > [!WARNING]
     > Felhasználói hello példa URL-címet az összes kisbetű. Használhatja azokat teljes zookeeper kvórum abban az esetben, ha ezek egyikét nem működik.  hello állomásnevek zookeeper0 zookeeper1 és zookeeper2.
     >
     >

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-driver]
5. Kattintson az **OK** gombra.

**toocreate alias toohello HBase-fürtöt**

1. A SQuirreL, kattintson a hello **aliasok** hello bal oldali lapon.
2. A hello **aliasok** menüben kattintson a **új Alias**.
3. Adja meg a következő információ hello:

   * **Név**: hello HBase-fürtöt, vagy inkább nevek hello nevét.
   * **Illesztőprogram**: Phoenix.  Ennek egyeznie kell a hello utolsó eljárásban létrehozott hello illesztőprogram neve.
   * **URL-cím**: hello URL-címet a rendszer átmásolja az illesztőprogram-konfigurációjáról. Győződjön meg arról, hogy toouser összes kisbetű.
   * **Felhasználónév**: szöveg lehet.  Itt adhat meg VPN-kapcsolatot, mert hello felhasználónév nem minden használja.
   * **Jelszó**: szöveg lehet.

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-alias]
4. Kattintson a **teszt**.
5. Kattintson a **Connect** (Csatlakozás) gombra. Ha hello kapcsolat, SQuirreL néz ki:

    ![A HBase Phoenix SQuirreL][img-squirrel]

**egy teszt toorun**

1. Kattintson a hello **SQL** jobb következő toohello lapon **objektumok** lapon.
2. Másolja és illessze be a kódját a következő hello:

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. Hello Futtatás gombra.

    ![A HBase Phoenix SQuirreL][img-squirrel-sql]
4. Váltson vissza toohello **objektumok** fülre.
5. Bontsa ki a hello aliasnév, és végül **tábla**.  Ekkor megjelenik a hello tartozó új tábla.

## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan toouse Apache Phoenix a hdinsight eszközben.  toolearn több, lásd:

* [HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.
* [Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, a HBase-fürtökkel telepíthetők telepített toohello azonos virtuális hálózati alkalmazásaihoz, így alkalmazások kommunikálhatnak A HBase közvetlenül.
* [A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): megtudhatja, hogyan tooconfigure HBase-replikálás két Azure üzemeltetésében.
* [Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: megtudhatja, hogyan valós idejű toodo [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.

[azure-portal]: https://portal.azure.com
[vnet-point-to-site-connectivity]: https://msdn.microsoft.com/library/azure/09926218-92ab-4f43-aa99-83ab4d355555#BKMK_VNETPT

[hdinsight-versions]: hdinsight-component-versioning.md
[hdinsight-hbase-get-started]: hdinsight-hbase-tutorial-get-started.md
[hdinsight-manage-portal]: hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp
[hdinsight-hbase-provision-vnet]: hdinsight-hbase-provision-vnet.md
[hdinsight-hbase-overview]: hdinsight-hbase-overview.md
[hbase-twitter-sentiment]: hdinsight-hbase-analyze-twitter-sentiment.md

[hdinsight-hbase-phoenix-sqlline]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-phoenix-sqlline.png
[img-certificate]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vpn-certificate.png
[img-vnet-diagram]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-vnet-point-to-site.png
[img-squirrel-driver]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-driver.png
[img-squirrel-alias]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-alias.png
[img-squirrel]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel.png
[img-squirrel-sql]: ./media/hdinsight-hbase-phoenix-squirrel/hdinsight-hbase-squirrel-sql.png
