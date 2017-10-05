---
title: "Használja az Apache Phoenix és az SQuirreL with Azure HDInsight Windows-alapú |} Microsoft Docs"
description: "Útmutató Apache Phoenix használata a Hdinsightban, és telepítésének és konfigurálásának SQuirreL való kapcsolódáshoz a hdinsight HBase-fürtöt a munkaállomáson."
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
ms.openlocfilehash: 024b70df99fdefa1598225ebb1fbfee85ea375d0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-apache-phoenix-and-squirrel-with-windows-based-hbase-clusters-in-hdinsight"></a><span data-ttu-id="8da4a-103">Apache Phoenix és az SQuirreL használata a Hdinsightban Windows-alapú HBase-fürtökkel</span><span class="sxs-lookup"><span data-stu-id="8da4a-103">Use Apache Phoenix and SQuirreL with Windows-based HBase clusters in HDInsight</span></span>
<span data-ttu-id="8da4a-104">Ismerje meg, hogyan használható [Apache Phoenix](http://phoenix.apache.org/) , és hogyan telepítheti és konfigurálhatja a munkaállomásra való kapcsolódáshoz a hdinsight HBase-fürtöt SQuirreL.</span><span class="sxs-lookup"><span data-stu-id="8da4a-104">Learn how to use [Apache Phoenix](http://phoenix.apache.org/) in HDInsight, and how to install and configure SQuirreL on your workstation to connect to an HBase cluster in HDInsight.</span></span> <span data-ttu-id="8da4a-105">Phoenix kapcsolatos további információkért lásd: [Phoenix kevesebb mint 15 perc alatt](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span><span class="sxs-lookup"><span data-stu-id="8da4a-105">For more information about Phoenix, see [Phoenix in 15 minutes or less](http://phoenix.apache.org/Phoenix-in-15-minutes-or-less.html).</span></span> <span data-ttu-id="8da4a-106">A Phoenix nyelvtan, lásd: [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="8da4a-106">For the Phoenix grammar, see [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

> [!NOTE]
> <span data-ttu-id="8da4a-107">A Phoenix fájlverzió-információkat a Hdinsightban, lásd: [What's new in HDInsight által biztosított Hadoop-fürt verziók?](hdinsight-component-versioning.md).</span><span class="sxs-lookup"><span data-stu-id="8da4a-107">For the Phoenix version information in HDInsight, see [What's new in the Hadoop cluster versions provided by HDInsight?](hdinsight-component-versioning.md).</span></span>
>

> [!IMPORTANT]
> <span data-ttu-id="8da4a-108">Az ebben a dokumentumban csak a lépések Windows-alapú HDInsight-fürtök.</span><span class="sxs-lookup"><span data-stu-id="8da4a-108">The steps in this document only work for Windows-based HDInsight clusters.</span></span> <span data-ttu-id="8da4a-109">HDInsight csak érhető el a Windows korábbi, mint a HDInsight 3.4-es verziójához.</span><span class="sxs-lookup"><span data-stu-id="8da4a-109">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="8da4a-110">A Linux az egyetlen operációs rendszer, amely a HDInsight 3.4-es vagy újabb verziói esetében használható.</span><span class="sxs-lookup"><span data-stu-id="8da4a-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="8da4a-111">További tudnivalókért lásd: [A HDInsight elavulása Windows rendszeren](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="8da4a-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="8da4a-112">Phoenix a Linux-alapú HDInsight használatáról további információért lásd: [használata Apache Phoenix a Linux-alapú HBase a HDInsight clusters](hdinsight-hbase-phoenix-squirrel-linux.md).</span><span class="sxs-lookup"><span data-stu-id="8da4a-112">For information on using Phoenix on Linux-based HDInsight, see [Use Apache Phoenix with Linux-based HBase clusters in HDInsight](hdinsight-hbase-phoenix-squirrel-linux.md).</span></span>
>



## <a name="use-sqlline"></a><span data-ttu-id="8da4a-113">SQLLine használata</span><span class="sxs-lookup"><span data-stu-id="8da4a-113">Use SQLLine</span></span>
<span data-ttu-id="8da4a-114">[SQLLine](http://sqlline.sourceforge.net/) egy parancssori segédprogram SQL végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="8da4a-114">[SQLLine](http://sqlline.sourceforge.net/) is a command line utility to execute SQL.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8da4a-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8da4a-115">Prerequisites</span></span>
<span data-ttu-id="8da4a-116">Mielőtt használhatná SQLLine, az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="8da4a-116">Before you can use SQLLine, you must have the following:</span></span>

* <span data-ttu-id="8da4a-117">**A hdinsight HBase-fürtöt**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-117">**A HBase cluster in HDInsight**.</span></span> <span data-ttu-id="8da4a-118">HBase kiépítési információk fürt esetén lásd: [Ismerkedés az Apache HBase hdinsightban][hdinsight-hbase-get-started].</span><span class="sxs-lookup"><span data-stu-id="8da4a-118">For information on provision HBase cluster, see [Get started with Apache HBase in HDInsight][hdinsight-hbase-get-started].</span></span>
* <span data-ttu-id="8da4a-119">**A HBase-fürtöt, a távoli asztal protokollal csatlakozni**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-119">**Connect to the HBase cluster via the remote desktop protocol**.</span></span> <span data-ttu-id="8da4a-120">Útmutatásért lásd: [kezelése Hadoop-fürtök a HDInsight a klasszikus Azure portál használatával][hdinsight-manage-portal].</span><span class="sxs-lookup"><span data-stu-id="8da4a-120">For instructions, see [Manage Hadoop clusters in HDInsight by using the Azure Classic Portal][hdinsight-manage-portal].</span></span>

<span data-ttu-id="8da4a-121">**Ha szeretné tudni, a gazdagép neve**</span><span class="sxs-lookup"><span data-stu-id="8da4a-121">**To find out the host name**</span></span>

1. <span data-ttu-id="8da4a-122">Nyissa meg **Hadoop parancssori** az asztalról.</span><span class="sxs-lookup"><span data-stu-id="8da4a-122">Open **Hadoop Command Line** from the desktop.</span></span>
2. <span data-ttu-id="8da4a-123">Futtassa a következő parancs használatával beszerezheti a DNS-utótag:</span><span class="sxs-lookup"><span data-stu-id="8da4a-123">Run the following command to get the DNS suffix:</span></span>

        ipconfig

    <span data-ttu-id="8da4a-124">Írja le **kapcsolatspecifikus DNS-utótag**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-124">Write down **Connection-specific DNS Suffix**.</span></span> <span data-ttu-id="8da4a-125">Például *myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="8da4a-125">For example, *myhbasecluster.f5.internal.cloudapp.net*.</span></span> <span data-ttu-id="8da4a-126">Való csatlakozáskor HBase-fürtöt, szüksége lesz a teljes tartománynév használatával Zookeepers egyik való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="8da4a-126">When you connect to an HBase cluster, you will need to connect to one of the Zookeepers using FQDN.</span></span> <span data-ttu-id="8da4a-127">Minden egyes HDInsight-fürt 3 Zookeepers rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="8da4a-127">Each HDInsight cluster has 3 Zookeepers.</span></span> <span data-ttu-id="8da4a-128">Ezek *zookeeper0*, *zookeeper1*, és *zookeeper2*.</span><span class="sxs-lookup"><span data-stu-id="8da4a-128">They are *zookeeper0*, *zookeeper1*, and *zookeeper2*.</span></span> <span data-ttu-id="8da4a-129">A teljes Tartománynevet fogja hasonlót *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span><span class="sxs-lookup"><span data-stu-id="8da4a-129">The FQDN will be something like *zookeeper2.myhbasecluster.f5.internal.cloudapp.net*.</span></span>

<span data-ttu-id="8da4a-130">**SQLLine használata**</span><span class="sxs-lookup"><span data-stu-id="8da4a-130">**To use SQLLine**</span></span>

1. <span data-ttu-id="8da4a-131">Nyissa meg **Hadoop parancssori** az asztalról.</span><span class="sxs-lookup"><span data-stu-id="8da4a-131">Open **Hadoop Command Line** from the desktop.</span></span>
2. <span data-ttu-id="8da4a-132">Nyissa meg a SQLLine a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="8da4a-132">Run the following commands to open SQLLine:</span></span>

        cd %phoenix_home%\bin
        sqlline.py [The FQDN of one of the Zookeepers]

    ![HDInsight hbase phoenix sqlline][hdinsight-hbase-phoenix-sqlline]

    <span data-ttu-id="8da4a-134">A mintában használt parancsok:</span><span class="sxs-lookup"><span data-stu-id="8da4a-134">The commands used in the sample:</span></span>

        CREATE TABLE Company (COMPANY_ID INTEGER PRIMARY KEY, NAME VARCHAR(225));

        !tables

        UPSERT INTO Company VALUES(1, 'Microsoft');

        SELECT * FROM Company;

<span data-ttu-id="8da4a-135">További információkért lásd: [SQLLine manuális](http://sqlline.sourceforge.net/#manual) és [Phoenix nyelvtan](http://phoenix.apache.org/language/index.html).</span><span class="sxs-lookup"><span data-stu-id="8da4a-135">For more information, see [SQLLine manual](http://sqlline.sourceforge.net/#manual) and [Phoenix Grammar](http://phoenix.apache.org/language/index.html).</span></span>

## <a name="use-squirrel"></a><span data-ttu-id="8da4a-136">SQuirreL használata</span><span class="sxs-lookup"><span data-stu-id="8da4a-136">Use SQuirreL</span></span>
<span data-ttu-id="8da4a-137">[SQL-ügyfél sQuirreL](http://squirrel-sql.sourceforge.net/) grafikus Java-program hozhat létre, amely lehetővé teszi olyan JDBC-kompatibilis adatbázisokba szerkezete megtekintése, keresse meg a táblázatok adatait, adja ki az SQL-parancsok stb. A HDInsight az Apache Phoenix csatlakozni használható.</span><span class="sxs-lookup"><span data-stu-id="8da4a-137">[SQuirreL SQL Client](http://squirrel-sql.sourceforge.net/) is a graphical Java program that will allow you to view the structure of a JDBC compliant database, browse the data in tables, issue SQL commands etc. It can be used to connect to Apache Phoenix on HDInsight.</span></span>

<span data-ttu-id="8da4a-138">Ez a szakasz bemutatja, hogyan telepítéséhez és konfigurálásához SQuirreL VPN-en keresztül a HDInsight HBase-fürtöt való csatlakozáshoz a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8da4a-138">This section shows you how to install and configure SQuirreL on your workstation to connect to an HBase cluster in HDInsight via VPN.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8da4a-139">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8da4a-139">Prerequisites</span></span>
<span data-ttu-id="8da4a-140">Eljárások végrehajtása előtt az alábbiakkal kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="8da4a-140">Before following the procedures, you must have the following:</span></span>

* <span data-ttu-id="8da4a-141">A DNS virtuális gépekkel egy Azure virtuális hálózatra telepített HBase-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="8da4a-141">An HBase cluster deployed to an Azure virtual network with a DNS virtual machine.</span></span>  <span data-ttu-id="8da4a-142">Útmutatásért lásd: [hozzon létre HBase-fürtök Azure virtuális hálózat][hdinsight-hbase-provision-vnet].</span><span class="sxs-lookup"><span data-stu-id="8da4a-142">For instructions, see [Create HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet].</span></span>

* <span data-ttu-id="8da4a-143">A HBase fürt fürt kapcsolatspecifikus DNS-utótag első.</span><span class="sxs-lookup"><span data-stu-id="8da4a-143">Get the HBase cluster cluster Connection-specific DNS suffix.</span></span> <span data-ttu-id="8da4a-144">Az RDP a fürthöz, és futtassa az IPConfig.</span><span class="sxs-lookup"><span data-stu-id="8da4a-144">To get it, RDP into the cluster, and then run IPConfig.</span></span>  <span data-ttu-id="8da4a-145">A DNS-utótag hasonlít:</span><span class="sxs-lookup"><span data-stu-id="8da4a-145">The DNS suffix is similar to:</span></span>

        myhbase.b7.internal.cloudapp.net
* <span data-ttu-id="8da4a-146">Töltse le és telepítse [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="8da4a-146">Download and install [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx) on your workstation.</span></span> <span data-ttu-id="8da4a-147">Szüksége lesz a csomag létrehozni a tanúsítványt a makecert.</span><span class="sxs-lookup"><span data-stu-id="8da4a-147">You will need makecert from the package to create your certificate.</span></span>  
* <span data-ttu-id="8da4a-148">Töltse le és telepítse [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="8da4a-148">Download and install [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html) on your workstation.</span></span>  <span data-ttu-id="8da4a-149">SQuirreL SQL ügyfélverzió 3.0-s és újabb rendszer JRE 1.6 vagy újabb verziója szükséges.</span><span class="sxs-lookup"><span data-stu-id="8da4a-149">SQuirreL SQL client version 3.0 and higher requires JRE version 1.6 or higher.</span></span>  

### <a name="configure-a-point-to-site-vpn-connection-to-the-azure-virtual-network"></a><span data-ttu-id="8da4a-150">Pont – hely típusú VPN-kapcsolat az Azure virtuális hálózat konfigurálva</span><span class="sxs-lookup"><span data-stu-id="8da4a-150">Configure a Point-to-Site VPN connection to the Azure virtual network</span></span>
<span data-ttu-id="8da4a-151">Nincsenek érintett pont-pont VPN-kapcsolat konfigurálása 3 lépést:</span><span class="sxs-lookup"><span data-stu-id="8da4a-151">There are 3 steps involved configuring a point-to-site VPN connection:</span></span>

1. [<span data-ttu-id="8da4a-152">Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8da4a-152">Configure a virtual network and a dynamic routing gateway</span></span>](#Configure-a-virtual-network-and-a-dynamic-routing-gateway)
2. [<span data-ttu-id="8da4a-153">A tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8da4a-153">Create your certificates</span></span>](#Create-your-certificates)
3. [<span data-ttu-id="8da4a-154">A VPN-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8da4a-154">Configure your VPN client</span></span>](#Configure-your-VPN-client)

<span data-ttu-id="8da4a-155">Lásd: [egy Azure virtuális hálózatra egy pont – hely típusú VPN-kapcsolat beállítása](../vpn-gateway/vpn-gateway-point-to-site-create.md) további információt.</span><span class="sxs-lookup"><span data-stu-id="8da4a-155">See [Configure a Point-to-Site VPN connection to an Azure Virtual Network](../vpn-gateway/vpn-gateway-point-to-site-create.md) for more information.</span></span>

#### <a name="configure-a-virtual-network-and-a-dynamic-routing-gateway"></a><span data-ttu-id="8da4a-156">Virtuális hálózat és a dinamikus útválasztó átjáró konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8da4a-156">Configure a virtual network and a dynamic routing gateway</span></span>
<span data-ttu-id="8da4a-157">Biztosítja a HBase-fürtöt egy Azure virtuális hálózatban ellátta (lásd az ebben a szakaszban).</span><span class="sxs-lookup"><span data-stu-id="8da4a-157">Assure you have provisioned an HBase cluster in an Azure virtual network (see the prerequisites for this section).</span></span> <span data-ttu-id="8da4a-158">A következő lépéssel konfigurálása egy pont – hely kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="8da4a-158">The next step is to configure a point-to-site connection.</span></span>

<span data-ttu-id="8da4a-159">**A pont – hely kapcsolat konfigurálása**</span><span class="sxs-lookup"><span data-stu-id="8da4a-159">**To configure the point-to-site connectivity**</span></span>

1. <span data-ttu-id="8da4a-160">Jelentkezzen be a [a klasszikus Azure portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="8da4a-160">Sign in to the [Azure Classic Portal][azure-portal].</span></span>
2. <span data-ttu-id="8da4a-161">Kattintson a bal oldali **hálózatok**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-161">On the left, click **NETWORKS**.</span></span>
3. <span data-ttu-id="8da4a-162">Kattintson a létrehozott virtuális hálózat (lásd: [rendelkezés HBase clusters Azure virtuális hálózat][hdinsight-hbase-provision-vnet]).</span><span class="sxs-lookup"><span data-stu-id="8da4a-162">Click the virtual network you have created (see [Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]).</span></span>
4. <span data-ttu-id="8da4a-163">Kattintson a **KONFIGURÁLÁSA** a lista elejéről.</span><span class="sxs-lookup"><span data-stu-id="8da4a-163">Click **CONFIGURE** from the top.</span></span>
5. <span data-ttu-id="8da4a-164">Az a **pont – hely kapcsolat** szakaszban jelölje be **pont – hely kapcsolat konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-164">In the **point-to-site connectivity** section, select **Configure point-to-site connectivity**.</span></span>
6. <span data-ttu-id="8da4a-165">Konfigurálása **KEZDŐ IP-** és **CIDR** adhatja meg az IP-címtartományt, amelyből a VPN-ügyfelek a csatlakozáskor IP-címet fog kapni.</span><span class="sxs-lookup"><span data-stu-id="8da4a-165">Configure **STARTING IP** and **CIDR** to specify the IP address range from which your VPN clients will receive an IP address when connected.</span></span> <span data-ttu-id="8da4a-166">A tartomány nem lehet átfedésben a helyszíni hálózat és az Azure virtuális hálózat való kapcsolódás található tartományt.</span><span class="sxs-lookup"><span data-stu-id="8da4a-166">The range cannot overlap with any of the ranges located on your on-premises network and the Azure virtual network you will be connecting to.</span></span> <span data-ttu-id="8da4a-167">Például.</span><span class="sxs-lookup"><span data-stu-id="8da4a-167">For example.</span></span> <span data-ttu-id="8da4a-168">Ha a virtuális hálózat 10.0.0.0/20 választotta, az ügyfél-címterület 10.1.0.0/24 választhatja meg.</span><span class="sxs-lookup"><span data-stu-id="8da4a-168">if you selected 10.0.0.0/20 for the virtual network, you can select 10.1.0.0/24 for the client address space.</span></span> <span data-ttu-id="8da4a-169">Tekintse meg a [pont – hely kapcsolat] [ vnet-point-to-site-connectivity] olvashat.</span><span class="sxs-lookup"><span data-stu-id="8da4a-169">See the [Point-To-Site Connectivity][vnet-point-to-site-connectivity] page for more information.</span></span>
7. <span data-ttu-id="8da4a-170">Kattintson a virtuális hálózati cím szóközöket csoport **átjáró alhálózatának hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-170">In the virtual network address spaces section, click **add gateway subnet**.</span></span>
8. <span data-ttu-id="8da4a-171">Kattintson a **mentése** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="8da4a-171">Click **SAVE** on the bottom of the page.</span></span>
9. <span data-ttu-id="8da4a-172">Kattintson a **Igen** a módosítás megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8da4a-172">Click **YES** to confirm the change.</span></span> <span data-ttu-id="8da4a-173">Várjon, amíg a rendszer a módosítást, mielőtt továbblép a következő eljárással befejeződött.</span><span class="sxs-lookup"><span data-stu-id="8da4a-173">Wait until the system has finished making the change before you proceed to the next procedure.</span></span>

<span data-ttu-id="8da4a-174">**Dinamikus útválasztó átjáró létrehozásához**</span><span class="sxs-lookup"><span data-stu-id="8da4a-174">**To create a dynamic routing gateway**</span></span>

1. <span data-ttu-id="8da4a-175">A klasszikus Azure portál, kattintson az **IRÁNYÍTÓPULT** az oldal tetején.</span><span class="sxs-lookup"><span data-stu-id="8da4a-175">From the Azure Classic Portal, click **DASHBOARD** from the top of the page.</span></span>
2. <span data-ttu-id="8da4a-176">Kattintson a **ÁTJÁRÓ létrehozása** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="8da4a-176">Click **CREATE GATEWAY** from the bottom of the page.</span></span>
3. <span data-ttu-id="8da4a-177">Kattintson a **Igen** megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8da4a-177">Click **YES** to confirm.</span></span> <span data-ttu-id="8da4a-178">Várjon, amíg az átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8da4a-178">Wait until the gateway is created.</span></span>
4. <span data-ttu-id="8da4a-179">Kattintson a **IRÁNYÍTÓPULT** a lista elejéről.</span><span class="sxs-lookup"><span data-stu-id="8da4a-179">Click **DASHBOARD** from the top.</span></span>  <span data-ttu-id="8da4a-180">A virtuális hálózati visual ábrázoló diagram jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8da4a-180">You will see a visual diagram of the virtual network:</span></span>

    ![Azure-beli virtuális hálózat pont-pont virtuális diagramja][img-vnet-diagram]

    <span data-ttu-id="8da4a-182">Az ábrán látható 0 ügyfélkapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="8da4a-182">The diagram shows 0 client connections.</span></span> <span data-ttu-id="8da4a-183">Után a kapcsolat a virtuális hálózathoz, az a szám egy fog frissülni.</span><span class="sxs-lookup"><span data-stu-id="8da4a-183">After you make a connection to the virtual network, the number will be updated to one.</span></span>

#### <a name="create-your-certificates"></a><span data-ttu-id="8da4a-184">A tanúsítványok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8da4a-184">Create your certificates</span></span>
<span data-ttu-id="8da4a-185">Egy X.509-tanúsítvány létrehozása módja a tanúsítvány-létrehozási eszközzel (makecert.exe), a [Microsoft Visual Studio Express a Windows asztal](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="8da4a-185">One way to create an X.509 certificate is by using the Certificate Creation Tool (makecert.exe) that comes with [Microsoft Visual Studio Express for Windows Desktop](https://www.visualstudio.com/products/visual-studio-express-vs.aspx).</span></span>

<span data-ttu-id="8da4a-186">**Gyökér önaláírt tanúsítvány létrehozása**</span><span class="sxs-lookup"><span data-stu-id="8da4a-186">**To create a self-signed root certificate**</span></span>

1. <span data-ttu-id="8da4a-187">A munkaállomáson nyisson meg egy parancssori ablakot.</span><span class="sxs-lookup"><span data-stu-id="8da4a-187">From your workstation, open a command prompt window.</span></span>
2. <span data-ttu-id="8da4a-188">Keresse meg a Visual Studio eszközök mappát.</span><span class="sxs-lookup"><span data-stu-id="8da4a-188">Navigate to the Visual Studio tools folder.</span></span>
3. <span data-ttu-id="8da4a-189">A következő parancsot az alábbi példa létrehoz és egy legfelső szintű tanúsítvány telepítése a munkaállomáson a személyes tanúsítványtárolóban, és is létrehozhat egy megfelelő .cer kiterjesztésű fájlba, amely a klasszikus Azure portál később fogja feltölteni.</span><span class="sxs-lookup"><span data-stu-id="8da4a-189">The following command in the example below will create and install a root certificate in the Personal certificate store on your workstation and also create a corresponding .cer file that you’ll later upload to the Azure Classic Portal.</span></span>

        makecert -sky exchange -r -n "CN=HBaseVnetVPNRootCertificate" -pe -a sha1 -len 2048 -ss My "C:\Users\JohnDole\Desktop\HBaseVNetVPNRootCertificate.cer"

    <span data-ttu-id="8da4a-190">Nyissa meg azt a könyvtárat, amelyet a .cer fájl található, ahol HBaseVnetVPNRootCertificate-e be a tanúsítvány nevét.</span><span class="sxs-lookup"><span data-stu-id="8da4a-190">Change to the directory that you want the .cer file to be located in, where HBaseVnetVPNRootCertificate is the name that you want to use for the certificate.</span></span>

    <span data-ttu-id="8da4a-191">Ne zárja be a parancssort.</span><span class="sxs-lookup"><span data-stu-id="8da4a-191">Don't close the command prompt.</span></span>  <span data-ttu-id="8da4a-192">Szüksége lesz rájuk a következő eljárásban.</span><span class="sxs-lookup"><span data-stu-id="8da4a-192">You will need it in the next procedure.</span></span>

   > [!NOTE]
   > <span data-ttu-id="8da4a-193">Mivel létrehozta a főtanúsítványt, amelyből létrejönnek az ügyféltanúsítványok, érdemes exportálni ezt a tanúsítványt a titkos kulcsával együtt és menteni egy olyan biztonságos helyen, ahonnan később helyreállítható.</span><span class="sxs-lookup"><span data-stu-id="8da4a-193">Because you have created a root certificate from which client certificates will be generated, you may want to export this certificate along with its private key and save it to a safe location where it may be recovered.</span></span>
   >
   >

<span data-ttu-id="8da4a-194">**Ügyfél-tanúsítvány létrehozása**</span><span class="sxs-lookup"><span data-stu-id="8da4a-194">**To create a client certificate**</span></span>

* <span data-ttu-id="8da4a-195">A parancssorból azonos (ez nem lehet ugyanazon a számítógépen, amelyen létrehozta a legfelső szintű tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="8da4a-195">From the same command prompt (It has to be on the same computer where you created the root certificate.</span></span> <span data-ttu-id="8da4a-196">Az ügyfél-tanúsítványt kell létrejönnie, a legfelső szintű tanúsítványa), a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="8da4a-196">The client certificate must be generated from the root certificate), run the following command:</span></span>

          makecert.exe -n "CN=HBaseVnetVPNClientCertificate" -pe -sky exchange -m 96 -ss My -in "HBaseVnetVPNRootCertificate" -is my -a sha1

    <span data-ttu-id="8da4a-197">HBaseVnetVPNRootCertificate a legfelső szintű tanúsítvány neve.</span><span class="sxs-lookup"><span data-stu-id="8da4a-197">HBaseVnetVPNRootCertificate is the root certificate name.</span></span>  <span data-ttu-id="8da4a-198">A legfelső szintű tanúsítvány névre van.</span><span class="sxs-lookup"><span data-stu-id="8da4a-198">It has to match the root certificate name.</span></span>  

    <span data-ttu-id="8da4a-199">A legfelső szintű tanúsítvány, mind az ügyfél-tanúsítványt a számítógép személyes tanúsítványtárolójában vannak tárolva.</span><span class="sxs-lookup"><span data-stu-id="8da4a-199">Both the root certificate and the client certificate are stored in your Personal certificate store on your computer.</span></span> <span data-ttu-id="8da4a-200">Certmgr.msc segítségével ellenőrizheti.</span><span class="sxs-lookup"><span data-stu-id="8da4a-200">Use certmgr.msc to verify.</span></span>

    ![Azure-beli virtuális hálózat pont-pont VPN-tanúsítványának][img-certificate]

    <span data-ttu-id="8da4a-202">Az összes, a virtuális hálózathoz csatlakoztatni kívánt számítógépen telepíteni kell egy ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="8da4a-202">A client certificate must be installed on each computer that you want to connect to the virtual network.</span></span> <span data-ttu-id="8da4a-203">Ajánlott egyedi ügyféltanúsítványok létrehozása minden olyan számítógéphez, amelyet csatlakoztatni szeretne a virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="8da4a-203">We recommend that you create unique client certificates for each computer that you want to connect to the virtual network.</span></span> <span data-ttu-id="8da4a-204">Az ügyféltanúsítványok exportálásához használja certmgr.msc.</span><span class="sxs-lookup"><span data-stu-id="8da4a-204">To export the client certificates, use certmgr.msc.</span></span>

<span data-ttu-id="8da4a-205">**A klasszikus Azure portálra a legfelső szintű tanúsítvány feltöltése**</span><span class="sxs-lookup"><span data-stu-id="8da4a-205">**To upload the root certificate to the Azure Classic Portal**</span></span>

1. <span data-ttu-id="8da4a-206">A klasszikus Azure portál, kattintson az **hálózati** a bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="8da4a-206">From the Azure Classic Portal, click **NETWORK** on the left.</span></span>
2. <span data-ttu-id="8da4a-207">Kattintson a virtuális hálózat, amelyen üzembe van helyezve a HBase fürt számára.</span><span class="sxs-lookup"><span data-stu-id="8da4a-207">Click the virtual network where your HBase cluster is deployed to.</span></span>
3. <span data-ttu-id="8da4a-208">Kattintson a **tanúsítványok** a lista elejéről.</span><span class="sxs-lookup"><span data-stu-id="8da4a-208">Click **CERTIFICATES** from the top.</span></span>
4. <span data-ttu-id="8da4a-209">Kattintson a **FELTÖLTÉSE** a lista aljáról, és adja meg a főtanúsítványfájlt előtti eljárásban létrehozott.</span><span class="sxs-lookup"><span data-stu-id="8da4a-209">Click **UPLOAD** from the bottom, and specify the root certificate file you have created in the procedure before last.</span></span> <span data-ttu-id="8da4a-210">Várjon, amíg a tanúsítvány lett importálva.</span><span class="sxs-lookup"><span data-stu-id="8da4a-210">Wait until the certificate got imported.</span></span>
5. <span data-ttu-id="8da4a-211">Kattintson a **IRÁNYÍTÓPULT** felső.</span><span class="sxs-lookup"><span data-stu-id="8da4a-211">Click **DASHBOARD** on the top.</span></span>  <span data-ttu-id="8da4a-212">A virtuális diagram állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8da4a-212">The virtual diagram shows the status.</span></span>

#### <a name="configure-your-vpn-client"></a><span data-ttu-id="8da4a-213">A VPN-ügyfél konfigurálása</span><span class="sxs-lookup"><span data-stu-id="8da4a-213">Configure your VPN client</span></span>
<span data-ttu-id="8da4a-214">**Töltse le, és a VPN-ügyfélcsomag telepítése**</span><span class="sxs-lookup"><span data-stu-id="8da4a-214">**To download and install the client VPN package**</span></span>

1. <span data-ttu-id="8da4a-215">Kattintson az IRÁNYÍTÓPULT-oldalon, a virtuális hálózat, a gyors áttekintő területen **a 64 bites ügyfél VPN-csomag** vagy **a 32 bites ügyfél VPN-csomag** munkaállomást az operációs rendszer verziójától függően.</span><span class="sxs-lookup"><span data-stu-id="8da4a-215">From the DASHBOARD page of the virtual network, in the quick glance section, click either **Download the 64-bit Client VPN Package** or **Download the 32-bit Client VPN Package** based on your workstation OS version.</span></span>
2. <span data-ttu-id="8da4a-216">Kattintson a **futtatása** a csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8da4a-216">Click **Run** to install the package.</span></span>
3. <span data-ttu-id="8da4a-217">Kattintson a biztonsági parancssorba **további információ**, és kattintson a **mégis futtatni**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-217">At the security prompt, click **More info**, and then click **Run anyway**.</span></span>
4. <span data-ttu-id="8da4a-218">Kattintson a **Igen** kétszer.</span><span class="sxs-lookup"><span data-stu-id="8da4a-218">Click **Yes** twice.</span></span>

<span data-ttu-id="8da4a-219">**A csatlakozás VPN hálózathoz**</span><span class="sxs-lookup"><span data-stu-id="8da4a-219">**To connect to VPN**</span></span>

1. <span data-ttu-id="8da4a-220">A munkaállomás az asztalon kattintson a tálcán hálózatok ikonjára.</span><span class="sxs-lookup"><span data-stu-id="8da4a-220">On the desktop of your workstation, click the Networks icon on the Task bar.</span></span> <span data-ttu-id="8da4a-221">A virtuális hálózati név látni fog egy VPN-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="8da4a-221">You shall see a VPN connection with your virtual network name.</span></span>
2. <span data-ttu-id="8da4a-222">Kattintson a VPN-kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="8da4a-222">Click the VPN connection name.</span></span>
3. <span data-ttu-id="8da4a-223">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-223">Click **Connect**.</span></span>

<span data-ttu-id="8da4a-224">**A virtuális Magánhálózati kapcsolat és a tartomány névfeloldás tesztelése**</span><span class="sxs-lookup"><span data-stu-id="8da4a-224">**To test the VPN connection and domain name resolution**</span></span>

* <span data-ttu-id="8da4a-225">A munkaállomás nyisson meg egy parancssort, és egyet a következő nevekre a HBase-fürtöt DNS-utótag ping myhbase.b7.internal.cloudapp.net:</span><span class="sxs-lookup"><span data-stu-id="8da4a-225">From the workstation, open a command prompt and ping one of the following names given the HBase cluster's DNS suffix is myhbase.b7.internal.cloudapp.net:</span></span>

        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        zookeeper0.myhbase.b7.internal.cloudapp.net
        headnode0.myhbase.b7.internal.cloudapp.net
        headnode1.myhbase.b7.internal.cloudapp.net
        workernode0.myhbase.b7.internal.cloudapp.net

### <a name="install-and-configure-squirrel-on-your-workstation"></a><span data-ttu-id="8da4a-226">Telepítse és konfigurálja az SQuirreL a munkaállomáson</span><span class="sxs-lookup"><span data-stu-id="8da4a-226">Install and configure SQuirreL on your workstation</span></span>
<span data-ttu-id="8da4a-227">**SQuirreL telepítése**</span><span class="sxs-lookup"><span data-stu-id="8da4a-227">**To install SQuirreL**</span></span>

1. <span data-ttu-id="8da4a-228">Töltse le a SQuirreL SQL ügyfél jar-fájlra a [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span><span class="sxs-lookup"><span data-stu-id="8da4a-228">Download the SQuirreL SQL client jar file from [http://squirrel-sql.sourceforge.net/#installation](http://squirrel-sql.sourceforge.net/#installation).</span></span>
2. <span data-ttu-id="8da4a-229">Megnyitás/Futtatás a jar-fájlra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-229">Open/run the jar file.</span></span> <span data-ttu-id="8da4a-230">Van szükség a [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span><span class="sxs-lookup"><span data-stu-id="8da4a-230">It requires the [Java Runtime Environment](http://www.oracle.com/technetwork/java/javase/downloads/jre7-downloads-1880261.html).</span></span>
3. <span data-ttu-id="8da4a-231">Kattintson a **következő** kétszer.</span><span class="sxs-lookup"><span data-stu-id="8da4a-231">Click **Next** twice.</span></span>
4. <span data-ttu-id="8da4a-232">Adjon meg egy elérési utat, ahol írási engedélye, és kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-232">Specify a path where you have the write permission, and then click **Next**.</span></span>

  > [!NOTE]
  > <span data-ttu-id="8da4a-233">Az alapértelmezett telepítési mappa a C:\Program Files\squirrel-sql-3.6 mappában van.</span><span class="sxs-lookup"><span data-stu-id="8da4a-233">The default installation folder is in the C:\Program Files\squirrel-sql-3.6 folder.</span></span>  <span data-ttu-id="8da4a-234">Ahhoz, hogy az elérési út írni, a telepítő meg kell adni a rendszergazdai jogosultság.</span><span class="sxs-lookup"><span data-stu-id="8da4a-234">In order to write to this path, the installer must be granted the administrator privilege.</span></span> <span data-ttu-id="8da4a-235">Nyisson meg egy parancssort rendszergazdaként, keresse meg a Java a bin mappát, és futtassa:</span><span class="sxs-lookup"><span data-stu-id="8da4a-235">You can open a command prompt as administrator, navigate to Java's bin folder, and then run:</span></span>
  >
  >     <span data-ttu-id="8da4a-236">Java.exe-jar [az SQuirreL jar-fájl elérési útja]</span><span class="sxs-lookup"><span data-stu-id="8da4a-236">java.exe -jar [the path of the SQuirreL jar file]</span></span>
5. <span data-ttu-id="8da4a-237">Kattintson a **OK** létrehozása a célkönyvtárat megerősítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8da4a-237">Click **OK** to confirm creating the target directory.</span></span>
6. <span data-ttu-id="8da4a-238">Az alapértelmezett érték a kiinduló és a Standard csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8da4a-238">The default setting is to install the Base and Standard packages.</span></span>  <span data-ttu-id="8da4a-239">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-239">Click **Next**.</span></span>
7. <span data-ttu-id="8da4a-240">Kattintson a **következő** kétszer, majd **végzett**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-240">Click **Next** twice, and then click **Done**.</span></span>

<span data-ttu-id="8da4a-241">**A Phoenix illesztőprogram telepítése**</span><span class="sxs-lookup"><span data-stu-id="8da4a-241">**To install the Phoenix driver**</span></span>

<span data-ttu-id="8da4a-242">A phoenix illesztőprogram jar-fájlra a HBase-fürtöt található.</span><span class="sxs-lookup"><span data-stu-id="8da4a-242">The phoenix driver jar file is located on the HBase cluster.</span></span> <span data-ttu-id="8da4a-243">Az elérési út verziók alapján a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="8da4a-243">The path is similar to the following based on the versions:</span></span>

    C:\apps\dist\phoenix-4.0.0.2.1.11.0-2316\phoenix-4.0.0.2.1.11.0-2316-client.jar
<span data-ttu-id="8da4a-244">Másolja a munkaállomás alatt [SQuirreL telepítési mappa] / lib elérési utat kell.</span><span class="sxs-lookup"><span data-stu-id="8da4a-244">You need to copy it to your workstation under the [SQuirreL installation folder]/lib path.</span></span>  <span data-ttu-id="8da4a-245">A legegyszerűbb módja az, hogy az RDP a fürthöz, és fájlt másolja és illessze be (CTRL + C és CTRL + V) másolja azt a munkaállomás használja.</span><span class="sxs-lookup"><span data-stu-id="8da4a-245">The easiest way is to RDP into the cluster, and then use file copy/paste (CTRL+C and CTRL+V) to copy it to your workstation.</span></span>

<span data-ttu-id="8da4a-246">**A Phoenix illesztőprogram hozzáadása SQuirreL**</span><span class="sxs-lookup"><span data-stu-id="8da4a-246">**To add a Phoenix driver to SQuirreL**</span></span>

1. <span data-ttu-id="8da4a-247">Nyissa meg a SQuirreL SQL ügyfél a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="8da4a-247">Open SQuirreL SQL Client from your workstation.</span></span>
2. <span data-ttu-id="8da4a-248">Kattintson a **illesztőprogram** a bal oldali lapon.</span><span class="sxs-lookup"><span data-stu-id="8da4a-248">Click the **Driver** tab on the left.</span></span>
3. <span data-ttu-id="8da4a-249">Az a **illesztőprogramok** menüben kattintson a **új illesztőprogram**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-249">From the **Drivers** menu, click **New Driver**.</span></span>
4. <span data-ttu-id="8da4a-250">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8da4a-250">Enter the following information:</span></span>

   * <span data-ttu-id="8da4a-251">**Név**: Phoenix</span><span class="sxs-lookup"><span data-stu-id="8da4a-251">**Name**: Phoenix</span></span>
   * <span data-ttu-id="8da4a-252">**Példa URL-cím**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="8da4a-252">**Example URL**: jdbc:phoenix:zookeeper2.contoso-hbase-eu.f5.internal.cloudapp.net</span></span>
   * <span data-ttu-id="8da4a-253">**Osztálynév**: org.apache.phoenix.jdbc.PhoenixDriver</span><span class="sxs-lookup"><span data-stu-id="8da4a-253">**Class Name**: org.apache.phoenix.jdbc.PhoenixDriver</span></span>

     > [!WARNING]
     > <span data-ttu-id="8da4a-254">Felhasználó összes kisbetű, például URL-címét.</span><span class="sxs-lookup"><span data-stu-id="8da4a-254">User all lower case in the Example URL.</span></span> <span data-ttu-id="8da4a-255">Használhatja azokat teljes zookeeper kvórum abban az esetben, ha ezek egyikét nem működik.</span><span class="sxs-lookup"><span data-stu-id="8da4a-255">You can use they full zookeeper quorum in case one of them is down.</span></span>  <span data-ttu-id="8da4a-256">A gazdagép neve a következők: zookeeper0, zookeeper1 és zookeeper2.</span><span class="sxs-lookup"><span data-stu-id="8da4a-256">The hostnames are zookeeper0, zookeeper1, and zookeeper2.</span></span>
     >
     >

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-driver]
5. <span data-ttu-id="8da4a-258">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-258">Click **OK**.</span></span>

<span data-ttu-id="8da4a-259">**A HBase-fürtöt az alias létrehozása**</span><span class="sxs-lookup"><span data-stu-id="8da4a-259">**To create an alias to the HBase cluster**</span></span>

1. <span data-ttu-id="8da4a-260">SQuirreL, kattintson a **aliasok** a bal oldali lapon.</span><span class="sxs-lookup"><span data-stu-id="8da4a-260">From SQuirreL, Click the **Aliases** tab on the left.</span></span>
2. <span data-ttu-id="8da4a-261">Az a **aliasok** menüben kattintson a **új Alias**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-261">From the **Aliases** menu, click **New Alias**.</span></span>
3. <span data-ttu-id="8da4a-262">Adja meg a következő információkat:</span><span class="sxs-lookup"><span data-stu-id="8da4a-262">Enter the following information:</span></span>

   * <span data-ttu-id="8da4a-263">**Név**: a HBase-fürtöt, vagy inkább nevek nevét.</span><span class="sxs-lookup"><span data-stu-id="8da4a-263">**Name**: The name of the HBase cluster or any name you prefer.</span></span>
   * <span data-ttu-id="8da4a-264">**Illesztőprogram**: Phoenix.</span><span class="sxs-lookup"><span data-stu-id="8da4a-264">**Driver**: Phoenix.</span></span>  <span data-ttu-id="8da4a-265">Ennek egyeznie kell az illesztőprogram az utolsó eljárás létrehozott nevét.</span><span class="sxs-lookup"><span data-stu-id="8da4a-265">This must match the driver name you created in the last procedure.</span></span>
   * <span data-ttu-id="8da4a-266">**URL-cím**: az URL-címet a rendszer átmásolja az illesztőprogram-konfigurációjáról.</span><span class="sxs-lookup"><span data-stu-id="8da4a-266">**URL**: The URL is copied from your driver configuration.</span></span> <span data-ttu-id="8da4a-267">Ellenőrizze, hogy a felhasználó minden kisbetű.</span><span class="sxs-lookup"><span data-stu-id="8da4a-267">Make sure to user all lower case.</span></span>
   * <span data-ttu-id="8da4a-268">**Felhasználónév**: szöveg lehet.</span><span class="sxs-lookup"><span data-stu-id="8da4a-268">**User name**: It can be any text.</span></span>  <span data-ttu-id="8da4a-269">Itt adhat meg VPN-kapcsolatot, mert a felhasználónév nem minden használja.</span><span class="sxs-lookup"><span data-stu-id="8da4a-269">Because you use VPN connectivity here, the user name is not used at all.</span></span>
   * <span data-ttu-id="8da4a-270">**Jelszó**: szöveg lehet.</span><span class="sxs-lookup"><span data-stu-id="8da4a-270">**Password**: It can be any text.</span></span>

     ![HDInsight HBase Phoenix SQuirreL illesztőprogram][img-squirrel-alias]
4. <span data-ttu-id="8da4a-272">Kattintson a **teszt**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-272">Click **Test**.</span></span>
5. <span data-ttu-id="8da4a-273">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-273">Click **Connect**.</span></span> <span data-ttu-id="8da4a-274">Ha a kapcsolat, SQuirreL néz ki:</span><span class="sxs-lookup"><span data-stu-id="8da4a-274">When it makes the connection, SQuirreL looks like:</span></span>

    ![A HBase Phoenix SQuirreL][img-squirrel]

<span data-ttu-id="8da4a-276">**Egy teszt futtatásához**</span><span class="sxs-lookup"><span data-stu-id="8da4a-276">**To run a test**</span></span>

1. <span data-ttu-id="8da4a-277">Kattintson a **SQL** lap jobb mellett a **objektumok** fülre.</span><span class="sxs-lookup"><span data-stu-id="8da4a-277">Click the **SQL** tab right next to the **Objects** tab.</span></span>
2. <span data-ttu-id="8da4a-278">Másolja és illessze be a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="8da4a-278">Copy and paste the following code:</span></span>

        CREATE TABLE IF NOT EXISTS us_population (state CHAR(2) NOT NULL, city VARCHAR NOT NULL, population BIGINT  CONSTRAINT my_pk PRIMARY KEY (state, city))
3. <span data-ttu-id="8da4a-279">A Futtatás gombra.</span><span class="sxs-lookup"><span data-stu-id="8da4a-279">Click the run button.</span></span>

    ![A HBase Phoenix SQuirreL][img-squirrel-sql]
4. <span data-ttu-id="8da4a-281">Váltás a **objektumok** fülre.</span><span class="sxs-lookup"><span data-stu-id="8da4a-281">Switch back to the **Objects** tab.</span></span>
5. <span data-ttu-id="8da4a-282">Bontsa ki a alias nevét, és végül **tábla**.</span><span class="sxs-lookup"><span data-stu-id="8da4a-282">Expand the alias name, and then expand **TABLE**.</span></span>  <span data-ttu-id="8da4a-283">Látni fogja az új táblázat területén.</span><span class="sxs-lookup"><span data-stu-id="8da4a-283">You shall see the new table listed under.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8da4a-284">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8da4a-284">Next steps</span></span>
<span data-ttu-id="8da4a-285">Ebben a cikkben megtanulta rendelkezik Apache Phoenix használata a Hdinsightban.</span><span class="sxs-lookup"><span data-stu-id="8da4a-285">In this article, you have learned how to use Apache Phoenix in HDInsight.</span></span>  <span data-ttu-id="8da4a-286">További tudnivalókért lásd:</span><span class="sxs-lookup"><span data-stu-id="8da4a-286">To learn more, see</span></span>

* <span data-ttu-id="8da4a-287">[HDInsight HBase overview][hdinsight-hbase-overview] (A HDInsight HBase áttekintése): A HBase egy Apache, nyílt forráskódú, a Hadoopra épülő NoSQL-adatbázis, amely véletlenszerű hozzáférést és erős konzisztenciát biztosít a nagy mennyiségű strukturálatlan és félig strukturált adatok számára.</span><span class="sxs-lookup"><span data-stu-id="8da4a-287">[HDInsight HBase overview][hdinsight-hbase-overview]: HBase is an Apache, open-source, NoSQL database built on Hadoop that provides random access and strong consistency for large amounts of unstructured and semistructured data.</span></span>
* <span data-ttu-id="8da4a-288">[Azure virtuális hálózat HBase-fürtök kiépítése][hdinsight-hbase-provision-vnet]: A virtuális hálózati integráció, HBase-fürtökkel telepíthetők az alkalmazások azonos virtuális hálózaton, hogy az alkalmazások közvetlenül kommunikálhatnak a HBase.</span><span class="sxs-lookup"><span data-stu-id="8da4a-288">[Provision HBase clusters on Azure Virtual Network][hdinsight-hbase-provision-vnet]: With virtual network integration, HBase clusters can be deployed to the same virtual network as your applications so that applications can communicate with HBase directly.</span></span>
* <span data-ttu-id="8da4a-289">[A HDInsight HBase-replikálás konfigurálása](hdinsight-hbase-replication.md): HBase-replikálás konfigurálása az Azure két adatközpont között.</span><span class="sxs-lookup"><span data-stu-id="8da4a-289">[Configure HBase replication in HDInsight](hdinsight-hbase-replication.md): Learn how to configure HBase replication across two Azure datacenters.</span></span>
* <span data-ttu-id="8da4a-290">[Elemzése a HBase a hdinsight eszközben Twitter sentiment][hbase-twitter-sentiment]: valós idejű módjáról [véleményeket elemzés](http://en.wikipedia.org/wiki/Sentiment_analysis) a big Data típusú adatok a HBase a hdinsight Hadoop-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="8da4a-290">[Analyze Twitter sentiment with HBase in HDInsight][hbase-twitter-sentiment]: Learn how to do real-time [sentiment analysis](http://en.wikipedia.org/wiki/Sentiment_analysis) of big data by using HBase in a Hadoop cluster in HDInsight.</span></span>

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
