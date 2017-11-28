---
title: "aaaUse SSH-bújtatás tooaccess Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse egy SSH-alagút toosecurely keresse meg a Linux-alapú HDInsight-csomópontok webes erőforrásaihoz."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 879834a4-52d0-499c-a3ae-8d28863abf65
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/21/2017
ms.author: larryfr
ms.openlocfilehash: 8a315c53751bbe6950a182674f4108c67c2f2f8e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-ssh-tunneling-tooaccess-ambari-web-ui-jobhistory-namenode-oozie-and-other-web-uis"></a><span data-ttu-id="3c9d0-103">SSH Tunneling tooaccess Ambari webes felhasználói felület, JobHistory, NameNode, Oozie és egyéb web UI használata</span><span class="sxs-lookup"><span data-stu-id="3c9d0-103">Use SSH Tunneling tooaccess Ambari web UI, JobHistory, NameNode, Oozie, and other web UIs</span></span>

<span data-ttu-id="3c9d0-104">Linux-alapú HDInsight-fürtök biztosítanak hozzáférést tooAmbari webes felhasználói felület hello interneten keresztül, de néhány hello UI-funkciók nem állnak.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-104">Linux-based HDInsight clusters provide access tooAmbari web UI over hello Internet, but some features of hello UI are not.</span></span> <span data-ttu-id="3c9d0-105">Például hello webes felhasználói felületen keresztül Ambari illesztett más szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-105">For example, hello web UI for other services that are surfaced through Ambari.</span></span> <span data-ttu-id="3c9d0-106">Hello Ambari webes felhasználói felület funkcióját egy SSH-alagút toohello fürt head kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-106">For full functionality of hello Ambari web UI, you must use an SSH tunnel toohello cluster head.</span></span>

## <a name="why-use-an-ssh-tunnel"></a><span data-ttu-id="3c9d0-107">Miért érdemes használni az SSH-alagút</span><span class="sxs-lookup"><span data-stu-id="3c9d0-107">Why use an SSH tunnel</span></span>

<span data-ttu-id="3c9d0-108">Az Ambari hello menük számos csak támogatott eszközplatformokon működnek az SSH-alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-108">Several of hello menus in Ambari only work through an SSH tunnel.</span></span> <span data-ttu-id="3c9d0-109">Ezek a menük webhelyek és az egyéb csomóponttípusok, például a feldolgozó csomópontok szolgáltatás támaszkodnak.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-109">These menus rely on web sites and services running on other node types, such as worker nodes.</span></span> <span data-ttu-id="3c9d0-110">Gyakran ezek a webhelyek nem biztonságosak, így azt nem teszi közzé a biztonságos toodirectly azokat a hello internet.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-110">Often, these web sites are not secured, so it is not safe toodirectly expose them on hello internet.</span></span>

<span data-ttu-id="3c9d0-111">a következő Web UI hello SSH-alagút megkövetelése:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-111">hello following Web UIs require an SSH tunnel:</span></span>

* <span data-ttu-id="3c9d0-112">JobHistory</span><span class="sxs-lookup"><span data-stu-id="3c9d0-112">JobHistory</span></span>
* <span data-ttu-id="3c9d0-113">NameNode</span><span class="sxs-lookup"><span data-stu-id="3c9d0-113">NameNode</span></span>
* <span data-ttu-id="3c9d0-114">A szál verem</span><span class="sxs-lookup"><span data-stu-id="3c9d0-114">Thread Stacks</span></span>
* <span data-ttu-id="3c9d0-115">Oozie webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="3c9d0-115">Oozie web UI</span></span>
* <span data-ttu-id="3c9d0-116">A HBase Master és a naplókat a felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="3c9d0-116">HBase Master and Logs UI</span></span>

<span data-ttu-id="3c9d0-117">A Parancsfájlműveletek toocustomize használatakor a fürt lehetnek szolgáltatások vagy a segédeszközök telepítése a webes felhasználói felület visszaállítását SSH-alagút szükséges.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-117">If you use Script Actions toocustomize your cluster, any services or utilities that you install that expose a web UI require an SSH tunnel.</span></span> <span data-ttu-id="3c9d0-118">Például ha egy parancsfájl műveletével Hue telepítése kell használnia egy SSH alagút tooaccess hello Hue webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-118">For example, if you install Hue using a Script Action, you must use an SSH tunnel tooaccess hello Hue web UI.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c9d0-119">Ha egy virtuális hálózaton keresztül a közvetlen hozzáférést tooHDInsight, nem kell toouse SSH-alagutat.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-119">If you have direct access tooHDInsight through a virtual network, you do not need toouse SSH tunnels.</span></span> <span data-ttu-id="3c9d0-120">Közvetlenül a virtuális hálózaton keresztül férnek hozzá a HDInsight példáért lásd: hello [csatlakozás HDInsight tooyour a helyszíni hálózat](connect-on-premises-network.md) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-120">For an example of directly accessing HDInsight through a virtual network, see hello [Connect HDInsight tooyour on-premises network](connect-on-premises-network.md) document.</span></span>

## <a name="what-is-an-ssh-tunnel"></a><span data-ttu-id="3c9d0-121">Mi az az SSH-alagút</span><span class="sxs-lookup"><span data-stu-id="3c9d0-121">What is an SSH tunnel</span></span>

<span data-ttu-id="3c9d0-122">[Secure Shell (SSH) bújtatás](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) irányítja a forgalmat tooa portot a helyi munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-122">[Secure Shell (SSH) tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) routes traffic sent tooa port on your local workstation.</span></span> <span data-ttu-id="3c9d0-123">hello forgalmat egy SSH kapcsolat tooyour HDInsight fürt átjárócsomópontjába keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-123">hello traffic is routed through an SSH connection tooyour HDInsight cluster head node.</span></span> <span data-ttu-id="3c9d0-124">hello kérelem meg van oldva, mintha hello átjárócsomópont származna.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-124">hello request is resolved as if it originated on hello head node.</span></span> <span data-ttu-id="3c9d0-125">hello válasz majd vissza hello alagút tooyour munkaállomás keresztül történik.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-125">hello response is then routed back through hello tunnel tooyour workstation.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c9d0-126">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3c9d0-126">Prerequisites</span></span>

* <span data-ttu-id="3c9d0-127">Egy SSH-ügyfél.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-127">An SSH client.</span></span> <span data-ttu-id="3c9d0-128">További információ: [Az SSH használata HDInsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3c9d0-128">For more information, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

* <span data-ttu-id="3c9d0-129">Egy olyan webböngésző, lehet toouse SOCKS5 proxy konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-129">A web browser that can be configured toouse a SOCKS5 proxy.</span></span>

    > [!WARNING]
    > <span data-ttu-id="3c9d0-130">beépített Windows hello SOCKS proxy támogatási SOCKS5 nem támogatja, és nem működik a jelen dokumentumban leírt lépések hello.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-130">hello SOCKS proxy support built into Windows does not support SOCKS5, and does not work with hello steps in this document.</span></span> <span data-ttu-id="3c9d0-131">hello alábbi böngészők Windows proxybeállítások alapulnak, és tegye jelenleg nem a jelen dokumentumban leírt lépések hello használata:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-131">hello following browsers rely on Windows proxy settings, and do not currently work with hello steps in this document:</span></span>
    >
    > * <span data-ttu-id="3c9d0-132">A Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="3c9d0-132">Microsoft Edge</span></span>
    > * <span data-ttu-id="3c9d0-133">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="3c9d0-133">Microsoft Internet Explorer</span></span>
    >
    > <span data-ttu-id="3c9d0-134">Google Chrome hello Windows proxybeállítások is támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-134">Google Chrome also relies on hello Windows proxy settings.</span></span> <span data-ttu-id="3c9d0-135">Bővítmények SOCKS5 támogató is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-135">However, you can install extensions that support SOCKS5.</span></span> <span data-ttu-id="3c9d0-136">Ajánlott [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span><span class="sxs-lookup"><span data-stu-id="3c9d0-136">We recommend [FoxyProxy Standard](https://chrome.google.com/webstore/detail/foxyproxy-standard/gcknhkkoolaabfmlnjonogaaifnjlfnp).</span></span>

## <span data-ttu-id="3c9d0-137"><a name="usessh"></a>Hozzon létre egy alagúton hello SSH-parancs használatával</span><span class="sxs-lookup"><span data-stu-id="3c9d0-137"><a name="usessh"></a>Create a tunnel using hello SSH command</span></span>

<span data-ttu-id="3c9d0-138">Használjon hello következő parancsot a toocreate az SSH-alagút segítségével hello `ssh` parancsot.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-138">Use hello following command toocreate an SSH tunnel using hello `ssh` command.</span></span> <span data-ttu-id="3c9d0-139">Cserélje le **felhasználónév** az SSH-felhasználó a HDInsight-fürtöt, és cserélje le a **CLUSTERNAME** hello nevet, a HDInsight-fürt:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-139">Replace **USERNAME** with an SSH user for your HDInsight cluster, and replace **CLUSTERNAME** with hello name of your HDInsight cluster:</span></span>

```bash
ssh -C2qTnNf -D 9876 USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
```

<span data-ttu-id="3c9d0-140">Ez a parancs kapcsolatot hoz létre, amely forgalmat toolocal port 9876 toohello fürthöz SSH-n keresztül.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-140">This command creates a connection that routes traffic toolocal port 9876 toohello cluster over SSH.</span></span> <span data-ttu-id="3c9d0-141">hello lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-141">hello options are:</span></span>

* <span data-ttu-id="3c9d0-142">**D 9876** -hello helyi port irányítja a forgalmat hello alagúton keresztül.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-142">**D 9876** - hello local port that routes traffic through hello tunnel.</span></span>
* <span data-ttu-id="3c9d0-143">**C** -tömörítése minden adatot, mert a webes forgalom leginkább szöveget.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-143">**C** - Compress all data, because web traffic is mostly text.</span></span>
* <span data-ttu-id="3c9d0-144">**2** -force SSH tootry protokoll 2-es verziójú csak.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-144">**2** - Force SSH tootry protocol version 2 only.</span></span>
* <span data-ttu-id="3c9d0-145">**a q** -csendes mód.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-145">**q** - Quiet mode.</span></span>
* <span data-ttu-id="3c9d0-146">**T** -pszeudo-tty kiosztását, tiltsa le, mivel jelenleg csupán továbbít egy portot.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-146">**T** - Disable pseudo-tty allocation, since we are just forwarding a port.</span></span>
* <span data-ttu-id="3c9d0-147">**n**– Előfordulhat, hogy olvasása STDIN, mivel jelenleg csupán továbbít egy portot.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-147">**n** - Prevent reading of STDIN, since we are just forwarding a port.</span></span>
* <span data-ttu-id="3c9d0-148">**N** -ne hajtsa végre a távoli parancstól, mivel jelenleg csupán továbbít egy portot.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-148">**N** - Do not execute a remote command, since we are just forwarding a port.</span></span>
* <span data-ttu-id="3c9d0-149">**f** -hello háttérben futnak.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-149">**f** - Run in hello background.</span></span>

<span data-ttu-id="3c9d0-150">Miután hello parancs elküldött forgalom tooport 9876 hello helyi számítógépen irányított toohello átjárócsomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-150">Once hello command finishes, traffic sent tooport 9876 on hello local computer is routed toohello cluster head node.</span></span>

## <span data-ttu-id="3c9d0-151"><a name="useputty"></a>A PuTTY használatával-alagút létrehozása</span><span class="sxs-lookup"><span data-stu-id="3c9d0-151"><a name="useputty"></a>Create a tunnel using PuTTY</span></span>

<span data-ttu-id="3c9d0-152">[A puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) egy grafikus SSH-ügyfél Windows.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-152">[PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty) is a graphical SSH client for Windows.</span></span> <span data-ttu-id="3c9d0-153">A következő lépéseket toocreate használja a PuTTY SSH-alagút hello használata:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-153">Use hello following steps toocreate an SSH tunnel using PuTTY:</span></span>

1. <span data-ttu-id="3c9d0-154">Nyissa meg a PuTTY, és adja meg a kapcsolódási adatokat.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-154">Open PuTTY, and enter your connection information.</span></span> <span data-ttu-id="3c9d0-155">Ha nem ismeri a PuTTY, lásd: hello [dokumentáció (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html) PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span><span class="sxs-lookup"><span data-stu-id="3c9d0-155">If you are not familiar with PuTTY, see hello [PuTTY documentation (http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html)](http://www.chiark.greenend.org.uk/~sgtatham/putty/docs.html).</span></span>

2. <span data-ttu-id="3c9d0-156">A hello **kategória** szakasz toohello hello párbeszédpanel bal oldali, bontsa ki a **kapcsolat**, bontsa ki a **SSH**, majd válassza ki **alagutak**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-156">In hello **Category** section toohello left of hello dialog, expand **Connection**, expand **SSH**, and then select **Tunnels**.</span></span>

3. <span data-ttu-id="3c9d0-157">Adja meg a következő információ a hello hello **vezérlő beállításokban SSH port átirányítása** űrlap:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-157">Provide hello following information on hello **Options controlling SSH port forwarding** form:</span></span>
   
   * <span data-ttu-id="3c9d0-158">**Forrásport** -port, hogy kívánja-e tooforward hello ügyfélen hello.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-158">**Source port** - hello port on hello client that you wish tooforward.</span></span> <span data-ttu-id="3c9d0-159">Például **9876**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-159">For example, **9876**.</span></span>

   * <span data-ttu-id="3c9d0-160">**Cél** -hello hello Linux-alapú HDInsight-fürthöz az SSH-cím.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-160">**Destination** - hello SSH address for hello Linux-based HDInsight cluster.</span></span> <span data-ttu-id="3c9d0-161">Például: **mycluster-ssh.azurehdinsight.net**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-161">For example, **mycluster-ssh.azurehdinsight.net**.</span></span>

   * <span data-ttu-id="3c9d0-162">**Dynamic** (Dinamikus) - Lehetővé teszi a dinamikus SOCKS proxy útválasztást.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-162">**Dynamic** - Enables dynamic SOCKS proxy routing.</span></span>
     
     ![beállítások tunneling képe](./media/hdinsight-linux-ambari-ssh-tunnel/puttytunnel.png)

4. <span data-ttu-id="3c9d0-164">Kattintson a **Hozzáadás** tooadd hello beállításait, és kattintson a **nyitott** tooopen az SSH-kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-164">Click **Add** tooadd hello settings, and then click **Open** tooopen an SSH connection.</span></span>

5. <span data-ttu-id="3c9d0-165">Amikor a rendszer kéri, jelentkezzen be toohello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-165">When prompted, log in toohello server.</span></span>

## <a name="use-hello-tunnel-from-your-browser"></a><span data-ttu-id="3c9d0-166">A böngészőből hello alagút használata</span><span class="sxs-lookup"><span data-stu-id="3c9d0-166">Use hello tunnel from your browser</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3c9d0-167">hello lépések Ez a szakasz használata hello Mozilla FireFox böngésző biztosít hello proxybeállításokat minden platformon.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-167">hello steps in this section use hello Mozilla FireFox browser, as it provides hello same proxy settings across all platforms.</span></span> <span data-ttu-id="3c9d0-168">Egyéb modern böngésző támogatja, például a Google Chrome, például a hello alagút FoxyProxy toowork bővítmény lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-168">Other modern browsers, such as Google Chrome, may require an extension such as FoxyProxy toowork with hello tunnel.</span></span>

1. <span data-ttu-id="3c9d0-169">Konfigurálhatja a böngésző toouse hello **localhost** és hello port használhatók, ha hello alagút, létre kell hozni egy **SOCKS v5** proxy.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-169">Configure hello browser toouse **localhost** and hello port you used when creating hello tunnel as a **SOCKS v5** proxy.</span></span> <span data-ttu-id="3c9d0-170">Ez hogyan meg hello Firefox beállításait.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-170">Here's what hello Firefox settings look like.</span></span> <span data-ttu-id="3c9d0-171">Ha 9876-as, módosítsa a hello port toohello egy használt:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-171">If you used a different port than 9876, change hello port toohello one you used:</span></span>
   
    ![a Firefox beállításai](./media/hdinsight-linux-ambari-ssh-tunnel/firefoxproxy.png)
   
   > [!NOTE]
   > <span data-ttu-id="3c9d0-173">Kiválasztása **távoli DNS** oldja fel a tartománynévrendszer (DNS) kérelmek hello HDInsight-fürt használatával.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-173">Selecting **Remote DNS** resolves Domain Name System (DNS) requests by using hello HDInsight cluster.</span></span> <span data-ttu-id="3c9d0-174">Ez a beállítás hello fürt átjárócsomópontjához hello segítségével oldja fel.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-174">This setting resolves DNS using hello head node of hello cluster.</span></span>

2. <span data-ttu-id="3c9d0-175">Ellenőrizze, hogy hello alagút működik, mint egy webhely felkeresésével [http://www.whatismyip.com/](http://www.whatismyip.com/).</span><span class="sxs-lookup"><span data-stu-id="3c9d0-175">Verify that hello tunnel works by visiting a site such as [http://www.whatismyip.com/](http://www.whatismyip.com/).</span></span> <span data-ttu-id="3c9d0-176">hello IP visszaadott egyik használják hello Microsoft Azure datacenter.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-176">hello IP returned should be one used by hello Microsoft Azure datacenter.</span></span>

## <a name="verify-with-ambari-web-ui"></a><span data-ttu-id="3c9d0-177">Ellenőrizze a következővel Ambari webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="3c9d0-177">Verify with Ambari web UI</span></span>

<span data-ttu-id="3c9d0-178">Hello fürt létrehozása után használja a következő lépéseket tooverify, hogy elérhető szolgáltatás web UI hello Ambari Web hello:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-178">Once hello cluster has been established, use hello following steps tooverify that you can access service web UIs from hello Ambari Web:</span></span>

1. <span data-ttu-id="3c9d0-179">A böngészőben nyissa meg toohttp://headnodehost:8080.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-179">In your browser, go toohttp://headnodehost:8080.</span></span> <span data-ttu-id="3c9d0-180">Hello `headnodehost` cím hello alagút toohello fürt átküldött, és oldja meg, hogy fut-Ambari toohello headnode.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-180">hello `headnodehost` address is sent over hello tunnel toohello cluster and resolve toohello headnode that Ambari is running on.</span></span> <span data-ttu-id="3c9d0-181">Amikor a rendszer kéri, adja meg (rendszergazda) hello rendszergazda felhasználónevét és jelszavát a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-181">When prompted, enter hello admin user name (admin) and password for your cluster.</span></span> <span data-ttu-id="3c9d0-182">Kérheti másodszor hello Ambari webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-182">You may be prompted a second time by hello Ambari web UI.</span></span> <span data-ttu-id="3c9d0-183">Ha igen, adja meg újból hello információkat.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-183">If so, reenter hello information.</span></span>

   > [!NOTE]
   > <span data-ttu-id="3c9d0-184">Hello http://headnodehost:8080 cím tooconnect toohello fürt használatakor hello alagúton keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-184">When using hello http://headnodehost:8080 address tooconnect toohello cluster, you are connecting through hello tunnel.</span></span> <span data-ttu-id="3c9d0-185">Kommunikációs használatával lett biztonságossá téve hello SSH-alagút HTTPS kapcsolat helyett.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-185">Communication is secured using hello SSH tunnel instead of HTTPS.</span></span> <span data-ttu-id="3c9d0-186">tooconnect több mint hello internetkapcsolat, és a HTTPS, használja a https://CLUSTERNAME.azurehdinsight.net, ahol **CLUSTERNAME** hello fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-186">tooconnect over hello internet using HTTPS, use https://CLUSTERNAME.azurehdinsight.net, where **CLUSTERNAME** is hello name of hello cluster.</span></span>

2. <span data-ttu-id="3c9d0-187">Hello Ambari webes felhasználói felületén HDFS hello bal hello lap hello listáról válassza ki.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-187">From hello Ambari Web UI, select HDFS from hello list on hello left of hello page.</span></span>

    ![A kiválasztott HDFS kép](./media/hdinsight-linux-ambari-ssh-tunnel/hdfsservice.png)

3. <span data-ttu-id="3c9d0-189">Amikor hello HDFS-szolgáltatás adatait megjelenik, válassza ki a **Gyorshivatkozások**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-189">When hello HDFS service information is displayed, select **Quick Links**.</span></span> <span data-ttu-id="3c9d0-190">Hello központi fürtcsomópontok listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-190">A list of hello cluster head nodes appears.</span></span> <span data-ttu-id="3c9d0-191">Hello átjárócsomópontokkal egyikét, majd válassza ki és **NameNode felhasználói felület**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-191">Select one of hello head nodes, and then select **NameNode UI**.</span></span>

    ![Hello QuickLinks menü rendszerképének kibontva](./media/hdinsight-linux-ambari-ssh-tunnel/namenodedropdown.png)

   > [!NOTE]
   > <span data-ttu-id="3c9d0-193">Ha bejelöli __Gyorshivatkozások__, várjon, amíg mutató kaphat.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-193">When you select __Quick Links__, you may get a wait indicator.</span></span> <span data-ttu-id="3c9d0-194">Ez az állapot akkor fordulhat elő, ha lassú internetkapcsolattal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-194">This condition can occur if you have a slow internet connection.</span></span> <span data-ttu-id="3c9d0-195">Várjon egy percet, amíg hello adatok toobe hello kiszolgálótól kapott, és próbálkozzon újra a hello listája.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-195">Wait a minute or two for hello data toobe received from hello server, then try hello list again.</span></span>
   >
   > <span data-ttu-id="3c9d0-196">Néhány elemet a hello **Gyorshivatkozások** menü előfordulhat, hogy elszigeteli hello jobb oldalán üdvözlő képernyőt.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-196">Some entries in hello **Quick Links** menu may be cut off by hello right side of hello screen.</span></span> <span data-ttu-id="3c9d0-197">Ha igen, bontsa ki az egér használatával hello menü és hello jobbra mutató nyílra kulcs tooscroll hello képernyő toohello jobb toosee hello részeinek hello menü.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-197">If so, expand hello menu using your mouse and use hello right arrow key tooscroll hello screen toohello right toosee hello rest of hello menu.</span></span>

4. <span data-ttu-id="3c9d0-198">A kép a következő lap hasonló toohello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-198">A page similar toohello following image is displayed:</span></span>

    ![Hello NameNode felhasználói felület képe](./media/hdinsight-linux-ambari-ssh-tunnel/namenode.png)

   > [!NOTE]
   > <span data-ttu-id="3c9d0-200">Figyelje meg ezen a lapon; hello URL-címe meg kell hasonló túl**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/fürt**.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-200">Notice hello URL for this page; it should be similar too**http://hn1-CLUSTERNAME.randomcharacters.cx.internal.cloudapp.net:8088/cluster**.</span></span> <span data-ttu-id="3c9d0-201">Ezt az URI hello belső teljesen minősített tartománynevét (FQDN) hello csomópont használ, és csak elérhető az SSH-alagút használatakor.</span><span class="sxs-lookup"><span data-stu-id="3c9d0-201">This URI is using hello internal fully qualified domain name (FQDN) of hello node, and is only accessible when using an SSH tunnel.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c9d0-202">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3c9d0-202">Next steps</span></span>

<span data-ttu-id="3c9d0-203">Most, hogy megtanulta, hogyan toocreate és használja az SSH-alagút: hello más módokon toouse Ambari a dokumentum a következő:</span><span class="sxs-lookup"><span data-stu-id="3c9d0-203">Now that you have learned how toocreate and use an SSH tunnel, see hello following document for other ways toouse Ambari:</span></span>

* [<span data-ttu-id="3c9d0-204">A HDInsight-fürtök kezelése az Ambari segítségével</span><span class="sxs-lookup"><span data-stu-id="3c9d0-204">Manage HDInsight clusters by using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

<span data-ttu-id="3c9d0-205">Az SSH és a HDInsight együttes használatával további információkért lásd: [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md).</span><span class="sxs-lookup"><span data-stu-id="3c9d0-205">For more information on using SSH with HDInsight, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md).</span></span>

