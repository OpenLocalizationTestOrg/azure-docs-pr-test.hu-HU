---
title: "Az SSH használata a Hadoop–Azure HDInsight rendszerben | Microsoft Docs"
description: "A HDInsight a Secure Shell (SSH) segítségével érhető el. Ez a dokumentum a HDInsighthoz történő csatlakozásról ad információt Windows, Linux, Unix vagy macOS rendszerű ügyfelekről érkező ssh és scp parancsok használata esetén."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
keywords: "hadoop parancsok linux rendszerben, hadoop linux parancsok, hadoop macos, ssh hadoop, ssh hadoop fürt"
ms.assetid: a6a16405-a4a7-4151-9bbf-ab26972216c5
ms.service: hdinsight
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/03/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive,hdiseo17may2017
ms.openlocfilehash: df0feb51469333bac42c779d860192d46f24ac62
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="connect-to-hdinsight-hadoop-using-ssh"></a><span data-ttu-id="0461f-105">Csatlakozás a HDInsighthoz (Hadoop) SSH-val</span><span class="sxs-lookup"><span data-stu-id="0461f-105">Connect to HDInsight (Hadoop) using SSH</span></span>

<span data-ttu-id="0461f-106">Ismerje meg, hogyan használhatja a [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) eszközt az Azure HDInsight-alapú Hadoop-hoz való biztonságos csatlakozáshoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-106">Learn how to use [Secure Shell (SSH)](https://en.wikipedia.org/wiki/Secure_Shell) to securely connect to Hadoop on Azure HDInsight.</span></span> 

<span data-ttu-id="0461f-107">A HDInsight használhatja a Linux (Ubuntu) rendszert a Hadoop-fürt csomópontjainak operációs rendszereként.</span><span class="sxs-lookup"><span data-stu-id="0461f-107">HDInsight can use Linux (Ubuntu) as the operating system for nodes within the Hadoop cluster.</span></span> <span data-ttu-id="0461f-108">Ha egy SSH-ügyfél használatával Linux rendszerű HDInsighthoz csatlakozik, az alábbi táblázatban látható cím-és portinformációkra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="0461f-108">The following table contains the address and port information needed when connecting to Linux-based HDInsight using an SSH client:</span></span>

| <span data-ttu-id="0461f-109">Cím</span><span class="sxs-lookup"><span data-stu-id="0461f-109">Address</span></span> | <span data-ttu-id="0461f-110">Port</span><span class="sxs-lookup"><span data-stu-id="0461f-110">Port</span></span> | <span data-ttu-id="0461f-111">A következőhöz csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="0461f-111">Connects to...</span></span> |
| ----- | ----- | ----- |
| `<clustername>-ed-ssh.azurehdinsight.net` | <span data-ttu-id="0461f-112">22</span><span class="sxs-lookup"><span data-stu-id="0461f-112">22</span></span> | <span data-ttu-id="0461f-113">Élcsomópont (R Server a HDInsightban)</span><span class="sxs-lookup"><span data-stu-id="0461f-113">Edge node (R Server on HDInsight)</span></span> |
| `<edgenodename>.<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="0461f-114">22</span><span class="sxs-lookup"><span data-stu-id="0461f-114">22</span></span> | <span data-ttu-id="0461f-115">Élcsomópont (bármely egyéb fürttípus, ha létezik élcsomópont)</span><span class="sxs-lookup"><span data-stu-id="0461f-115">Edge node (any other cluster type, if an edge node exists)</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="0461f-116">22</span><span class="sxs-lookup"><span data-stu-id="0461f-116">22</span></span> | <span data-ttu-id="0461f-117">Elsődleges átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="0461f-117">Primary headnode</span></span> |
| `<clustername>-ssh.azurehdinsight.net` | <span data-ttu-id="0461f-118">23</span><span class="sxs-lookup"><span data-stu-id="0461f-118">23</span></span> | <span data-ttu-id="0461f-119">Másodlagos átjárócsomópont</span><span class="sxs-lookup"><span data-stu-id="0461f-119">Secondary headnode</span></span> |

> [!NOTE]
> <span data-ttu-id="0461f-120">Cserélje le az `<edgenodename>` elemet az élcsomópont nevére.</span><span class="sxs-lookup"><span data-stu-id="0461f-120">Replace `<edgenodename>` with the name of the edge node.</span></span>
>
> <span data-ttu-id="0461f-121">Cserélje le a `<clustername>` elemet a fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="0461f-121">Replace `<clustername>` with the name of your cluster.</span></span>
>
> <span data-ttu-id="0461f-122">Ha a fürt élcsomópontot tartalmaz, ajánlott __az élcsomóponthoz mindig__ az SSH segítségével kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="0461f-122">If your cluster contains an edge node, we recommend that you __always connect to the edge node__ using SSH.</span></span> <span data-ttu-id="0461f-123">Az átjárócsomópontok olyan szolgáltatásokat futtatnak, amelyek kritikus fontosságúak a Hadoop állapota szempontjából.</span><span class="sxs-lookup"><span data-stu-id="0461f-123">The head nodes host services that are critical to the health of Hadoop.</span></span> <span data-ttu-id="0461f-124">Az élcsomópont csak azt futtatja, amit Ön telepít rá.</span><span class="sxs-lookup"><span data-stu-id="0461f-124">The edge node runs only what you put on it.</span></span>
>
> <span data-ttu-id="0461f-125">Az élcsomópontok használatával kapcsolatban további információért lásd: [Élcsomópontok használata a HDInsightban](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span><span class="sxs-lookup"><span data-stu-id="0461f-125">For more information on using edge nodes, see [Use edge nodes in HDInsight](hdinsight-apps-use-edge-node.md#access-an-edge-node).</span></span>

## <a name="ssh-clients"></a><span data-ttu-id="0461f-126">SSH-ügyfelek</span><span class="sxs-lookup"><span data-stu-id="0461f-126">SSH clients</span></span>

<span data-ttu-id="0461f-127">Az `ssh` és `scp` parancs elérhető a Linux, Unix és macOS rendszerekben.</span><span class="sxs-lookup"><span data-stu-id="0461f-127">Linux, Unix, and macOS systems provide the `ssh` and `scp` commands.</span></span> <span data-ttu-id="0461f-128">Az `ssh`-ügyfelet általában arra használják, hogy távoli parancssori munkamenetet hozzon létre Linux vagy Unix rendszerben.</span><span class="sxs-lookup"><span data-stu-id="0461f-128">The `ssh` client is commonly used to create a remote command-line session with a Linux or Unix-based system.</span></span> <span data-ttu-id="0461f-129">Az `scp`-ügyfél segítségével biztonságosan másolhat fájlokat a saját ügyfél és a távoli rendszer között.</span><span class="sxs-lookup"><span data-stu-id="0461f-129">The `scp` client is used to securely copy files between your client and the remote system.</span></span>

<span data-ttu-id="0461f-130">A Microsoft Windows alapértelmezés szerint nem biztosít SSH-ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="0461f-130">Microsoft Windows does not provide any SSH clients by default.</span></span> <span data-ttu-id="0461f-131">Az `ssh`- és az `scp`-ügyfél az alábbi csomagokban érhető el a Windows rendszerhez:</span><span class="sxs-lookup"><span data-stu-id="0461f-131">The `ssh` and `scp` clients are available for Windows through the following packages:</span></span>

* <span data-ttu-id="0461f-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): A Cloud Shell Bash-környezetet biztosít a böngészőben, továbbá lehetővé teszi az `ssh`, az `scp`, és egyéb gyakori Linux-parancsok használatát.</span><span class="sxs-lookup"><span data-stu-id="0461f-132">[Azure Cloud Shell](../cloud-shell/quickstart.md): The Cloud Shell provides a Bash environment in your browser, and provides the `ssh`, `scp`, and other common Linux commands.</span></span>

* <span data-ttu-id="0461f-133">[Windows 10-en futó Ubuntu Bash-környezet](https://msdn.microsoft.com/commandline/wsl/about): Az `ssh` és az `scp` parancs a Windows rendszeren futó Bash parancssorából érhető el.</span><span class="sxs-lookup"><span data-stu-id="0461f-133">[Bash on Ubuntu on Windows 10](https://msdn.microsoft.com/commandline/wsl/about): The `ssh` and `scp` commands are available through the Bash on Windows command line.</span></span>

* <span data-ttu-id="0461f-134">[Git (https://git-scm.com/)](https://git-scm.com/): Az `ssh` és az `scp` parancs a GitBash parancssorából érhető el.</span><span class="sxs-lookup"><span data-stu-id="0461f-134">[Git (https://git-scm.com/)](https://git-scm.com/): The `ssh` and `scp` commands are available through the GitBash command line.</span></span>

* <span data-ttu-id="0461f-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) Az `ssh` és az `scp` parancs a GitHub Shell parancssorából érhető el.</span><span class="sxs-lookup"><span data-stu-id="0461f-135">[GitHub Desktop (https://desktop.github.com/)](https://desktop.github.com/) The `ssh` and `scp` commands are available through the GitHub Shell command line.</span></span> <span data-ttu-id="0461f-136">A GitHub Desktop konfigurálható a Bash, a Windows-parancssor vagy a PowerShell használatára a Git Shell parancssoraként.</span><span class="sxs-lookup"><span data-stu-id="0461f-136">GitHub Desktop can be configured to use Bash, the Windows Command Prompt, or PowerShell as the command line for the Git Shell.</span></span>

* <span data-ttu-id="0461f-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): A PowerShell csapata portolja az OpenSSH-t Windows rendszerre, és tesztkiadásokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="0461f-137">[OpenSSH (https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH)](https://github.com/PowerShell/Win32-OpenSSH/wiki/Install-Win32-OpenSSH): The PowerShell team is porting OpenSSH to Windows, and provides test releases.</span></span>

    > [!WARNING]
    > <span data-ttu-id="0461f-138">Az OpenSSH csomag tartalmazza az `sshd` SSH-kiszolgálóösszetevőt.</span><span class="sxs-lookup"><span data-stu-id="0461f-138">The OpenSSH package includes the SSH server component, `sshd`.</span></span> <span data-ttu-id="0461f-139">Ez az összetevő elindít egy SSH-kiszolgálót a rendszerén, így mások csatlakozhatnak ahhoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-139">This component starts an SSH server on your system, allowing others to connect to it.</span></span> <span data-ttu-id="0461f-140">Ne konfigurálja ezt az összetevőt, és ne nyissa meg a 22-es portot, ha nem szeretne SSH-kiszolgálót futtatni a rendszerén.</span><span class="sxs-lookup"><span data-stu-id="0461f-140">Do not configure this component or open port 22 unless you want to host an SSH server on your system.</span></span> <span data-ttu-id="0461f-141">Ez nem szükséges a HDInsighttal való kommunikációhoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-141">It is not required to communicate with HDInsight.</span></span>

<span data-ttu-id="0461f-142">Több grafikus SSH-ügyfél is elérhető, például a [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) és a [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span><span class="sxs-lookup"><span data-stu-id="0461f-142">There are also several graphical SSH clients, such as [PuTTY (http://www.chiark.greenend.org.uk/~sgtatham/putty/)](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and [MobaXterm (http://mobaxterm.mobatek.net/)](http://mobaxterm.mobatek.net/).</span></span> <span data-ttu-id="0461f-143">Bár ezek az ügyfelek használhatók a HDInsighthoz történő kapcsolódáshoz, a kapcsolódás folyamata más, mint az `ssh` segédprogram használatakor.</span><span class="sxs-lookup"><span data-stu-id="0461f-143">While these clients can be used to connect to HDInsight, the process of connecting is different than using the `ssh` utility.</span></span> <span data-ttu-id="0461f-144">További információt az Ön által használt grafikus ügyfél dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="0461f-144">For more information, see the documentation of the graphical client you are using.</span></span>

## <span data-ttu-id="0461f-145"><a id="sshkey"></a>Hitelesítés: SSH-kulcsok</span><span class="sxs-lookup"><span data-stu-id="0461f-145"><a id="sshkey"></a>Authentication: SSH Keys</span></span>

<span data-ttu-id="0461f-146">Az SSH-kulcsok [nyilvános kulcsú titkosítással](https://en.wikipedia.org/wiki/Public-key_cryptography) hitelesítik az SSH-munkameneteket.</span><span class="sxs-lookup"><span data-stu-id="0461f-146">SSH keys use [Public-key cryptography](https://en.wikipedia.org/wiki/Public-key_cryptography) to authenticate SSH sessions.</span></span> <span data-ttu-id="0461f-147">Az SSH-kulcsok biztonságosabbak a jelszavaknál, és egyszerű módszert kínálnak a Hadoop-fürt biztonságos elérésére.</span><span class="sxs-lookup"><span data-stu-id="0461f-147">SSH keys are more secure than passwords, and provide an easy way to secure access to your Hadoop cluster.</span></span>

<span data-ttu-id="0461f-148">Ha az SSH-fiókja kulccsal van védve, az ügyfélnek meg kell adnia az egyező titkos kulcsot, amikor csatlakozik:</span><span class="sxs-lookup"><span data-stu-id="0461f-148">If your SSH account is secured using a key, the client must provide the matching private key when you connect:</span></span>

* <span data-ttu-id="0461f-149">A legtöbb ügyfél konfigurálható __alapértelmezett kulcs__ használatára.</span><span class="sxs-lookup"><span data-stu-id="0461f-149">Most clients can be configured to use a __default key__.</span></span> <span data-ttu-id="0461f-150">Az `ssh`-ügyfél például a titkos kulcsot a `~/.ssh/id_rsa` helyen keresi Linux- és Unix-környezetekben.</span><span class="sxs-lookup"><span data-stu-id="0461f-150">For example, the `ssh` client looks for a private key at `~/.ssh/id_rsa` on Linux and Unix environments.</span></span>

* <span data-ttu-id="0461f-151">Megadhatja __egy titkos kulcs elérési útját__.</span><span class="sxs-lookup"><span data-stu-id="0461f-151">You can specify the __path to a private key__.</span></span> <span data-ttu-id="0461f-152">Az `ssh`-ügyfél esetében az `-i` paraméterrel adható meg a titkos kulcs elérési útja.</span><span class="sxs-lookup"><span data-stu-id="0461f-152">With the `ssh` client, the `-i` parameter is used to specify the path to private key.</span></span> <span data-ttu-id="0461f-153">Például: `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="0461f-153">For example, `ssh -i ~/.ssh/id_rsa sshuser@myedge.mycluster-ssh.azurehdinsight.net`.</span></span>

* <span data-ttu-id="0461f-154">Ha __több titkos kulccsal__ rendelkezik, amelyeket különböző kiszolgálókhoz használ, fontolja meg az olyan segédprogramok használatát, mint például az [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent) (ssh-ügynök).</span><span class="sxs-lookup"><span data-stu-id="0461f-154">If you have __multiple private keys__ for use with different servers, consider using a utility such as [ssh-agent (https://en.wikipedia.org/wiki/Ssh-agent)](https://en.wikipedia.org/wiki/Ssh-agent).</span></span> <span data-ttu-id="0461f-155">Az `ssh-agent` segédprogram segítségével automatikusan választhatja ki a kulcsot az SSH-munkamenet megkezdéséhez.</span><span class="sxs-lookup"><span data-stu-id="0461f-155">The `ssh-agent` utility can be used to automatically select the key to use when establishing an SSH session.</span></span>

> [!IMPORTANT]
>
> <span data-ttu-id="0461f-156">Ha jelszóval védi a titkos kulcsot, a kulcs használatakor be kell írnia a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0461f-156">If you secure your private key with a passphrase, you must enter the passphrase when using the key.</span></span> <span data-ttu-id="0461f-157">Az `ssh-agent` és hasonló segédprogramokkal a kényelmes használat érdekében gyorsítótárazhatja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0461f-157">Utilities such as `ssh-agent` can cache the password for your convenience.</span></span>

### <a name="create-an-ssh-key-pair"></a><span data-ttu-id="0461f-158">SSH-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="0461f-158">Create an SSH key pair</span></span>

<span data-ttu-id="0461f-159">Az `ssh-keygen` paranccsal hozhat létre nyilvános- és titkoskulcs-fájlokat.</span><span class="sxs-lookup"><span data-stu-id="0461f-159">Use the `ssh-keygen` command to create public and private key files.</span></span> <span data-ttu-id="0461f-160">A következő parancs egy 2048 bites RSA-kulcspárt hoz létre, amely a HDInsighttal használható:</span><span class="sxs-lookup"><span data-stu-id="0461f-160">The following command generates a 2048-bit RSA key pair that can be used with HDInsight:</span></span>

    ssh-keygen -t rsa -b 2048

<span data-ttu-id="0461f-161">A kulcs létrehozása során a rendszer információk megadását kéri.</span><span class="sxs-lookup"><span data-stu-id="0461f-161">You are prompted for information during the key creation process.</span></span> <span data-ttu-id="0461f-162">Például a kulcsok tárolási helyét kell megadnia, vagy azt, hogy használ-e jelszót.</span><span class="sxs-lookup"><span data-stu-id="0461f-162">For example, where the keys are stored or whether to use a passphrase.</span></span> <span data-ttu-id="0461f-163">A folyamat befejezése után két fájl jön létre; egy nyilvános kulcs és egy titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="0461f-163">After the process completes, two files are created; a public key and a private key.</span></span>

* <span data-ttu-id="0461f-164">A __nyilvános kulccsal__ hozható létre a HDInsight-fürt.</span><span class="sxs-lookup"><span data-stu-id="0461f-164">The __public key__ is used to create an HDInsight cluster.</span></span> <span data-ttu-id="0461f-165">A nyilvános kulcs kiterjesztése `.pub`.</span><span class="sxs-lookup"><span data-stu-id="0461f-165">The public key has an extension of `.pub`.</span></span>

* <span data-ttu-id="0461f-166">A __titkos kulccsal__ hitelesíthető az ügyfél a HDInsight-fürtön.</span><span class="sxs-lookup"><span data-stu-id="0461f-166">The __private key__ is used to authenticate your client to the HDInsight cluster.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0461f-167">A kulcsokat jelszóval védheti.</span><span class="sxs-lookup"><span data-stu-id="0461f-167">You can secure your keys using a passphrase.</span></span> <span data-ttu-id="0461f-168">Ez a jelszó lényegében egy kód a titkos kulcson.</span><span class="sxs-lookup"><span data-stu-id="0461f-168">A passphrase is effectively a password on your private key.</span></span> <span data-ttu-id="0461f-169">Ilyen esetekben még ha valaki meg is szerzi a titkos kulcsát, a jelszóra is szüksége van a kulcs használatához.</span><span class="sxs-lookup"><span data-stu-id="0461f-169">Even if someone obtains your private key, they must have the passphrase to use the key.</span></span>

### <a name="create-hdinsight-using-the-public-key"></a><span data-ttu-id="0461f-170">HDInsight létrehozása a nyilvános kulccsal</span><span class="sxs-lookup"><span data-stu-id="0461f-170">Create HDInsight using the public key</span></span>

| <span data-ttu-id="0461f-171">Létrehozási metódus</span><span class="sxs-lookup"><span data-stu-id="0461f-171">Creation method</span></span> | <span data-ttu-id="0461f-172">A nyilvános kulcs használata</span><span class="sxs-lookup"><span data-stu-id="0461f-172">How to use the public key</span></span> |
| ------- | ------- |
| <span data-ttu-id="0461f-173">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="0461f-173">**Azure portal**</span></span> | <span data-ttu-id="0461f-174">Törölje a __Használja ugyanazt a jelszót, mint a fürtbe való bejelentkezésekor__ jelölőnégyzet jelölését, majd válassza a __Nyilvános kulcs__ elemet az SSH-hitelesítés típusaként.</span><span class="sxs-lookup"><span data-stu-id="0461f-174">Uncheck __Use same password as cluster login__, and then select __Public Key__ as the SSH authentication type.</span></span> <span data-ttu-id="0461f-175">Végül válassza ki a nyilvános kulcs fájlját, vagy illessze be a fájl szöveges tartalmát a __Nyilvános SSH-kulcs__ mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0461f-175">Finally, select the public key file or paste the text contents of the file in the __SSH public key__ field.</span></span></br><span data-ttu-id="0461f-176">![Nyilvános SSH-kulcs párbeszédpanel a HDInsight-fürt létrehozásakor](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span><span class="sxs-lookup"><span data-stu-id="0461f-176">![SSH public key dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-public-key.png)</span></span> |
| <span data-ttu-id="0461f-177">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0461f-177">**Azure PowerShell**</span></span> | <span data-ttu-id="0461f-178">A `New-AzureRmHdinsightCluster` parancsmag `-SshPublicKey` paraméterével illesztheti be a nyilvános kulcs tartalmát karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="0461f-178">Use the `-SshPublicKey` parameter of the `New-AzureRmHdinsightCluster` cmdlet and pass the contents of the public key as a string.</span></span>|
| <span data-ttu-id="0461f-179">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="0461f-179">**Azure CLI 1.0**</span></span> | <span data-ttu-id="0461f-180">Az `azure hdinsight cluster create` parancs `--sshPublicKey` paraméterével illesztheti be a nyilvános kulcs tartalmát karakterláncként.</span><span class="sxs-lookup"><span data-stu-id="0461f-180">Use the `--sshPublicKey` parameter of the `azure hdinsight cluster create` command and pass the contents of the public key as a string.</span></span> |
| <span data-ttu-id="0461f-181">**Resource Manager-sablon**</span><span class="sxs-lookup"><span data-stu-id="0461f-181">**Resource Manager Template**</span></span> | <span data-ttu-id="0461f-182">Az SSH-kulcsok sablonnal történő használatának példájáért tekintse meg a [HDInsight Linux rendszeren, SSH-kulccsal való telepítéséről](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="0461f-182">For an example of using SSH keys with a template, see [Deploy HDInsight on Linux with SSH key](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-publickey/).</span></span> <span data-ttu-id="0461f-183">Az [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) fájlban lévő `publicKeys` elemmel illeszthetők be a kulcsok az Azure-ba a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0461f-183">The `publicKeys` element in the [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-publickey/azuredeploy.json) file is used to pass the keys to Azure when creating the cluster.</span></span> |

## <span data-ttu-id="0461f-184"><a id="sshpassword"></a>Hitelesítés: Jelszó</span><span class="sxs-lookup"><span data-stu-id="0461f-184"><a id="sshpassword"></a>Authentication: Password</span></span>

<span data-ttu-id="0461f-185">Az SSH-fiókok jelszóval védhetők.</span><span class="sxs-lookup"><span data-stu-id="0461f-185">SSH accounts can be secured using a password.</span></span> <span data-ttu-id="0461f-186">Amikor SSH-fiókkal csatlakozik a HDInsighthoz, a rendszer jelszót kér.</span><span class="sxs-lookup"><span data-stu-id="0461f-186">When you connect to HDInsight using SSH, you are prompted to enter the password.</span></span>

> [!WARNING]
> <span data-ttu-id="0461f-187">Nem ajánlott jelszavas hitelesítést használni az SSH-hoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-187">We do not recommend using password authentication for SSH.</span></span> <span data-ttu-id="0461f-188">A jelszavakat ki lehet találni, és védtelenek a találgatásos támadásokkal szemben.</span><span class="sxs-lookup"><span data-stu-id="0461f-188">Passwords can be guessed and are vulnerable to brute force attacks.</span></span> <span data-ttu-id="0461f-189">Ehelyett azt javasoljuk, hogy használjon [SSH-kulcsokat a hitelesítéshez](#sshkey).</span><span class="sxs-lookup"><span data-stu-id="0461f-189">Instead, we recommend that you use [SSH keys for authentication](#sshkey).</span></span>

### <a name="create-hdinsight-using-a-password"></a><span data-ttu-id="0461f-190">HDInsight létrehozása jelszóval</span><span class="sxs-lookup"><span data-stu-id="0461f-190">Create HDInsight using a password</span></span>

| <span data-ttu-id="0461f-191">Létrehozási metódus</span><span class="sxs-lookup"><span data-stu-id="0461f-191">Creation method</span></span> | <span data-ttu-id="0461f-192">Jelszó megadása</span><span class="sxs-lookup"><span data-stu-id="0461f-192">How to specify the password</span></span> |
| --------------- | ---------------- |
| <span data-ttu-id="0461f-193">**Azure Portal**</span><span class="sxs-lookup"><span data-stu-id="0461f-193">**Azure portal**</span></span> | <span data-ttu-id="0461f-194">Alapértelmezés szerint az SSH-felhasználói fióknak ugyanaz a jelszava, mint a fürt bejelentkezési fiókjának.</span><span class="sxs-lookup"><span data-stu-id="0461f-194">By default, the SSH user account has the same password as the cluster login account.</span></span> <span data-ttu-id="0461f-195">Ha más jelszót szeretne használni, törölje a __Használja ugyanazt a jelszót, mint a fürtbe való bejelentkezésekor__ jelölőnégyzet jelölését, majd írja be a jelszót az __SSH-jelszó__ mezőbe.</span><span class="sxs-lookup"><span data-stu-id="0461f-195">To use a different password, uncheck __Use same password as cluster login__, and then enter the password in the __SSH password__ field.</span></span></br><span data-ttu-id="0461f-196">![SSH-jelszó párbeszédpanel a HDInsight-fürt létrehozásakor](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span><span class="sxs-lookup"><span data-stu-id="0461f-196">![SSH password dialog in HDInsight cluster creation](./media/hdinsight-hadoop-linux-use-ssh-unix/create-hdinsight-ssh-password.png)</span></span>|
| <span data-ttu-id="0461f-197">**Azure PowerShell**</span><span class="sxs-lookup"><span data-stu-id="0461f-197">**Azure PowerShell**</span></span> | <span data-ttu-id="0461f-198">A `New-AzureRmHdinsightCluster` parancsmag `--SshCredential` paraméterével illessze be az SSH-felhasználói fiók nevét és jelszavát tartalmazó `PSCredential` objektumot.</span><span class="sxs-lookup"><span data-stu-id="0461f-198">Use the `--SshCredential` parameter of the `New-AzureRmHdinsightCluster` cmdlet and pass a `PSCredential` object that contains the SSH user account name and password.</span></span> |
| <span data-ttu-id="0461f-199">**Azure CLI 1.0**</span><span class="sxs-lookup"><span data-stu-id="0461f-199">**Azure CLI 1.0**</span></span> | <span data-ttu-id="0461f-200">Az `azure hdinsight cluster create` parancs `--sshPassword` paraméterével adja meg a jelszó értékét.</span><span class="sxs-lookup"><span data-stu-id="0461f-200">Use the `--sshPassword` parameter of the `azure hdinsight cluster create` command and provide the password value.</span></span> |
| <span data-ttu-id="0461f-201">**Resource Manager-sablon**</span><span class="sxs-lookup"><span data-stu-id="0461f-201">**Resource Manager Template**</span></span> | <span data-ttu-id="0461f-202">A jelszavak sablonnal történő használatának példájáért tekintse meg a [HDInsight Linux rendszeren, SSH-kulccsal való telepítéséről](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/) szóló témakört.</span><span class="sxs-lookup"><span data-stu-id="0461f-202">For an example of using a password with a template, see [Deploy HDInsight on Linux with SSH password](https://azure.microsoft.com/resources/templates/101-hdinsight-linux-ssh-password/).</span></span> <span data-ttu-id="0461f-203">Az [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) fájlban lévő `linuxOperatingSystemProfile` elemmel illeszthető be az SSH-fióknév és -jelszó az Azure-ba a fürt létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0461f-203">The `linuxOperatingSystemProfile` element in the [azuredeploy.json](https://github.com/Azure/azure-quickstart-templates/blob/master/101-hdinsight-linux-ssh-password/azuredeploy.json) file is used to pass the SSH account name and password to Azure when creating the cluster.</span></span>|

### <a name="change-the-ssh-password"></a><span data-ttu-id="0461f-204">Az SSH-jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="0461f-204">Change the SSH password</span></span>

<span data-ttu-id="0461f-205">Az SSH-felhasználói fiók jelszavának módosításával kapcsolatos információért tekintse meg a [HDInsight kezelése](hdinsight-administer-use-portal-linux.md#change-passwords) dokumentum __Jelszavak módosítása__ szakaszát.</span><span class="sxs-lookup"><span data-stu-id="0461f-205">For information on changing the SSH user account password, see the __Change passwords__ section of the [Manage HDInsight](hdinsight-administer-use-portal-linux.md#change-passwords) document.</span></span>

## <span data-ttu-id="0461f-206"><a id="domainjoined"></a>Hitelesítés: Tartományhoz csatlakoztatott HDInsight</span><span class="sxs-lookup"><span data-stu-id="0461f-206"><a id="domainjoined"></a>Authentication: Domain-joined HDInsight</span></span>

<span data-ttu-id="0461f-207">Ha __tartományhoz csatlakoztatott HDInsight-fürtöt__ használ, a `kinit` parancsot kell használnia az SSH-hoz való kapcsolódás után.</span><span class="sxs-lookup"><span data-stu-id="0461f-207">If you are using a __domain-joined HDInsight cluster__, you must use the `kinit` command after connecting with SSH.</span></span> <span data-ttu-id="0461f-208">Ez a parancs tartományi felhasználónevet és jelszót kér, és hitelesíti a munkamenetet a fürttel társított Azure Active Directory-tartománnyal.</span><span class="sxs-lookup"><span data-stu-id="0461f-208">This command prompts you for a domain user and password, and authenticates your session with the Azure Active Directory domain associated with the cluster.</span></span>

<span data-ttu-id="0461f-209">További információkat itt talál: [Tartományhoz csatlakoztatott HDInsight konfigurálása](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="0461f-209">For more information, see [Configure domain-joined HDInsight](hdinsight-domain-joined-configure.md).</span></span>

## <a name="connect-to-nodes"></a><span data-ttu-id="0461f-210">Csatlakozás csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="0461f-210">Connect to nodes</span></span>

<span data-ttu-id="0461f-211">Az átjárócsomópontokhoz és az élcsomóponthoz (ha van) az interneten, a 22-es és a 23-as porton lehet hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="0461f-211">The head nodes and edge node (if there is one) can be accessed over the internet on ports 22 and 23.</span></span>

* <span data-ttu-id="0461f-212">Az __átjárócsomópontokhoz__ való csatlakozás során az elsődleges átjárócsomóponthoz való csatlakozáshoz használja a __22-es__ portot, a másodlagos átjárócsomóponthoz való csatlakozáshoz pedig a __23-as__ portot.</span><span class="sxs-lookup"><span data-stu-id="0461f-212">When connecting to the __head nodes__, use port __22__ to connect to the primary head node and port __23__ to connect to the secondary head node.</span></span> <span data-ttu-id="0461f-213">A használandó teljes tartománynév a `clustername-ssh.azurehdinsight.net`, ahol a `clustername` a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="0461f-213">The fully qualified domain name to use is `clustername-ssh.azurehdinsight.net`, where `clustername` is the name of your cluster.</span></span>

    ```bash
    # Connect to primary head node
    # port not specified since 22 is the default
    ssh sshuser@clustername-ssh.azurehdinsight.net

    # Connect to secondary head node
    ssh -p 23 sshuser@clustername-ssh.azurehdinsight.net
    ```
    
* <span data-ttu-id="0461f-214">Az __élcsomóponthoz__ való csatlakozáskor használja a 22-es portot.</span><span class="sxs-lookup"><span data-stu-id="0461f-214">When connectiung to the __edge node__, use port 22.</span></span> <span data-ttu-id="0461f-215">A teljes tartománynév az `edgenodename.clustername-ssh.azurehdinsight.net`, ahol az `edgenodename` az élcsomópont létrehozásakor megadott név.</span><span class="sxs-lookup"><span data-stu-id="0461f-215">The fully qualified domain name is `edgenodename.clustername-ssh.azurehdinsight.net`, where `edgenodename` is a name you provided when creating the edge node.</span></span> <span data-ttu-id="0461f-216">A `clustername` a fürt neve.</span><span class="sxs-lookup"><span data-stu-id="0461f-216">`clustername` is the name of the cluster.</span></span>

    ```bash
    # Connect to edge node
    ssh sshuser@edgnodename.clustername-ssh.azurehdinsight.net
    ```

> [!IMPORTANT]
> <span data-ttu-id="0461f-217">Az előző példák azt feltételezik, hogy jelszavas hitelesítést használ, vagy automatikus tanúsítványalapú hitelesítés történik.</span><span class="sxs-lookup"><span data-stu-id="0461f-217">The previous examples assume that you are using password authentication, or that certificate authentication is occuring automatically.</span></span> <span data-ttu-id="0461f-218">Ha SSH-kulcspárt használ a hitelesítéshez, és a tanúsítvány használata nem automatikus, az `-i` paraméterrel adja meg a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0461f-218">If you use an SSH key-pair for authentication, and the certificate is not used automatically, use the `-i` parameter to specify the private key.</span></span> <span data-ttu-id="0461f-219">Például: `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span><span class="sxs-lookup"><span data-stu-id="0461f-219">For example, `ssh -i ~/.ssh/mykey sshuser@clustername-ssh.azurehdinsight.net`.</span></span>

<span data-ttu-id="0461f-220">A csatlakozás után a parancssor megváltozik, és megjeleníti az SSH-felhasználónevet és a csomópontot, amelyhez csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="0461f-220">Once connected, the prompt changes to indicate the SSH user name and the node you are connected to.</span></span> <span data-ttu-id="0461f-221">Ha például az `sshuser` felhasználóként csatlakozik az elsődleges átjárócsomóponthoz, a parancssor az `sshuser@hn0-clustername:~$`.</span><span class="sxs-lookup"><span data-stu-id="0461f-221">For example, when connected to the primary head node as `sshuser`, the prompt is `sshuser@hn0-clustername:~$`.</span></span>

### <a name="connect-to-worker-and-zookeeper-nodes"></a><span data-ttu-id="0461f-222">Csatlakozás a feldolgozó és Zookeeper-csomópontokhoz</span><span class="sxs-lookup"><span data-stu-id="0461f-222">Connect to worker and Zookeeper nodes</span></span>

<span data-ttu-id="0461f-223">A feldolgozó és Zookeeper-csomópontok nem közvetlenül az internetről,</span><span class="sxs-lookup"><span data-stu-id="0461f-223">The worker nodes and Zookeeper nodes are not directly accessible from the internet.</span></span> <span data-ttu-id="0461f-224">hanem a fürt átjáró- vagy élcsomópontjain keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="0461f-224">They can be accessed from the cluster head nodes or edge nodes.</span></span> <span data-ttu-id="0461f-225">A következő általános lépésekkel csatlakozhat más csomópontokhoz:</span><span class="sxs-lookup"><span data-stu-id="0461f-225">The following are the general steps to connect to other nodes:</span></span>

1. <span data-ttu-id="0461f-226">Az SSH-val csatlakozzon egy átjáró- vagy élcsomóponthoz:</span><span class="sxs-lookup"><span data-stu-id="0461f-226">Use SSH to connect to a head or edge node:</span></span>

        ssh sshuser@myedge.mycluster-ssh.azurehdinsight.net

2. <span data-ttu-id="0461f-227">Az átjáró- vagy élcsomópont felé fennálló SSH-kapcsolatból használja az `ssh` parancsot a fürt egyik munkavégző csomópontjához való csatlakozáshoz:</span><span class="sxs-lookup"><span data-stu-id="0461f-227">From the SSH connection to the head or edge node, use the `ssh` command to connect to a worker node in the cluster:</span></span>

        ssh sshuser@wn0-myhdi

    <span data-ttu-id="0461f-228">A fürtben található csomópontok tartománynévlistájának lekéréséhez lásd: [A HDInsight kezelése az Ambari REST API használatával](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes).</span><span class="sxs-lookup"><span data-stu-id="0461f-228">To retrieve a list of the domain names of the nodes in the cluster, see the [Manage HDInsight by using the Ambari REST API](hdinsight-hadoop-manage-ambari-rest-api.md#example-get-the-fqdn-of-cluster-nodes) document.</span></span>

<span data-ttu-id="0461f-229">Ha az SSH-fiókot __jelszó__ védi, a kapcsolódáshoz adja meg a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0461f-229">If the SSH account is secured using a __password__, enter the password when connecting.</span></span>

<span data-ttu-id="0461f-230">Ha az SSH-fiókot __SSH-kulcsok__ védik, győződjön meg róla, hogy az SSH-továbbítás engedélyezve van az ügyfélen.</span><span class="sxs-lookup"><span data-stu-id="0461f-230">If the SSH account is secured using __SSH keys__, make sure that SSH forwarding is enabled on the client.</span></span>

> [!NOTE]
> <span data-ttu-id="0461f-231">A fürtben lévő összes csomópont közvetlen elérésének másik módja, ha a HDInsightot Azure virtuális hálózatra telepíti.</span><span class="sxs-lookup"><span data-stu-id="0461f-231">Another way to directly access all nodes in the cluster is to install HDInsight into an Azure Virtual Network.</span></span> <span data-ttu-id="0461f-232">Ezután csatlakoztathatja a távoli gépet ugyanehhez a virtuális hálózathoz, és közvetlenül érheti el a fürtben lévő összes csomópontot.</span><span class="sxs-lookup"><span data-stu-id="0461f-232">Then, you can join your remote machine to the same virtual network and directly access all nodes in the cluster.</span></span>
>
> <span data-ttu-id="0461f-233">További információ: [Virtuális hálózat használata HDInsighttal](hdinsight-extend-hadoop-virtual-network.md).</span><span class="sxs-lookup"><span data-stu-id="0461f-233">For more information, see [Use a virtual network with HDInsight](hdinsight-extend-hadoop-virtual-network.md).</span></span>

### <a name="configure-ssh-agent-forwarding"></a><span data-ttu-id="0461f-234">SSH-ügynöktovábbítás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0461f-234">Configure SSH agent forwarding</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0461f-235">A következő lépések Linux vagy UNIX-alapú rendszert feltételeznek, és a Windows 10-en futó Bash-környezet esetén működnek.</span><span class="sxs-lookup"><span data-stu-id="0461f-235">The following steps assume a Linux or UNIX-based system, and work with Bash on Windows 10.</span></span> <span data-ttu-id="0461f-236">Ha a lépések nem működnek a rendszerén, lehet, hogy át kell tekintenie az SSH-ügyfél dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="0461f-236">If these steps do not work for your system, you may need to consult the documentation for your SSH client.</span></span>

1. <span data-ttu-id="0461f-237">Egy szövegszerkesztővel nyissa meg a `~/.ssh/config` fájlt.</span><span class="sxs-lookup"><span data-stu-id="0461f-237">Using a text editor, open `~/.ssh/config`.</span></span> <span data-ttu-id="0461f-238">Ha a fájl nem létezik, létrehozhatja a parancssoron az `touch ~/.ssh/config` karakterlánc beírásával.</span><span class="sxs-lookup"><span data-stu-id="0461f-238">If this file doesn't exist, you can create it by entering `touch ~/.ssh/config` at a command line.</span></span>

2. <span data-ttu-id="0461f-239">Adja hozzá a következő szöveget az `config` fájlhoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-239">Add the following text to the `config` file.</span></span>

        Host <edgenodename>.<clustername>-ssh.azurehdinsight.net
          ForwardAgent yes

    <span data-ttu-id="0461f-240">Cserélje le a __Gazda__ információit azon csomópont címére, amelyet SSH-val csatlakoztat.</span><span class="sxs-lookup"><span data-stu-id="0461f-240">Replace the __Host__ information with the address of the node you connect to using SSH.</span></span> <span data-ttu-id="0461f-241">Az előző példa az élcsomópontot használja.</span><span class="sxs-lookup"><span data-stu-id="0461f-241">The previous example uses the edge node.</span></span> <span data-ttu-id="0461f-242">Ez a bejegyzés konfigurálja az SSH-ügynöktovábbítást az adott csomópont számára.</span><span class="sxs-lookup"><span data-stu-id="0461f-242">This entry configures SSH agent forwarding for the specified node.</span></span>

3. <span data-ttu-id="0461f-243">Tesztelje az SSH-ügynöktovábbítást a terminálból a következő parancs segítségével:</span><span class="sxs-lookup"><span data-stu-id="0461f-243">Test SSH agent forwarding by using the following command from the terminal:</span></span>

        echo "$SSH_AUTH_SOCK"

    <span data-ttu-id="0461f-244">Ez a parancs az alábbi szöveghez hasonló információt ad vissza:</span><span class="sxs-lookup"><span data-stu-id="0461f-244">This command returns information similar to the following text:</span></span>

        /tmp/ssh-rfSUL1ldCldQ/agent.1792

    <span data-ttu-id="0461f-245">Ha a parancs nem ad vissza semmit, a(z) `ssh-agent` nem fut.</span><span class="sxs-lookup"><span data-stu-id="0461f-245">If nothing is returned, then `ssh-agent` is not running.</span></span> <span data-ttu-id="0461f-246">További tudnivalókért lásd az ügynök indítási szkriptjeire vonatkozó részt a [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) (Az ssh-agent és az ssh együttes használata) című cikkben, vagy az SSH-ügyfél dokumentációját.</span><span class="sxs-lookup"><span data-stu-id="0461f-246">For more information, see the agent startup scripts information at [Using ssh-agent with ssh (http://mah.everybody.org/docs/ssh)](http://mah.everybody.org/docs/ssh) or consult your SSH client documentation.</span></span>

4. <span data-ttu-id="0461f-247">Ha meggyőződött róla, hogy az **ssh-agent** fut, a következő segítségével adja hozzá a titkos SSH-kulcsot az ügynökhöz:</span><span class="sxs-lookup"><span data-stu-id="0461f-247">Once you have verified that **ssh-agent** is running, use the following to add your SSH private key to the agent:</span></span>

        ssh-add ~/.ssh/id_rsa

    <span data-ttu-id="0461f-248">Ha a titkos kulcsot egy másik fájl tárolja, a `~/.ssh/id_rsa` részt cserélje ki a fájl elérési útjára.</span><span class="sxs-lookup"><span data-stu-id="0461f-248">If your private key is stored in a different file, replace `~/.ssh/id_rsa` with the path to the file.</span></span>

5. <span data-ttu-id="0461f-249">Csatlakozzon a fürt élcsomópontjához vagy átjárócsomópontjaihoz SSH-val.</span><span class="sxs-lookup"><span data-stu-id="0461f-249">Connect to the cluster edge node or head nodes using SSH.</span></span> <span data-ttu-id="0461f-250">Ezután az SSH-paranccsal csatlakozzon egy feldolgozó vagy Zookeeper-csomóponthoz.</span><span class="sxs-lookup"><span data-stu-id="0461f-250">Then use the SSH command to connect to a worker or zookeeper node.</span></span> <span data-ttu-id="0461f-251">A kapcsolat létrejön a továbbított kulccsal.</span><span class="sxs-lookup"><span data-stu-id="0461f-251">The connection is established using the forwarded key.</span></span>

## <a name="copy-files"></a><span data-ttu-id="0461f-252">Fájlok másolása</span><span class="sxs-lookup"><span data-stu-id="0461f-252">Copy files</span></span>

<span data-ttu-id="0461f-253">Az `scp` segédprogrammal fájlokat másolhat a fürt egyes csomópontjairól más csomópontokra.</span><span class="sxs-lookup"><span data-stu-id="0461f-253">The `scp` utility can be used to copy files to and from individual nodes in the cluster.</span></span> <span data-ttu-id="0461f-254">A következő paranccsal például a `test.txt` könyvtárat a helyi rendszerről az elsődleges átjárócsomópontra másolhatja:</span><span class="sxs-lookup"><span data-stu-id="0461f-254">For example, the following command copies the `test.txt` directory from the local system to the primary head node:</span></span>

```bash
scp test.txt sshuser@clustername-ssh.azurehdinsight.net:
```

<span data-ttu-id="0461f-255">Mivel nincs megadva elérési út a `:` után, a fájl az `sshuser` kezdőkönyvtárba kerül.</span><span class="sxs-lookup"><span data-stu-id="0461f-255">Since no path is specified after the `:`, the file is placed in the `sshuser` home directory.</span></span>

<span data-ttu-id="0461f-256">A következő parancs a `test.txt` fájlt az elsődleges átjárócsomóponton található `sshuser` kezdőkönyvtárból a helyi rendszerre másolja:</span><span class="sxs-lookup"><span data-stu-id="0461f-256">The following example copies the `test.txt` file from the `sshuser` home directory on the primary head node to the local system:</span></span>

```bash
scp sshuser@clustername-ssh.azurehdinsight.net:test.txt .
```

> [!IMPORTANT]
> <span data-ttu-id="0461f-257">Az `scp` csak a fürt egyes csomópontjainak fájlrendszeréhez képes hozzáférni.</span><span class="sxs-lookup"><span data-stu-id="0461f-257">`scp` can only access the file system of individual nodes within the cluster.</span></span> <span data-ttu-id="0461f-258">Nem használható a fürt HDFS-kompatibilis tárolójában tárolt adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="0461f-258">It cannot be used to access data in the HDFS-compatible storage for the cluster.</span></span>
>
> <span data-ttu-id="0461f-259">Ha egy SSH-munkamenetből kíván feltölteni egy használni kívánt erőforrást, használja az `scp` segédprogramot.</span><span class="sxs-lookup"><span data-stu-id="0461f-259">Use `scp` when you need to upload a resource for use from an SSH session.</span></span> <span data-ttu-id="0461f-260">Például töltsön fel egy Python-szkriptet, majd futtassa a szkriptet egy SSH-munkamenetből.</span><span class="sxs-lookup"><span data-stu-id="0461f-260">For example, upload a Python script and then run the script from an SSH session.</span></span>
>
> <span data-ttu-id="0461f-261">Adatok közvetlenül a HDFS-kompatibilis tárolóba való betöltésével kapcsolatos információkat a következő dokumentumokban talál:</span><span class="sxs-lookup"><span data-stu-id="0461f-261">For information on directly loading data into the HDFS-compatible storage, see the following documents:</span></span>
>
> * <span data-ttu-id="0461f-262">[Az Azure Storage-et használó HDInsight](hdinsight-hadoop-use-blob-storage.md).</span><span class="sxs-lookup"><span data-stu-id="0461f-262">[HDInsight using Azure Storage](hdinsight-hadoop-use-blob-storage.md).</span></span>
>
> * <span data-ttu-id="0461f-263">[Az Azure Data Lake Store-t használó HDInsight](hdinsight-hadoop-use-data-lake-store.md).</span><span class="sxs-lookup"><span data-stu-id="0461f-263">[HDInsight using Azure Data Lake Store](hdinsight-hadoop-use-data-lake-store.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0461f-264">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0461f-264">Next steps</span></span>

* [<span data-ttu-id="0461f-265">SSH-alagútkezelés használata a HDInsighttal</span><span class="sxs-lookup"><span data-stu-id="0461f-265">Use SSH tunneling with HDInsight</span></span>](hdinsight-linux-ambari-ssh-tunnel.md)
* [<span data-ttu-id="0461f-266">Virtuális hálózat használata a HDInsighttal</span><span class="sxs-lookup"><span data-stu-id="0461f-266">Use a virtual network with HDInsight</span></span>](hdinsight-extend-hadoop-virtual-network.md)
* [<span data-ttu-id="0461f-267">Élcsomópontok használata a HDInsightban</span><span class="sxs-lookup"><span data-stu-id="0461f-267">Use edge nodes in HDInsight</span></span>](hdinsight-apps-use-edge-node.md#access-an-edge-node)