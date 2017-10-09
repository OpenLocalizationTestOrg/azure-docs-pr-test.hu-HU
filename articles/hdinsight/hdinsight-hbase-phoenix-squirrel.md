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
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="84bad-103">Apache Phoenix és az SQuirreL használata a Hdinsightban Windows-alapú HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="84bad-103">Use Apache Phoenix and SQuirreL with Windows-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="84bad-104">Megtudhatja, hogyan toouse [Apache Phoenix](http://phoenix.apache.org/) a Hdinsightban, és hogyan tooinstall és a munkaállomás tooconnect tooan hdinsight HBase fürt SQuirreL konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84bad-104">Learn how toouse [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight.</span></span> <span data-ttu-id="84bad-105">Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="84bad-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="84bad-106">Hello Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="84bad-106">For hello Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="84bad-107">Hello Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított hello Hadoop-fürt verziók?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="84bad-107">For hello Phoenix version information in HDInsight, see [What's new in hello Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>

> [!IMPORTANT]
> <span data-ttu-id="84bad-108">Ez a dokumentum a Windows-alapú HDInsight-fürtök csak munkahelyi hello szükséges lépések.</span><span class="sxs-lookup"><span data-stu-id="84bad-108">hello steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="84bad-109">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="84bad-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="84bad-110">Linux hello azt az egyetlen operációs rendszer, használja a HDInsight 3.4 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="84bad-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="84bad-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="84bad-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="84bad-112">Phoenix a Linux-alapú HDInsight használatáról további információért lásd: [használata Apache Phoenix a Linux-alapú HBase a HDInsight clusters](hdinsight-hbase-phoenix-squirrel-linux.md).</span><span class="sxs-lookup"><span data-stu-id="84bad-112">For information on using Phoenix on Linux-based HDInsight, see [Use Apache Phoenix with Linux-based HBase clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span></span>
>



## <a name="use-sqlline"></a><span data-ttu-id="84bad-113">SQLLine használata</span><span class="sxs-lookup"><span data-stu-id="84bad-113">Use SQLLine</span></span>
<span data-ttu-id="84bad-114">[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram tooexecute SQL.</span><span class="sxs-lookup"><span data-stu-id="84bad-114">[SQLLine](http://sqlline.sourceforge.net/) is a command line utility tooexecute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="84bad-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84bad-115">Prerequisites</span></span>
<span data-ttu-id="84bad-116">SQLLine használata előtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="84bad-116">Before you can use SQLLine, you must have hello following:</span></span>

* <span data-ttu-id="84bad-117">**A hdinsight HBase-fürtöt**.</span><span class="sxs-lookup"><span data-stu-id="84bad-117">**A HBase cluster in HDInsight**.</span></span> <span data-ttu-id="84bad-118">HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="84bad-118">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="84bad-119">**Csatlakozás toohello HBase-fürtöt hello távoli asztal protokollal**.</span><span class="sxs-lookup"><span data-stu-id="84bad-119">**Connect toohello HBase cluster via hello remote desktop protocol**.</span></span> <span data-ttu-id="84bad-120">Útmutatásért lásd: [hello klasszikus Azure portál használatával hdinsight fürtök kezelése Hadoop][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="84bad-120">For instructions, see [Manage Hadoop clusters in HDInsight by using hello Azure Classic Portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="84bad-121">**kimenő hello állomásnév toofind**</span><span class="sxs-lookup"><span data-stu-id="84bad-121">**toofind out hello host name**</span></span>

1. <span data-ttu-id="84bad-122">Nyissa meg **Hadoop parancssori** hello asztalról.</span><span class="sxs-lookup"><span data-stu-id="84bad-122">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="84bad-123">Futtassa a következő parancs tooget hello DNS-utótag hello:</span><span class="sxs-lookup"><span data-stu-id="84bad-123">Run hello following command tooget hello DNS suffix:</span></span>

        ipconfig

    <span data-ttu-id="84bad-124">Írja le **kapcsolatspecifikus DNS-utótag**.</span><span class="sxs-lookup"><span data-stu-id="84bad-124">Write down **Connection-specific DNS Suffix**.</span></span> <span data-ttu-id="84bad-125">Például *myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="84bad-125">For example, *myhbasecluster.f5.internal.cloudapp.net*.</span></span> <span data-ttu-id="84bad-126">Tooan HBase-fürtöt csatlakoztatásakor szüksége lesz a teljes tartománynév használatával hello Zookeepers tooconnect tooone.</span><span class="sxs-lookup"><span data-stu-id="84bad-126">When you connect tooan HBase cluster, you will need tooconnect tooone of hello Zookeepers using FQDN.</span></span> <span data-ttu-id="84bad-127">Minden egyes HDInsight-fürt 3 Zookeepers rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="84bad-127">Each HDInsight cluster has 3 Zookeepers.</span></span> <span data-ttu-id="84bad-128">Ezek *zookeeper0*, *zookeeper1*, és *zookeeper2*.</span><span class="sxs-lookup"><span data-stu-id="84bad-128">They are *zookeeper0*, *zookeeper1*, and *zookeeper2*.</span></span> <span data-ttu-id="84bad-129">hello FQDN lesz hasonlót *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="84bad-129">hello FQDN will be something like *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span></span>

<span data-ttu-id="84bad-130">**toouse SQLLine**</span><span class="sxs-lookup"><span data-stu-id="84bad-130">**toouse SQLLine**</span></span>

1. <span data-ttu-id="84bad-131">Nyissa meg **Hadoop parancssori** hello asztalról.</span><span class="sxs-lookup"><span data-stu-id="84bad-131">Open **Hadoop Command Line** from hello desktop.</span></span>
2. <span data-ttu-id="84bad-132">Futtassa a következő parancsok tooopen SQLLine hello:</span><span class="sxs-lookup"><span data-stu-id="84bad-132">Run hello following commands tooopen SQLLine:</span></span>

        cd %phoenix_home%\bin
        sqlline.py [hello FQDN of one of hello Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    <span data-ttu-id="84bad-134">hello mintában használt hello parancsok:</span><span class="sxs-lookup"><span data-stu-id="84bad-134">hello commands used in hello sample:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

<span data-ttu-id="84bad-135">További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="84bad-135">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="use-squirrel"></a><span data-ttu-id="84bad-136">SQuirreL használata</span><span class="sxs-lookup"><span data-stu-id="84bad-136">Use SQuirreL</span></span>
<span data-ttu-id="84bad-137">[SQL-ügyfél sQuirreL](http://squirrel-sql.sourceforge.net/) grafikus Java-program hozhat létre, amely lehetővé teszi olyan JDBC-kompatibilis adatbázisokba tooview hello szerkezete, keresse meg a táblázatok hello adatait, adja ki az SQL-parancsok stb. A HDInsight Phoenix használt tooconnect tooApache lehet.</span><span class="sxs-lookup"><span data-stu-id="84bad-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is a graphical Java program that will allow you tooview hello structure of a JDBC compliant database, browse hello data in tables, issue SQL commands etc. It can be used tooconnect tooApache Phoenix on HDInsight.</span></span>

<span data-ttu-id="84bad-138">Ez a szakasz bemutatja, hogyan tooinstall és SQuirreL konfigurálása a munkaállomás tooconnect tooan HBase fürt a Hdinsightban VPN-en keresztül.</span><span class="sxs-lookup"><span data-stu-id="84bad-138">This section shows you how tooinstall and configure SQuirreL on your workstation tooconnect tooan HBase cluster in HDInsight via VPN.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="84bad-139">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="84bad-139">Prerequisites</span></span>
<span data-ttu-id="84bad-140">A következő hello eljárásokat, mielőtt hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="84bad-140">Before following hello procedures, you must have hello following:</span></span>

* <span data-ttu-id="84bad-141">HBase-fürtöt telepített tooan Azure virtuális hálózat DNS-virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="84bad-141">An HBase cluster deployed tooan Azure virtual network with a DNS virtual machine.</span></span>  <span data-ttu-id="84bad-142">Útmutatásért lásd: [hozzon létre HBase-fürtök Azure virtuális hálózat][hdinsight-hbase-provision-vnet].</span><span class="sxs-lookup"><span data-stu-id="84bad-142">For instructions, see [Create HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet].</span></span>

* <span data-ttu-id="84bad-143">Hello HBase fürt fürt kapcsolatspecifikus DNS-utótag első.</span><span class="sxs-lookup"><span data-stu-id="84bad-143">Get hello HBase cluster cluster Connection-specific DNS suffix.</span></span> <span data-ttu-id="84bad-144">tooget az RDP hello fürtbe, majd futtassa az IPConfig.</span><span class="sxs-lookup"><span data-stu-id="84bad-144">tooget it, RDP into hello cluster, and then run IPConfig.</span></span>  <span data-ttu-id="84bad-145">hello DNS-utótag hasonlít:</span><span class="sxs-lookup"><span data-stu-id="84bad-145">hello DNS suffix is similar to:</span></span>

        myhbase.b7.internal.cloudapp.net
* <span data-ttu-id="84bad-146">Töltse le és telepítse [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="84bad-146">Download and install [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) on your workstation.</span></span> <span data-ttu-id="84bad-147">Be kell szereznie a hello csomag toocreate makecert a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="84bad-147">You will need makecert from hello package toocreate your certificate.</span></span>  
* <span data-ttu-id="84bad-148">Töltse le és telepítse [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="84bad-148">Download and install [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) on your workstation.</span></span>  <span data-ttu-id="84bad-149">SQuirreL SQL ügyfélverzió 3.0-s és újabb rendszer JRE 1.6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="84bad-149">SQuirreL SQL client version 3.0 and higher requires JRE version 1.6 or higher.</span></span>  

### <a name="configure-a-point-to-site-vpn-connection-toohello-azure-virtual-network"></a><span data-ttu-id="84bad-150">Egy pont – hely típusú VPN-kapcsolat toohello Azure-beli virtuális hálózat konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84bad-150">Configure a Point-to-Site VPN connection toohello Azure virtual network</span></span>
<span data-ttu-id="84bad-151">Nincsenek érintett pont-pont VPN-kapcsolat konfigurálása 3 lépést:</span><span class="sxs-lookup"><span data-stu-id="84bad-151">There are 3 steps involved configuring a point-to-site VPN connection:</span></span>

1. [<span data-ttu-id="84bad-152">Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84bad-152">Configure a virtual network and a dynamic routing gateway</span></span>](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [<span data-ttu-id="84bad-153">A tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="84bad-153">Create your certificates</span></span>](#Create-your-certificates)
3. [<span data-ttu-id="84bad-154">A VPN-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84bad-154">Configure your VPN client</span></span>](#Configure-your-VPN-client)

<span data-ttu-id="84bad-155">Lásd: [egy pont – hely típusú VPN-kapcsolat tooan Azure virtuális hálózat konfigurálása](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="84bad-155">See [Configure a Point-to-Site VPN connection tooan Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a><span data-ttu-id="84bad-156">Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84bad-156">Configure a virtual network and a dynamic routing gateway</span></span>
<span data-ttu-id="84bad-157">Biztosítja a HBase-fürtöt egy Azure virtuális hálózatban ellátta (lásd az ebben a szakaszban hello előfeltételei).</span><span class="sxs-lookup"><span data-stu-id="84bad-157">Assure you have provisioned an HBase cluster in an Azure virtual network (see hello prerequisites for this section).</span></span> <span data-ttu-id="84bad-158">következő lépés hello tooconfigure egy pont – hely kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="84bad-158">hello next step is tooconfigure a point-to-site connection.</span></span>

<span data-ttu-id="84bad-159">**tooconfigure hello pont – hely kapcsolat**</span><span class="sxs-lookup"><span data-stu-id="84bad-159">**tooconfigure hello point-to-site connectivity**</span></span>

1. <span data-ttu-id="84bad-160">Jelentkezzen be toohello [klasszikus Azure portál][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="84bad-160">Sign in toohello [Azure Classic Portal][azure-portal].</span></span>
2. <span data-ttu-id="84bad-161">Hello bal oldalon kattintson **hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="84bad-161">On hello left, click **NETWORKS**.</span></span>
3. <span data-ttu-id="84bad-162">Kattintson a létrehozott virtuális hálózat hello (lásd: [rendelkezés HBase clusters Azure virtuális hálózat][hdinsight-hbase-provision-vnet]).</span><span class="sxs-lookup"><span data-stu-id="84bad-162">Click hello virtual network you have created (see [Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span></span>
4. <span data-ttu-id="84bad-163">Kattintson a **KONFIGURÁLÁSA** hello elejéről.</span><span class="sxs-lookup"><span data-stu-id="84bad-163">Click **CONFIGURE** from hello top.</span></span>
5. <span data-ttu-id="84bad-164">A hello **pont – hely kapcsolat** szakaszban jelölje be **pont – hely kapcsolat konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="84bad-164">In hello **point-to-site connectivity** section, select **Configure point-to-site connectivity**.</span></span>
6. <span data-ttu-id="84bad-165">Konfigurálása **KEZDŐ IP-** és **CIDR** toospecify hello IP-címtartomány, amelyből a VPN-ügyfelek olyan IP-címet kap cím csatlakozáskor.</span><span class="sxs-lookup"><span data-stu-id="84bad-165">Configure **STARTING IP** and **CIDR** toospecify hello IP address range from which your VPN clients will receive an IP address when connected.</span></span> <span data-ttu-id="84bad-166">hello tartomány bármely hello a helyszíni található tartományokat a hálózati és hello Azure virtuális hálózatra való kapcsolódás nem lehet átfedésben.</span><span class="sxs-lookup"><span data-stu-id="84bad-166">hello range cannot overlap with any of hello ranges located on your on-premises network and hello Azure virtual network you will be connecting to.</span></span> <span data-ttu-id="84bad-167">Például.</span><span class="sxs-lookup"><span data-stu-id="84bad-167">For example.</span></span> <span data-ttu-id="84bad-168">Ha 10.0.0.0/20 hello virtuális hálózat, kiválaszthatja a hello ügyfél címterület 10.1.0.0/24.</span><span class="sxs-lookup"><span data-stu-id="84bad-168">if you selected 10.0.0.0/20 for hello virtual network, you can select 10.1.0.0/24 for hello client address space.</span></span> <span data-ttu-id="84bad-169">Lásd: hello [pont – hely kapcsolat] [ vnet-point-to-site-connectivity] olvashat.</span><span class="sxs-lookup"><span data-stu-id="84bad-169">See hello [Point-To-Site Connectivity][vnet-point-to-site-connectivity] page for more information.</span></span>
7. <span data-ttu-id="84bad-170">Hello virtuális hálózati cím szóközöket szakaszban kattintson **átjáró alhálózatának hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="84bad-170">In hello virtual network address spaces section, click **add gateway subnet**.</span></span>
8. <span data-ttu-id="84bad-171">Kattintson a **mentése** a hello hello lap alján.</span><span class="sxs-lookup"><span data-stu-id="84bad-171">Click **SAVE** on hello bottom of hello page.</span></span>
9. <span data-ttu-id="84bad-172">Kattintson a **Igen** tooconfirm hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="84bad-172">Click **YES** tooconfirm hello change.</span></span> <span data-ttu-id="84bad-173">Várjon, amíg hello rendszer befejezte, így módosíthatja, mielőtt továbblépne a következő eljárással toohello hello.</span><span class="sxs-lookup"><span data-stu-id="84bad-173">Wait until hello system has finished making hello change before you proceed toohello next procedure.</span></span>

<span data-ttu-id="84bad-174">**a dinamikus útválasztó átjáró toocreate**</span><span class="sxs-lookup"><span data-stu-id="84bad-174">**toocreate a dynamic routing gateway**</span></span>

1. <span data-ttu-id="84bad-175">A klasszikus Azure portál hello, kattintson az **IRÁNYÍTÓPULT** hello lap hello elejéről.</span><span class="sxs-lookup"><span data-stu-id="84bad-175">From hello Azure Classic Portal, click **DASHBOARD** from hello top of hello page.</span></span>
2. <span data-ttu-id="84bad-176">Kattintson a **ÁTJÁRÓ létrehozása** hello lap hello lista aljáról.</span><span class="sxs-lookup"><span data-stu-id="84bad-176">Click **CREATE GATEWAY** from hello bottom of hello page.</span></span>
3. <span data-ttu-id="84bad-177">Kattintson a **Igen** tooconfirm.</span><span class="sxs-lookup"><span data-stu-id="84bad-177">Click **YES** tooconfirm.</span></span> <span data-ttu-id="84bad-178">Várjon, amíg hello átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84bad-178">Wait until hello gateway is created.</span></span>
4. <span data-ttu-id="84bad-179">Kattintson a **IRÁNYÍTÓPULT** hello elejéről.</span><span class="sxs-lookup"><span data-stu-id="84bad-179">Click **DASHBOARD** from hello top.</span></span>  <span data-ttu-id="84bad-180">Hello virtuális hálózati visual ábrázoló diagram jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="84bad-180">You will see a visual diagram of hello virtual network:</span></span>

    ![Azure-beli virtuális hálózat pont-pont virtuális diagramja][img-vnet-diagram]

    <span data-ttu-id="84bad-182">hello ábrán 0 ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="84bad-182">hello diagram shows 0 client connections.</span></span> <span data-ttu-id="84bad-183">Miután elvégezte a kapcsolat toohello virtuális hálózat, hello lesz frissített tooone.</span><span class="sxs-lookup"><span data-stu-id="84bad-183">After you make a connection toohello virtual network, hello number will be updated tooone.</span></span>

#### <a name="create-your-certificates"></a><span data-ttu-id="84bad-184">A tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="84bad-184">Create your certificates</span></span>
<span data-ttu-id="84bad-185">Egy X.509 tanúsítvány használatával van egyirányú toocreate hello tanúsítvány létrehozási eszköz (makecert.exe) a [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="84bad-185">One way toocreate an X.509 certificate is by using hello Certificate Creation Tool (makecert.exe) that comes with [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span></span>

<span data-ttu-id="84bad-186">**toocreate önaláírt legfelső szintű tanúsítvány**</span><span class="sxs-lookup"><span data-stu-id="84bad-186">**toocreate a self-signed root certificate**</span></span>

1. <span data-ttu-id="84bad-187">A munkaállomáson nyisson meg egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="84bad-187">From your workstation, open a command prompt window.</span></span>
2. <span data-ttu-id="84bad-188">Keresse meg a toohello Visual Studio eszközök mappában.</span><span class="sxs-lookup"><span data-stu-id="84bad-188">Navigate toohello Visual Studio tools folder.</span></span>
3. <span data-ttu-id="84bad-189">hello hello az alábbi példában lévő parancs a következő hozzon létre és telepítsen egy legfelső szintű tanúsítványt hello személyes tanúsítványtárolójába a munkaállomáson és a megfelelő .cer fájlt, hogy később fogja feltölteni toohello klasszikus Azure portálon is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="84bad-189">hello following command in hello example below will create and install a root certificate in hello Personal certificate store on your workstation and also create a corresponding .cer file that you’ll later upload toohello Azure Classic Portal.</span></span>

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    <span data-ttu-id="84bad-190">Módosítsa a használni kívánt .cer fájl toobe található, ahol HBaseVnetVPNRootCertificate az hello nevét, amelyet az toouse hello tanúsítvány hello toohello-könyvtárat.</span><span class="sxs-lookup"><span data-stu-id="84bad-190">Change toohello directory that you want hello .cer file toobe located in, where HBaseVnetVPNRootCertificate is hello name that you want toouse for hello certificate.</span></span>

    <span data-ttu-id="84bad-191">Ne zárja be a hello parancssort.</span><span class="sxs-lookup"><span data-stu-id="84bad-191">Don't close hello command prompt.</span></span>  <span data-ttu-id="84bad-192">Szüksége lesz rájuk a következő eljárással hello.</span><span class="sxs-lookup"><span data-stu-id="84bad-192">You will need it in hello next procedure.</span></span>

   > [!NOTE]
   > <span data-ttu-id="84bad-193">Létrehozott egy legfelső szintű tanúsítványt, amelyből ügyféltanúsítványok jön létre, mert tooexport szeretné ezt a tanúsítványt és annak titkos kulcsát, és tooa biztonságos helyre, ahol lehetséges, hogy helyre mentse.</span><span class="sxs-lookup"><span data-stu-id="84bad-193">Because you have created a root certificate from which client certificates will be generated, you may want tooexport this certificate along with its private key and save it tooa safe location where it may be recovered.</span></span>
   >
   >

<span data-ttu-id="84bad-194">**toocreate ügyféltanúsítványt**</span><span class="sxs-lookup"><span data-stu-id="84bad-194">**toocreate a client certificate**</span></span>

* <span data-ttu-id="84bad-195">A hello azonos parancssort (toobe rendelkezzen hello ugyanazon a számítógépen, amelyen létrehozta hello legfelső szintű tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="84bad-195">From hello same command prompt (It has toobe on hello same computer where you created hello root certificate.</span></span> <span data-ttu-id="84bad-196">hello ügyféltanúsítvány generálása származó hello főtanúsítvány), futtatási hello következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="84bad-196">hello client certificate must be generated from hello root certificate), run hello following command:</span></span>

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    <span data-ttu-id="84bad-197">HBaseVnetVPNRootCertificate hello legfelső szintű tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="84bad-197">HBaseVnetVPNRootCertificate is hello root certificate name.</span></span>  <span data-ttu-id="84bad-198">Toomatch hello legfelső szintű tanúsítvány névvel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="84bad-198">It has toomatch hello root certificate name.</span></span>  

    <span data-ttu-id="84bad-199">A számítógép személyes tanúsítványtárolójában hello legfelső szintű tanúsítvány és a hello ügyféltanúsítvány vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="84bad-199">Both hello root certificate and hello client certificate are stored in your Personal certificate store on your computer.</span></span> <span data-ttu-id="84bad-200">Certmgr.msc tooverify használja.</span><span class="sxs-lookup"><span data-stu-id="84bad-200">Use certmgr.msc tooverify.</span></span>

    ![Azure-beli virtuális hálózat pont-pont VPN-tanúsítványának][img-certificate]

    <span data-ttu-id="84bad-202">Ügyféltanúsítvány telepítenie kell minden olyan számítógépre, amelyet az tooconnect toohello virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="84bad-202">A client certificate must be installed on each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="84bad-203">Azt javasoljuk, hogy hozzon létre egyedi ügyfél számítógépenként, amelyet az tooconnect toohello virtuális hálózati tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="84bad-203">We recommend that you create unique client certificates for each computer that you want tooconnect toohello virtual network.</span></span> <span data-ttu-id="84bad-204">tooexport hello ügyféltanúsítványok certmgr.msc használja.</span><span class="sxs-lookup"><span data-stu-id="84bad-204">tooexport hello client certificates, use certmgr.msc.</span></span>

<span data-ttu-id="84bad-205">**tooupload hello legfelső szintű tanúsítvány toohello klasszikus Azure portálon**</span><span class="sxs-lookup"><span data-stu-id="84bad-205">**tooupload hello root certificate toohello Azure Classic Portal**</span></span>

1. <span data-ttu-id="84bad-206">A klasszikus Azure portál hello, kattintson az **hálózati** hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="84bad-206">From hello Azure Classic Portal, click **NETWORK** on hello left.</span></span>
2. <span data-ttu-id="84bad-207">Kattintson a hello virtuális hálózat, amelyen üzembe van helyezve a HBase-fürtöt a.</span><span class="sxs-lookup"><span data-stu-id="84bad-207">Click hello virtual network where your HBase cluster is deployed to.</span></span>
3. <span data-ttu-id="84bad-208">Kattintson a **tanúsítványok** hello elejéről.</span><span class="sxs-lookup"><span data-stu-id="84bad-208">Click **CERTIFICATES** from hello top.</span></span>
4. <span data-ttu-id="84bad-209">Kattintson a **FELTÖLTÉSE** hello az alsó, és adja meg a hello főtanúsítványfájlt hello eljárás utolsó előtt létrehozott.</span><span class="sxs-lookup"><span data-stu-id="84bad-209">Click **UPLOAD** from hello bottom, and specify hello root certificate file you have created in hello procedure before last.</span></span> <span data-ttu-id="84bad-210">Várjon, amíg hello tanúsítvány lett importálva.</span><span class="sxs-lookup"><span data-stu-id="84bad-210">Wait until hello certificate got imported.</span></span>
5. <span data-ttu-id="84bad-211">Kattintson a **IRÁNYÍTÓPULT** hello felül.</span><span class="sxs-lookup"><span data-stu-id="84bad-211">Click **DASHBOARD** on hello top.</span></span>  <span data-ttu-id="84bad-212">hello virtuális diagram hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="84bad-212">hello virtual diagram shows hello status.</span></span>

#### <a name="configure-your-vpn-client"></a><span data-ttu-id="84bad-213">A VPN-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="84bad-213">Configure your VPN client</span></span>
<span data-ttu-id="84bad-214">**toodownload és a telepítés hello ügyfél VPN-csomag**</span><span class="sxs-lookup"><span data-stu-id="84bad-214">**toodownload and install hello client VPN package**</span></span>

1. <span data-ttu-id="84bad-215">Az IRÁNYÍTÓPULT-oldalon hello hello virtuális hálózat hello gyors áttekintő területen jelölje be az **letöltési hello 64 bites ügyfél VPN-csomag** vagy **letöltési hello 32-bit-es VPN ügyfélcsomag** alapján a munkaállomást az operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="84bad-215">From hello DASHBOARD page of hello virtual network, in hello quick glance section, click either **Download hello 64-bit Client VPN Package** or **Download hello 32-bit Client VPN Package** based on your workstation OS version.</span></span>
2. <span data-ttu-id="84bad-216">Kattintson a **futtatása** tooinstall hello csomag.</span><span class="sxs-lookup"><span data-stu-id="84bad-216">Click **Run** tooinstall hello package.</span></span>
3. <span data-ttu-id="84bad-217">A parancssorból hello biztonsági kattintson **további információ**, és kattintson a **mégis futtatni**.</span><span class="sxs-lookup"><span data-stu-id="84bad-217">At hello security prompt, click **More info**, and then click **Run anyway**.</span></span>
4. <span data-ttu-id="84bad-218">Kattintson a **Igen** kétszer.</span><span class="sxs-lookup"><span data-stu-id="84bad-218">Click **Yes** twice.</span></span>

<span data-ttu-id="84bad-219">**tooconnect tooVPN**</span><span class="sxs-lookup"><span data-stu-id="84bad-219">**tooconnect tooVPN**</span></span>

1. <span data-ttu-id="84bad-220">A munkaállomás hello asztalon kattintson a tálcán hello hello hálózatok ikonjára.</span><span class="sxs-lookup"><span data-stu-id="84bad-220">On hello desktop of your workstation, click hello Networks icon on hello Task bar.</span></span> <span data-ttu-id="84bad-221">A virtuális hálózati név látni fog egy VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="84bad-221">You shall see a VPN connection with your virtual network name.</span></span>
2. <span data-ttu-id="84bad-222">Kattintson a hello VPN-kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="84bad-222">Click hello VPN connection name.</span></span>
3. <span data-ttu-id="84bad-223">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84bad-223">Click **Connect**.</span></span>

<span data-ttu-id="84bad-224">**VPN-kapcsolat és a tartomány névfeloldás tootest hello**</span><span class="sxs-lookup"><span data-stu-id="84bad-224">**tootest hello VPN connection and domain name resolution**</span></span>

* <span data-ttu-id="84bad-225">Hello munkaállomásról, nyisson meg egy parancssort, és ping a következő nevekre hello HBase fürt DNS-utótag hello myhbase.b7.internal.cloudapp.net:</span><span class="sxs-lookup"><span data-stu-id="84bad-225">From hello workstation, open a command prompt and ping one of hello following names given hello HBase cluster's DNS suffix is myhbase.b7.internal.cloudapp.net:</span></span>

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a><span data-ttu-id="84bad-226">Telepítse és konfigurálja az SQuirreL a munkaállomáson</span><span class="sxs-lookup"><span data-stu-id="84bad-226">Install and configure SQuirreL on your workstation</span></span>
<span data-ttu-id="84bad-227">**tooinstall SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="84bad-227">**tooinstall SQuirreL**</span></span>

1. <span data-ttu-id="84bad-228">Töltse le a hello SQuirreL SQL ügyfél jar-fájlra a [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span><span class="sxs-lookup"><span data-stu-id="84bad-228">Download hello SQuirreL SQL client jar file from [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span></span>
2. <span data-ttu-id="84bad-229">Megnyitás/futtatás hello jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="84bad-229">Open/run hello jar file.</span></span> <span data-ttu-id="84bad-230">Hello igényel [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span><span class="sxs-lookup"><span data-stu-id="84bad-230">It requires hello [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span></span>
3. <span data-ttu-id="84bad-231">Kattintson a **következő** kétszer.</span><span class="sxs-lookup"><span data-stu-id="84bad-231">Click **Next** twice.</span></span>
4. <span data-ttu-id="84bad-232">Adjon meg egy elérési utat, melyekben hello írási engedélye, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="84bad-232">Specify a path where you have hello write permission, and then click **Next**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="84bad-233">hello alapértelmezett telepítési mappa hello C:\Program Files\squirrel-sql-3.6 mappában van.</span><span class="sxs-lookup"><span data-stu-id="84bad-233">hello default installation folder is in hello C:\Program Files\squirrel-sql-3.6 folder.</span></span>  <span data-ttu-id="84bad-234">Az order toowrite toothis elérési út hello telepítő joggal kell hello rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="84bad-234">In order toowrite toothis path, hello installer must be granted hello administrator privilege.</span></span> <span data-ttu-id="84bad-235">Nyisson meg egy parancssort rendszergazdaként, keresse meg a tooJava a bin mappát, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="84bad-235">You can open a command prompt as administrator, navigate tooJava's bin folder, and then run:</span></span>
  >
  >     <span data-ttu-id="84bad-236">Java.exe-jar [hello elérési útja hello SQuirreL jar-fájlt]</span><span class="sxs-lookup"><span data-stu-id="84bad-236">java.exe -jar [hello path of hello SQuirreL jar file]</span></span>
5. <span data-ttu-id="84bad-237">Kattintson a **OK** tooconfirm hello cél könyvtár létrehozása.</span><span class="sxs-lookup"><span data-stu-id="84bad-237">Click **OK** tooconfirm creating hello target directory.</span></span>
6. <span data-ttu-id="84bad-238">hello alapértéke tooinstall hello kiinduló és a szabványos csomagok.</span><span class="sxs-lookup"><span data-stu-id="84bad-238">hello default setting is tooinstall hello Base and Standard packages.</span></span>  <span data-ttu-id="84bad-239">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="84bad-239">Click **Next**.</span></span>
7. <span data-ttu-id="84bad-240">Kattintson a **következő** kétszer, majd **végzett**.</span><span class="sxs-lookup"><span data-stu-id="84bad-240">Click **Next** twice, and then click **Done**.</span></span>

<span data-ttu-id="84bad-241">**tooinstall hello Phoenix illesztőprogram**</span><span class="sxs-lookup"><span data-stu-id="84bad-241">**tooinstall hello Phoenix driver**</span></span>

<span data-ttu-id="84bad-242">hello phoenix illesztőprogram jar-fájlra található hello HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="84bad-242">hello phoenix driver jar file is located on hello HBase cluster.</span></span> <span data-ttu-id="84bad-243">elérési út hello hasonló toohello következő hello verziók alapján:</span><span class="sxs-lookup"><span data-stu-id="84bad-243">hello path is similar toohello following based on hello versions:</span></span>

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
<span data-ttu-id="84bad-244">Toocopy kell azt tooyour munkaállomás alatt hello [SQuirreL telepítési mappa] / lib elérési útja.</span><span class="sxs-lookup"><span data-stu-id="84bad-244">You need toocopy it tooyour workstation under hello [SQuirreL installation folder]/lib path.</span></span>  <span data-ttu-id="84bad-245">hello legegyszerűbb módja a tooRDP hello fürt, majd a fájlt másolja és illessze be (CTRL + C és CTRL + V) toocopy használja azt tooyour munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="84bad-245">hello easiest way is tooRDP into hello cluster, and then use file copy/paste (CTRL+C and CTRL+V) toocopy it tooyour workstation.</span></span>

<span data-ttu-id="84bad-246">**a Phoenix illesztőprogram tooSQuirreL tooadd**</span><span class="sxs-lookup"><span data-stu-id="84bad-246">**tooadd a Phoenix driver tooSQuirreL**</span></span>

1. <span data-ttu-id="84bad-247">Nyissa meg a SQuirreL SQL ügyfél a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="84bad-247">Open SQuirreL SQL Client from your workstation.</span></span>
2. <span data-ttu-id="84bad-248">Kattintson a hello **illesztőprogram** hello bal oldali lapon.</span><span class="sxs-lookup"><span data-stu-id="84bad-248">Click hello **Driver** tab on hello left.</span></span>
3. <span data-ttu-id="84bad-249">A hello **illesztőprogramok** menüben kattintson a **új illesztőprogram**.</span><span class="sxs-lookup"><span data-stu-id="84bad-249">From hello **Drivers** menu, click **New Driver**.</span></span>
4. <span data-ttu-id="84bad-250">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="84bad-250">Enter hello following information:</span></span>

   * <span data-ttu-id="84bad-251">**Név**: Phoenix</span><span class="sxs-lookup"><span data-stu-id="84bad-251">**Name**: Phoenix</span></span>
   * <span data-ttu-id="84bad-252">**Példa URL-cím**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="84bad-252">**Example URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span></span>
   * <span data-ttu-id="84bad-253">**Osztálynév**: org.apache.phoenix.jdbc.PhoenixDriver</span><span class="sxs-lookup"><span data-stu-id="84bad-253">**Class Name**: org.apache.phoenix.jdbc.PhoenixDriver</span></span>

     > [!WARNING]
     > <span data-ttu-id="84bad-254">Felhasználói hello példa URL-címet az összes kisbetű.</span><span class="sxs-lookup"><span data-stu-id="84bad-254">User all lower case in hello Example URL.</span></span> <span data-ttu-id="84bad-255">Használhatja azokat teljes zookeeper kvórum abban az esetben, ha ezek egyikét nem működik.</span><span class="sxs-lookup"><span data-stu-id="84bad-255">You can use they full zookeeper quorum in case one of them is down.</span></span>  <span data-ttu-id="84bad-256">hello állomásnevek zookeeper0 zookeeper1 és zookeeper2.</span><span class="sxs-lookup"><span data-stu-id="84bad-256">hello hostnames are zookeeper0, zookeeper1, and zookeeper2.</span></span>
     >
     >

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-driver]
5. <span data-ttu-id="84bad-258">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="84bad-258">Click **OK**.</span></span>

<span data-ttu-id="84bad-259">**toocreate alias toohello HBase-fürtöt**</span><span class="sxs-lookup"><span data-stu-id="84bad-259">**toocreate an alias toohello HBase cluster**</span></span>

1. <span data-ttu-id="84bad-260">A SQuirreL, kattintson a hello **aliasok** hello bal oldali lapon.</span><span class="sxs-lookup"><span data-stu-id="84bad-260">From SQuirreL, Click hello **Aliases** tab on hello left.</span></span>
2. <span data-ttu-id="84bad-261">A hello **aliasok** menüben kattintson a **új Alias**.</span><span class="sxs-lookup"><span data-stu-id="84bad-261">From hello **Aliases** menu, click **New Alias**.</span></span>
3. <span data-ttu-id="84bad-262">Adja meg a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="84bad-262">Enter hello following information:</span></span>

   * <span data-ttu-id="84bad-263">**Név**: hello HBase-fürtöt, vagy inkább nevek hello nevét.</span><span class="sxs-lookup"><span data-stu-id="84bad-263">**Name**: hello name of hello HBase cluster or any name you prefer.</span></span>
   * <span data-ttu-id="84bad-264">**Illesztőprogram**: Phoenix.</span><span class="sxs-lookup"><span data-stu-id="84bad-264">**Driver**: Phoenix.</span></span>  <span data-ttu-id="84bad-265">Ennek egyeznie kell a hello utolsó eljárásban létrehozott hello illesztőprogram neve.</span><span class="sxs-lookup"><span data-stu-id="84bad-265">This must match hello driver name you created in hello last procedure.</span></span>
   * <span data-ttu-id="84bad-266">**URL-cím**: hello URL-címet a rendszer átmásolja az illesztőprogram-konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="84bad-266">**URL**: hello URL is copied from your driver configuration.</span></span> <span data-ttu-id="84bad-267">Győződjön meg arról, hogy toouser összes kisbetű.</span><span class="sxs-lookup"><span data-stu-id="84bad-267">Make sure toouser all lower case.</span></span>
   * <span data-ttu-id="84bad-268">**Felhasználónév**: szöveg lehet.</span><span class="sxs-lookup"><span data-stu-id="84bad-268">**User name**: It can be any text.</span></span>  <span data-ttu-id="84bad-269">Itt adhat meg VPN-kapcsolatot, mert hello felhasználónév nem minden használja.</span><span class="sxs-lookup"><span data-stu-id="84bad-269">Because you use VPN connectivity here, hello user name is not used at all.</span></span>
   * <span data-ttu-id="84bad-270">**Jelszó**: szöveg lehet.</span><span class="sxs-lookup"><span data-stu-id="84bad-270">**Password**: It can be any text.</span></span>

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-alias]
4. <span data-ttu-id="84bad-272">Kattintson a **teszt**.</span><span class="sxs-lookup"><span data-stu-id="84bad-272">Click **Test**.</span></span>
5. <span data-ttu-id="84bad-273">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="84bad-273">Click **Connect**.</span></span> <span data-ttu-id="84bad-274">Ha hello kapcsolat, SQuirreL néz ki:</span><span class="sxs-lookup"><span data-stu-id="84bad-274">When it makes hello connection, SQuirreL looks like:</span></span>

    ![A HBase Phoenix SQuirreL][img-squirrel]

<span data-ttu-id="84bad-276">**egy teszt toorun**</span><span class="sxs-lookup"><span data-stu-id="84bad-276">**toorun a test**</span></span>

1. <span data-ttu-id="84bad-277">Kattintson a hello **SQL** jobb következő toohello lapon **objektumok** lapon.</span><span class="sxs-lookup"><span data-stu-id="84bad-277">Click hello **SQL** tab right next toohello **Objects** tab.</span></span>
2. <span data-ttu-id="84bad-278">Másolja és illessze be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="84bad-278">Copy and paste hello following code:</span></span>

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. <span data-ttu-id="84bad-279">Hello Futtatás gombra.</span><span class="sxs-lookup"><span data-stu-id="84bad-279">Click hello run button.</span></span>

    ![A HBase Phoenix SQuirreL][img-squirrel-sql]
4. <span data-ttu-id="84bad-281">Váltson vissza toohello **objektumok** fülre.</span><span class="sxs-lookup"><span data-stu-id="84bad-281">Switch back toohello **Objects** tab.</span></span>
5. <span data-ttu-id="84bad-282">Bontsa ki a hello aliasnév, és végül **tábla**.</span><span class="sxs-lookup"><span data-stu-id="84bad-282">Expand hello alias name, and then expand **TABLE**.</span></span>  <span data-ttu-id="84bad-283">Ekkor megjelenik a hello tartozó új tábla.</span><span class="sxs-lookup"><span data-stu-id="84bad-283">You shall see hello new table listed under.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84bad-284">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="84bad-284">Next steps</span></span>
<span data-ttu-id="84bad-285">Ebben a cikkben megtanulta, hogyan toouse Apache Phoenix a hdinsight eszközben.</span><span class="sxs-lookup"><span data-stu-id="84bad-285">In this article, you have learned how toouse Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="84bad-286">toolearn több, lásd:</span><span class="sxs-lookup"><span data-stu-id="84bad-286">toolearn more, see</span></span>

* <span data-ttu-id="84bad-287">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="84bad-287">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="84bad-288">[Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, a HBase-fürtökkel telepíthetők telepített toohello azonos virtuális hálózati alkalmazásaihoz, így alkalmazások kommunikálhatnak A HBase közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="84bad-288">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed toohello same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="84bad-289">[A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): megtudhatja, hogyan tooconfigure HBase-replikálás két Azure üzemeltetésében.</span><span class="sxs-lookup"><span data-stu-id="84bad-289">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how tooconfigure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="84bad-290">[Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: megtudhatja, hogyan valós idejű toodo [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="84bad-290">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how toodo real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
