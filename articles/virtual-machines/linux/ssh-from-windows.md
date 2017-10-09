---
title: "a Windows, Linux virtuális gépek kulcsok aaaUse SSH |} Microsoft Docs"
description: "Ismerje meg, hogyan toogenerate és -felhasználási SSH kulcsok Windows számítógép tooconnect tooa Linux virtuális gépre az Azure-on."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 2cacda3b-7949-4036-bd5d-837e8b09a9c8
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/08/2017
ms.author: danlep
ms.openlocfilehash: 6c44217332538857cc2ca2e85de4b476aa71251c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-ssh-keys-with-windows-on-azure"></a><span data-ttu-id="95d9b-103">Hogyan tooUse SSH-kulcsok a Windows Azure</span><span class="sxs-lookup"><span data-stu-id="95d9b-103">How tooUse SSH keys with Windows on Azure</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="95d9b-104">Windows</span><span class="sxs-lookup"><span data-stu-id="95d9b-104">Windows</span></span>](ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
> * [<span data-ttu-id="95d9b-105">Linux/Mac</span><span class="sxs-lookup"><span data-stu-id="95d9b-105">Linux/Mac</span></span>](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
>
>

<span data-ttu-id="95d9b-106">TooLinux virtuális gépek (VM) az Azure-ban kapcsolódáskor használjon [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide egy biztonságosabb módszer toolog tooyour Linux virtuális gép található.</span><span class="sxs-lookup"><span data-stu-id="95d9b-106">When you connect tooLinux virtual machines (VMs) in Azure, you should use [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography) tooprovide a more secure way toolog in tooyour Linux VM.</span></span> <span data-ttu-id="95d9b-107">A folyamat során a nyilvános és titkos kulcs az exchange rendszer hello secure shell (SSH) parancs tooauthenticate saját kezűleg ahelyett, hogy a felhasználónév és jelszó.</span><span class="sxs-lookup"><span data-stu-id="95d9b-107">This process involves a public and private key exchange using hello secure shell (SSH) command tooauthenticate yourself rather than a username and password.</span></span> <span data-ttu-id="95d9b-108">A jelszóban sebezhető toobrute-módszerén alapuló támadások, különösen az internetre irányuló virtuális gépeken futó például.</span><span class="sxs-lookup"><span data-stu-id="95d9b-108">Passwords are vulnerable toobrute-force attacks, especially on Internet-facing VMs such as web servers.</span></span> <span data-ttu-id="95d9b-109">Ez a cikk áttekintést nyújt az SSH-kulcsokat, és hogyan toogenerate hello Windows rendszerű számítógépeken a megfelelő kulcsokkal.</span><span class="sxs-lookup"><span data-stu-id="95d9b-109">This article provides an overview of SSH keys and how toogenerate hello appropriate keys on a Windows computer.</span></span>

## <a name="overview-of-ssh-and-keys"></a><span data-ttu-id="95d9b-110">SSH és a kulcsok – áttekintés</span><span class="sxs-lookup"><span data-stu-id="95d9b-110">Overview of SSH and keys</span></span>
<span data-ttu-id="95d9b-111">Biztonságosan bejelentkezhet tooyour Linux virtuális gép nyilvános és titkos kulcsok használatával:</span><span class="sxs-lookup"><span data-stu-id="95d9b-111">You can securely log in tooyour Linux VM by using public and private keys:</span></span>

* <span data-ttu-id="95d9b-112">Hello **nyilvános kulcs** a Linux virtuális Gépet, vagy bármely más, hogy kívánja-e a nyilvános kulcsú titkosítás toouse szolgáltatás el van helyezve.</span><span class="sxs-lookup"><span data-stu-id="95d9b-112">hello **public key** is placed on your Linux VM, or any other service that you wish toouse with public-key cryptography.</span></span>
* <span data-ttu-id="95d9b-113">Hello **titkos kulcs** esetén milyen meg jelenlegi tooyour Linux virtuális gép jelentkezik be, tooverify a személyazonosságát.</span><span class="sxs-lookup"><span data-stu-id="95d9b-113">hello **private key** is what you present tooyour Linux VM when you log in, tooverify your identity.</span></span> <span data-ttu-id="95d9b-114">Védje a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="95d9b-114">Protect this private key.</span></span> <span data-ttu-id="95d9b-115">Ne ossza meg senkivel.</span><span class="sxs-lookup"><span data-stu-id="95d9b-115">Do not share it.</span></span>

<span data-ttu-id="95d9b-116">A nyilvános és titkos kulcsokat több virtuális gépek és szolgáltatások is használható.</span><span class="sxs-lookup"><span data-stu-id="95d9b-116">These public and private keys can be used on multiple VMs and services.</span></span> <span data-ttu-id="95d9b-117">Nem kell minden virtuális gép vagy szolgáltatás tooaccess kívánja egy kulcspárra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="95d9b-117">You do not need a pair of keys for each VM or service you wish tooaccess.</span></span> <span data-ttu-id="95d9b-118">Részletesebb áttekintéséért lásd: [nyilvános kulcsú](https://wikipedia.org/wiki/Public-key_cryptography).</span><span class="sxs-lookup"><span data-stu-id="95d9b-118">For a more detailed overview, see [public-key cryptography](https://wikipedia.org/wiki/Public-key_cryptography).</span></span>

<span data-ttu-id="95d9b-119">Az SSH egy olyan titkosított kapcsolati protokoll, amely lehetővé teszi, hogy a biztonságos bejelentkezés titkosítatlan kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="95d9b-119">SSH is an encrypted connection protocol that allows secure logins over unsecured connections.</span></span> <span data-ttu-id="95d9b-120">Hello alapértelmezett csatlakozási protokoll Azure szolgáltatásban üzemeltetett Linux virtuális gépek esetén is.</span><span class="sxs-lookup"><span data-stu-id="95d9b-120">It is hello default connection protocol for Linux VMs hosted in Azure.</span></span> <span data-ttu-id="95d9b-121">Habár maga SSH titkosított kapcsolatot biztosít, jelszavak használata SSH-kapcsolatok továbbra is hagyja, hello VM sebezhető toobrute-módszerén alapuló támadások vagy a jelszavak találgatás.</span><span class="sxs-lookup"><span data-stu-id="95d9b-121">Although SSH itself provides an encrypted connection, using passwords with SSH connections still leaves hello VM vulnerable toobrute-force attacks or guessing of passwords.</span></span> <span data-ttu-id="95d9b-122">A biztonságosabb és előnyben részesített csatlakozó tooa SSH használó virtuális gépek módszer a nyilvános és titkos kulcsok, más néven az SSH-kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="95d9b-122">A more secure and preferred method of connecting tooa VM using SSH is by using these public and private keys, also known as SSH keys.</span></span>

<span data-ttu-id="95d9b-123">Ha nem kíván toouse SSH-kulcsok, továbbra is bejelentkezhet tooyour Linux virtuális gépek jelszó használatával.</span><span class="sxs-lookup"><span data-stu-id="95d9b-123">If you do not wish toouse SSH keys, you can still log in tooyour Linux VMs using a password.</span></span> <span data-ttu-id="95d9b-124">Ha a virtuális gép nem kitett toohello Internet, elegendő jelszóval lehet.</span><span class="sxs-lookup"><span data-stu-id="95d9b-124">If your VM is not exposed toohello Internet, using passwords may be sufficient.</span></span> <span data-ttu-id="95d9b-125">Azonban továbbra is a jelszavak toomanage kell az egyes Linux virtuális gépek és karbantartása a megfelelő jelszót házirendeket és gyakorlatokat, például a jelszó minimális hossza, és rendszeresen frissíteni azokat.</span><span class="sxs-lookup"><span data-stu-id="95d9b-125">However, you still need toomanage your passwords for each Linux VM and maintain healthy password policies and practices, such as minimum password length and regularly updating them.</span></span> <span data-ttu-id="95d9b-126">SSH-kulcsok hello használata csökkenti a hello összetettsége egyéni hitelesítő adatok kezelése több virtuális gépek között.</span><span class="sxs-lookup"><span data-stu-id="95d9b-126">hello use of SSH keys reduces hello complexity of managing individual credentials across multiple VMs.</span></span>

## <a name="windows-packages-and-ssh-clients"></a><span data-ttu-id="95d9b-127">Windows-csomagok és az SSH-ügyfél</span><span class="sxs-lookup"><span data-stu-id="95d9b-127">Windows packages and SSH clients</span></span>
<span data-ttu-id="95d9b-128">Csatlakozás tooand Linux virtuális gépek kezelése az Azure használatával egy **SSH-ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="95d9b-128">You connect tooand manage Linux VMs in Azure using an **SSH client**.</span></span> <span data-ttu-id="95d9b-129">Windows rendszerű számítógépek általában nincs egy SSH-ügyfél telepítve.</span><span class="sxs-lookup"><span data-stu-id="95d9b-129">Windows computers do not typically have an SSH client installed.</span></span> <span data-ttu-id="95d9b-130">Windows 10 évforduló frissítés hello hozzáadott Bash a Windows, és hello Windows 10 Creators legfrissebb nyújt további frissítések.</span><span class="sxs-lookup"><span data-stu-id="95d9b-130">hello Windows 10 Anniversary Update added Bash for Windows, and hello latest Windows 10 Creators Update provides additional updates.</span></span> <span data-ttu-id="95d9b-131">A Linux Windows alrendszere lehetővé teszi például egy SSH-ügyfél alapértelmezés szerint a rendszerhéjakba belül toorun és hozzáférési segédprogramok.</span><span class="sxs-lookup"><span data-stu-id="95d9b-131">This Windows Subsystem for Linux allows you toorun and access utilities such as an SSH client natively within a Bash shell.</span></span> <span data-ttu-id="95d9b-132">Kövesse a hello Linux dokumentumok, például a [hogyan toogenerate SSH-kulcsot a Linux párokat](mac-create-ssh-keys.md).</span><span class="sxs-lookup"><span data-stu-id="95d9b-132">You can then follow any of hello Linux docs, such as [How toogenerate SSH key pairs for Linux](mac-create-ssh-keys.md).</span></span> <span data-ttu-id="95d9b-133">Bash Windows még fejlesztés alatt áll, ezért a bétaverzió tekinthető.</span><span class="sxs-lookup"><span data-stu-id="95d9b-133">Bash for Windows is still under development, and is considered a beta release.</span></span> <span data-ttu-id="95d9b-134">A Windows Bash kapcsolatos további információkért lásd: [Windows Ubuntu a Bash](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="95d9b-134">For more information about Bash for Windows, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="95d9b-135">Ha a toouse valami Bash a Windowstól eltérő, közös Windows SSH ügyfelek telepítése hello csomagok a következő tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="95d9b-135">If you wish toouse something other than Bash for Windows, common Windows SSH clients you can install are included in hello following packages:</span></span>

* [<span data-ttu-id="95d9b-136">A Windows Git</span><span class="sxs-lookup"><span data-stu-id="95d9b-136">Git For Windows</span></span>](https://git-for-windows.github.io/)
* [<span data-ttu-id="95d9b-137">a puTTY</span><span class="sxs-lookup"><span data-stu-id="95d9b-137">puTTY</span></span>](http://www.chiark.greenend.org.uk/~sgtatham/putty/)
* [<span data-ttu-id="95d9b-138">MobaXterm</span><span class="sxs-lookup"><span data-stu-id="95d9b-138">MobaXterm</span></span>](http://mobaxterm.mobatek.net/)
* [<span data-ttu-id="95d9b-139">Cygwin</span><span class="sxs-lookup"><span data-stu-id="95d9b-139">Cygwin</span></span>](https://cygwin.com/)


## <a name="which-key-files-do-you-need-toocreate"></a><span data-ttu-id="95d9b-140">Amely kulcs fájlok toocreate van szüksége?</span><span class="sxs-lookup"><span data-stu-id="95d9b-140">Which key files do you need toocreate?</span></span>
<span data-ttu-id="95d9b-141">Az Azure-nak, legalább 2048 bites **ssh-rsa** formátumú nyilvános és titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="95d9b-141">Azure requires at least 2048-bit, **ssh-rsa** formatted public and private keys.</span></span> <span data-ttu-id="95d9b-142">Azure-erőforrások hello klasszikus telepítési modell használatával felügyeli, is szükség van egy PEM toogenerate (`.pem` fájl).</span><span class="sxs-lookup"><span data-stu-id="95d9b-142">If you are managing Azure resources using hello Classic deployment model, you also need toogenerate a PEM (`.pem` file).</span></span>

<span data-ttu-id="95d9b-143">Az alábbiakban hello telepítési helyzetekben, és minden használni kívánt hello típusú:</span><span class="sxs-lookup"><span data-stu-id="95d9b-143">Here are hello deployment scenarios, and hello types of files you use in each:</span></span>

1. <span data-ttu-id="95d9b-144">**ssh-rsa** hello segítségével minden telepítésében szükség [Azure-portálon](https://portal.azure.com), és a Resource Manager üzembe helyezése hello [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="95d9b-144">**ssh-rsa** keys are required for any deployment using hello [Azure portal](https://portal.azure.com), and Resource Manager deployments using hello [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="95d9b-145">Ezek a kulcsok rendszerint minden legtöbbek számára szükséges.</span><span class="sxs-lookup"><span data-stu-id="95d9b-145">These keys are usually all most people need.</span></span>
2. <span data-ttu-id="95d9b-146">A `.pem` fájl szükséges toocreate virtuális gépek hello klasszikus központi telepítéssel.</span><span class="sxs-lookup"><span data-stu-id="95d9b-146">A `.pem` file is required toocreate VMs using hello Classic deployment.</span></span> <span data-ttu-id="95d9b-147">Ezek a kulcsok hello használata esetén támogatottak klasszikus telepítések [Azure-portálon](https://portal.azure.com) vagy [Azure CLI](../../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="95d9b-147">These keys are supported in Classic deployments when using hello [Azure portal](https://portal.azure.com) or [Azure CLI](../../cli-install-nodejs.md).</span></span>
   * <span data-ttu-id="95d9b-148">Csak akkor kell toocreate ezeket a további kulcsoknak és tanúsítványoknak hello klasszikus telepítési modellel készült erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="95d9b-148">You only need toocreate these additional keys and certificates if you are managing resources created using hello Classic deployment model.</span></span>

## <a name="install-git-for-windows"></a><span data-ttu-id="95d9b-149">Git for Windows telepítése</span><span class="sxs-lookup"><span data-stu-id="95d9b-149">Install Git for Windows</span></span>
<span data-ttu-id="95d9b-150">hello előző szakaszban felsorolt több csomagot, amely tartalmazza az hello `openssl` eszköz Windows rendszerhez.</span><span class="sxs-lookup"><span data-stu-id="95d9b-150">hello preceding section listed several packages that include hello `openssl` tool for Windows.</span></span> <span data-ttu-id="95d9b-151">Ez az eszköz a szükséges toocreate nyilvános és titkos kulcsok.</span><span class="sxs-lookup"><span data-stu-id="95d9b-151">This tool is needed toocreate public and private keys.</span></span> <span data-ttu-id="95d9b-152">a következő példák részletes hogyan hello tooinstall, és **Git for Windows**, abban az esetben, ha bármelyik csomag inkább választhat.</span><span class="sxs-lookup"><span data-stu-id="95d9b-152">hello following examples detail how tooinstall and use **Git for Windows**, though you can choose whichever package you prefer.</span></span> <span data-ttu-id="95d9b-153">**Git for Windows** hozzáférést tud biztosítani, hogy toosome további nyílt forráskódú szoftvereket ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) eszközök és segédeszközök Linux virtuális gépek használata hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="95d9b-153">**Git for Windows** gives you access toosome additional open-source software ([OSS](https://en.wikipedia.org/wiki/Open-source_software)) tools and utilities that may be useful as you work with Linux VMs.</span></span>

1. <span data-ttu-id="95d9b-154">Töltse le és telepítse **Git for Windows** a hello a következő helyen: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span><span class="sxs-lookup"><span data-stu-id="95d9b-154">Download and install **Git for Windows** from hello following location: [https://git-for-windows.github.io/](https://git-for-windows.github.io/).</span></span>
2. <span data-ttu-id="95d9b-155">Fogadja el a hello alapértelmezett beállítások hello során a telepítés folyamata hacsak nincs kifejezetten szüksége a toochange őket.</span><span class="sxs-lookup"><span data-stu-id="95d9b-155">Accept hello default options during hello install process unless you specifically need toochange them.</span></span>
3. <span data-ttu-id="95d9b-156">Futtatás **Git bash eszközt** a hello **Start menü** > **Git** > **Git bash eszközt**.</span><span class="sxs-lookup"><span data-stu-id="95d9b-156">Run **Git Bash** from hello **Start Menu** > **Git** > **Git Bash**.</span></span> <span data-ttu-id="95d9b-157">hello konzol a következő példa hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="95d9b-157">hello console looks similar toohello following example:</span></span>

    ![Git a Windows Bash rendszerhéjat](./media/ssh-from-windows/git-bash-window.png)

## <a name="create-a-private-key"></a><span data-ttu-id="95d9b-159">A titkos kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="95d9b-159">Create a private key</span></span>
1. <span data-ttu-id="95d9b-160">Az a **Git bash eszközt** ablakban használata `openssl.exe` toocreate egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="95d9b-160">In your **Git Bash** window, use `openssl.exe` toocreate a private key.</span></span> <span data-ttu-id="95d9b-161">hello alábbi példa létrehoz egy nevű kulcsot `myPrivateKey` és nevű tanúsítvány `myCert.pem`:</span><span class="sxs-lookup"><span data-stu-id="95d9b-161">hello following example creates a key named `myPrivateKey` and certificate named `myCert.pem`:</span></span>

    ```bash
    openssl.exe req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout myPrivateKey.key -out myCert.pem
    ```

    <span data-ttu-id="95d9b-162">hello kimenete a következő példa hasonló toohello néz ki:</span><span class="sxs-lookup"><span data-stu-id="95d9b-162">hello output looks similar toohello following example:</span></span>

    ```bash
    Generating a 2048 bit RSA private key
    .......................................+++
    .......................+++
    writing new private key too'myPrivateKey.key'
    -----
    You are about toobe asked tooenter information that will be incorporated
    into your certificate request.
    What you are about tooenter is what is called a Distinguished Name or a DN.
    There are quite a few fields but you can leave some blank
    For some fields there will be a default value,
    If you enter '.', hello field will be left blank.
    -----
    Country Name (2 letter code) [AU]:
    ```

   <span data-ttu-id="95d9b-163">Ha bash hiba azt jelenti, próbálkozzon egy új **Git bash eszközt** ablakot emelt szintű jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="95d9b-163">If bash reports an error, try opening a new **Git Bash** window with elevated privileges.</span></span> <span data-ttu-id="95d9b-164">Ezután futtassa újra a hello `openssl` parancsot.</span><span class="sxs-lookup"><span data-stu-id="95d9b-164">Then, rerun hello `openssl` command.</span></span>

2. <span data-ttu-id="95d9b-165">Válasz hello megadását kéri az ország-név, a helyet, a szervezet neve, a stb.</span><span class="sxs-lookup"><span data-stu-id="95d9b-165">Answer hello prompts for country name, location, organization name, etc.</span></span>
3. <span data-ttu-id="95d9b-166">Az új titkos kulcsot és tanúsítványt az aktuális munkakönyvtárban jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="95d9b-166">Your new private key and certificate are created in your current working directory.</span></span> <span data-ttu-id="95d9b-167">Biztonsági okból kell hello engedélyeinek beállítása a titkos kulcsot, hogy csak Ön férhessen hozzá:</span><span class="sxs-lookup"><span data-stu-id="95d9b-167">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey.key
    ```

4. <span data-ttu-id="95d9b-168">Hello [következő szakasz](#create-a-private-key-for-putty) PuTTYgen tooboth használatával részletek megtekintése és hello nyilvános kulcsot használ, és a titkos kulcs PuTTY tooSSH tooLinux virtuális gépek az adott létrehozása.</span><span class="sxs-lookup"><span data-stu-id="95d9b-168">hello [next section](#create-a-private-key-for-putty) details using PuTTYgen tooboth view and use hello public key, and create a private key specific for using PuTTY tooSSH tooLinux VMs.</span></span> <span data-ttu-id="95d9b-169">hello alábbi parancs létrehozza nevű nyilvános kulcsfájlt `myPublicKey.key` azonnal használható:</span><span class="sxs-lookup"><span data-stu-id="95d9b-169">hello following command generates a public key file named `myPublicKey.key` that you can use right away:</span></span>

    ```bash
    openssl.exe rsa -pubout -in myPrivateKey.key -out myPublicKey.key
    ```

5. <span data-ttu-id="95d9b-170">Ha toomanage klasszikus erőforrásokat is szükség van, alakítsa át hello `myCert.pem` túl`myCert.cer` (DER kódolású X509 tanúsítvány).</span><span class="sxs-lookup"><span data-stu-id="95d9b-170">If you also need toomanage Classic resources, convert hello `myCert.pem` too`myCert.cer` (DER encoded X509 certificate).</span></span> <span data-ttu-id="95d9b-171">Hajtsa végre ezt a lépést nem kötelező, csak akkor, ha toospecifically kell régebbi klasszikus erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="95d9b-171">Perform this optional step only if you need toospecifically manage older Classic resources.</span></span>

    <span data-ttu-id="95d9b-172">Alakítsa át a következő parancs hello hello-tanúsítvány:</span><span class="sxs-lookup"><span data-stu-id="95d9b-172">Convert hello certificate using hello following command:</span></span>

    ```bash
    openssl.exe  x509 -outform der -in myCert.pem -out myCert.cer
    ```

## <a name="create-a-private-key-for-putty"></a><span data-ttu-id="95d9b-173">Hozzon létre egy titkos kulcsot a putty rendszerhez</span><span class="sxs-lookup"><span data-stu-id="95d9b-173">Create a private key for PuTTY</span></span>
<span data-ttu-id="95d9b-174">A puTTY gyakori SSH-ügyfél Windows rendszerre.</span><span class="sxs-lookup"><span data-stu-id="95d9b-174">PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="95d9b-175">Meg vannak szabad toouse kívánja SSH ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="95d9b-175">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="95d9b-176">a PuTTY toouse, meg kell toocreate egy további típusú kulcsot – a PuTTY titkos kulcs (PPK).</span><span class="sxs-lookup"><span data-stu-id="95d9b-176">toouse PuTTY, you need toocreate an additional type of key - a PuTTY Private Key (PPK).</span></span> <span data-ttu-id="95d9b-177">Ha nem szeretné a toouse PuTTY, hagyja ki az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="95d9b-177">If you do not wish toouse PuTTY, skip this section.</span></span>

<span data-ttu-id="95d9b-178">hello alábbi példakód létrehozza a kifejezetten a PuTTY toouse további titkos kulcs:</span><span class="sxs-lookup"><span data-stu-id="95d9b-178">hello following example creates this additional private key specifically for PuTTY toouse:</span></span>

1. <span data-ttu-id="95d9b-179">Használjon **Git bash eszközt** tooconvert a titkos kulcs titkos RSA-kulcs az, hogy a PuTTYgen tudja értelmezni.</span><span class="sxs-lookup"><span data-stu-id="95d9b-179">Use **Git Bash** tooconvert your private key into an RSA private key that PuTTYgen can understand.</span></span> <span data-ttu-id="95d9b-180">hello alábbi példa létrehoz egy nevű kulcsot `myPrivateKey_rsa` nevű hello meglévő kulcsból `myPrivateKey`:</span><span class="sxs-lookup"><span data-stu-id="95d9b-180">hello following example creates a key named `myPrivateKey_rsa` from hello existing key named `myPrivateKey`:</span></span>

    ```bash
    openssl rsa -in ./myPrivateKey.key -out myPrivateKey_rsa
    ```

    <span data-ttu-id="95d9b-181">Biztonsági okból kell hello engedélyeinek beállítása a titkos kulcsot, hogy csak Ön férhessen hozzá:</span><span class="sxs-lookup"><span data-stu-id="95d9b-181">As a security measure, you should set hello permissions on your private key so that only you can access it:</span></span>

    ```bash
    chmod 0600 myPrivateKey_rsa
    ```
2. <span data-ttu-id="95d9b-182">Töltse le és futtassa a következő helyen hello PuTTYgen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="95d9b-182">Download and run PuTTYgen from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
3. <span data-ttu-id="95d9b-183">Hello menüjét: **fájl** > **terhelés titkos kulcs**</span><span class="sxs-lookup"><span data-stu-id="95d9b-183">Click hello menu: **File** > **Load Private Key**</span></span>
4. <span data-ttu-id="95d9b-184">Keresse meg a titkos kulcsot (`myPrivateKey_rsa` hello előző példában).</span><span class="sxs-lookup"><span data-stu-id="95d9b-184">Locate your private key (`myPrivateKey_rsa` in hello previous example).</span></span> <span data-ttu-id="95d9b-185">hello alapértelmezett címtár indításakor **Git bash eszközt** van `C:\Users\%username%`.</span><span class="sxs-lookup"><span data-stu-id="95d9b-185">hello default directory when you start **Git Bash** is `C:\Users\%username%`.</span></span> <span data-ttu-id="95d9b-186">Módosítsa a hello fájl szűrő tooshow **minden fájl (\*.\*)** :</span><span class="sxs-lookup"><span data-stu-id="95d9b-186">Change hello file filter tooshow **All Files (\*.\*)**:</span></span>

    ![Hello meglévő titkos kulcs betöltése a PuTTYgen](./media/ssh-from-windows/load-private-key.png)
5. <span data-ttu-id="95d9b-188">Kattintson a **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="95d9b-188">Click **Open**.</span></span> <span data-ttu-id="95d9b-189">A kérdés azt jelzi, hogy hello kulcs sikeresen importálva lett:</span><span class="sxs-lookup"><span data-stu-id="95d9b-189">A prompt indicates that hello key has been successfully imported:</span></span>

    ![Az importálás sikeres kulcs tooPuTTYgen](./media/ssh-from-windows/successfully-imported-key.png)
6. <span data-ttu-id="95d9b-191">Kattintson a **OK** tooclose hello kérdés.</span><span class="sxs-lookup"><span data-stu-id="95d9b-191">Click **OK** tooclose hello prompt.</span></span>
7. <span data-ttu-id="95d9b-192">nyilvános kulcs hello jelenik meg a hello hello tetején **PuTTYgen** ablak.</span><span class="sxs-lookup"><span data-stu-id="95d9b-192">hello public key is displayed at hello top of hello **PuTTYgen** window.</span></span> <span data-ttu-id="95d9b-193">Másolja és illessze be a nyilvános kulcs hello Azure-portálon vagy az Azure Resource Manager sablon Linux virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="95d9b-193">You copy and paste this public key into hello Azure portal or Azure Resource Manager template when you create a Linux VM.</span></span> <span data-ttu-id="95d9b-194">Is **mentés nyilvános kulcs** toosave másolási tooyour számítógép:</span><span class="sxs-lookup"><span data-stu-id="95d9b-194">You can also click **Save public key** toosave a copy tooyour computer:</span></span>

    ![PuTTY nyilvánoskulcs-fájl mentése](./media/ssh-from-windows/save-public-key.png)

    <span data-ttu-id="95d9b-196">hello alábbi példa bemutatja, hogyan szeretné másolja és illessze be a nyilvános kulcs hello Azure-portálon való Linux virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="95d9b-196">hello following example shows how you would copy and paste this public key into hello Azure portal when you create a Linux VM.</span></span> <span data-ttu-id="95d9b-197">nyilvános kulcs hello általában aztán a `~/.ssh/authorized_keys` a új virtuális gépén.</span><span class="sxs-lookup"><span data-stu-id="95d9b-197">hello public key is typically then stored in `~/.ssh/authorized_keys` on your new VM.</span></span>

    ![Nyilvános kulcs használata a virtuális gépek létrehozásakor a hello Azure-portálon](./media/ssh-from-windows/use-public-key-azure-portal.png)
8. <span data-ttu-id="95d9b-199">Vissza a **PuTTYgen**, kattintson a **titkos kulcs mentése**:</span><span class="sxs-lookup"><span data-stu-id="95d9b-199">Back in **PuTTYgen**, Click **Save private Key**:</span></span>

    ![A PuTTY titkos kulcs fájl mentése](./media/ssh-from-windows/save-ppk-file.png)

   > [!WARNING]
   > <span data-ttu-id="95d9b-201">A kérdés megkérdezi, hogy ha toocontinue kívánja-e a kulcsot a jelszó megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="95d9b-201">A prompt asks if you wish toocontinue without entering a passphrase for your key.</span></span> <span data-ttu-id="95d9b-202">Jelszó szükséges jelszó csatolt tooyour titkos kulcsa hasonlít.</span><span class="sxs-lookup"><span data-stu-id="95d9b-202">A passphrase is like a password attached tooyour private key.</span></span> <span data-ttu-id="95d9b-203">Akkor is, ha valaki tooobtain volt a titkos kulcsot, azok továbbra is nem lenne képes tooauthenticate csak hello kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="95d9b-203">Even if someone were tooobtain your private key, they still would not be able tooauthenticate using just hello key.</span></span> <span data-ttu-id="95d9b-204">Hello jelszót is kell.</span><span class="sxs-lookup"><span data-stu-id="95d9b-204">They would also need hello passphrase.</span></span> <span data-ttu-id="95d9b-205">Nélkül egy hozzáférési kódot Ha valaki beszerzi a titkos kulcsot, akkor is tooany Virtuálisgép jelentkezni vagy adott kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="95d9b-205">Without a passphrase, if someone obtains your private key, they can log in tooany VM or service that uses that key.</span></span> <span data-ttu-id="95d9b-206">Azt javasoljuk, hogy hozzon létre egy hozzáférési kódot.</span><span class="sxs-lookup"><span data-stu-id="95d9b-206">We recommend you create a passphrase.</span></span> <span data-ttu-id="95d9b-207">Azonban ha elfelejti hello jelszó, nincs nincs módja toorecover azt.</span><span class="sxs-lookup"><span data-stu-id="95d9b-207">However, if you forget hello passphrase, there is no way toorecover it.</span></span>
   >
   >

    <span data-ttu-id="95d9b-208">Ha tooenter egy hozzáférési kódot, kattintson **nem**, hello fő PuTTYgen ablakban adja meg, és kattintson a **mentés titkos kulcs** újra.</span><span class="sxs-lookup"><span data-stu-id="95d9b-208">If you wish tooenter a passphrase, click **No**, enter a passphrase in hello main PuTTYgen window, and then click **Save private key** again.</span></span> <span data-ttu-id="95d9b-209">Ellenkező esetben kattintson a **Igen** toocontinue hello választható jelszó megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="95d9b-209">Otherwise, click **Yes** toocontinue without providing hello optional passphrase.</span></span>
9. <span data-ttu-id="95d9b-210">Adjon meg egy név és hely toosave a PPK fájlt.</span><span class="sxs-lookup"><span data-stu-id="95d9b-210">Enter a name and location toosave your PPK file.</span></span>

## <a name="use-putty-toossh-tooa-linux-machine"></a><span data-ttu-id="95d9b-211">Használja a Putty tooSSH tooa Linux rendszerű számítógép</span><span class="sxs-lookup"><span data-stu-id="95d9b-211">Use Putty tooSSH tooa Linux Machine</span></span>
<span data-ttu-id="95d9b-212">A PuTTY újra, a gyakori SSH-ügyfél Windows.</span><span class="sxs-lookup"><span data-stu-id="95d9b-212">Again, PuTTY is a common SSH client for Windows.</span></span> <span data-ttu-id="95d9b-213">Meg vannak szabad toouse kívánja SSH ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="95d9b-213">You are free toouse any SSH client that you wish.</span></span> <span data-ttu-id="95d9b-214">a következő lépések részletes hogyan hello toouse a titkos kulcs tooauthenticate együtt az Azure virtuális gép SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="95d9b-214">hello following steps detail how toouse your private key tooauthenticate with your Azure VM using SSH.</span></span> <span data-ttu-id="95d9b-215">hello lépések hasonlóak a más SSH-kulcs ügyfél kellene tooload a titkos kulcs tooauthenticate hello SSH-kapcsolat tekintetében.</span><span class="sxs-lookup"><span data-stu-id="95d9b-215">hello steps are similar in other SSH key clients in terms of needing tooload your private key tooauthenticate hello SSH connection.</span></span>

1. <span data-ttu-id="95d9b-216">Letöltése és futtatása a putty a hello a következő helyen: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span><span class="sxs-lookup"><span data-stu-id="95d9b-216">Download and run putty from hello following location: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)</span></span>
2. <span data-ttu-id="95d9b-217">Töltse ki hello állomásnevet vagy IP-címet a virtuális gép a hello Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="95d9b-217">Fill in hello host name or IP address of your VM from hello Azure portal:</span></span>

    ![Új PuTTY kapcsolat megnyitása](./media/ssh-from-windows/putty-new-connection.png)
3. <span data-ttu-id="95d9b-219">Mielőtt kiválasztja **nyissa meg**, kattintson a **kapcsolat** > **SSH** > **Auth** lapon. Tallózás tooand a titkos kulcs kiválasztása:</span><span class="sxs-lookup"><span data-stu-id="95d9b-219">Before selecting **Open**, click **Connection** > **SSH** > **Auth** tab. Browse tooand select your private key:</span></span>

    ![Válassza ki a PuTTY titkos kulcsot a hitelesítéshez](./media/ssh-from-windows/putty-auth-dialog.png)
4. <span data-ttu-id="95d9b-221">Kattintson a **nyitott** tooconnect tooyour virtuális gép</span><span class="sxs-lookup"><span data-stu-id="95d9b-221">Click **Open** tooconnect tooyour virtual machine</span></span>

## <a name="next-steps"></a><span data-ttu-id="95d9b-222">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="95d9b-222">Next steps</span></span>
<span data-ttu-id="95d9b-223">Hello nyilvános és titkos kulcsokat is létrehozhat [OS X-és Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95d9b-223">You can also generate hello public and private keys [using OS X and Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="95d9b-224">További információ a Bash a Windows és a hello előnyei OSS eszközöket a Windows-számítógépen: [Windows Ubuntu a Bash](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="95d9b-224">For more information about Bash for Windows and hello benefits of having OSS tools readily available on your Windows computer, see [Bash on Ubuntu on Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span>

<span data-ttu-id="95d9b-225">Ha gondja támad SSH tooconnect tooyour Linux virtuális gépek használatával, lásd: [hibaelhárítása SSH kapcsolatok tooan Azure Linux virtuális gép](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="95d9b-225">If you have trouble using SSH tooconnect tooyour Linux VMs, see [Troubleshoot SSH connections tooan Azure Linux VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
