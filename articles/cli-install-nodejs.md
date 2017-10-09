---
title: aaaInstall hello Azure CLI 1.0 |} Microsoft Docs
description: "Hello Azure CLI 1.0 a Mac, Linux és Windows toostart az Azure szolgáltatások telepítése"
editor: 
manager: timlt
documentationcenter: 
author: squillace
services: virtual-machines-linux,virtual-network,storage,azure-resource-manager
tags: azure-resource-manager,azure-service-management
ms.assetid: bdb776c8-7a76-4f3a-887c-236b4fffee10
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: rasquill
ms.openlocfilehash: a8cd4e38fde6e4b17a768a7caecd280cd91a70f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-azure-cli-10"></a><span data-ttu-id="48bb1-103">Hello Azure CLI 1.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="48bb1-103">Install hello Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="48bb1-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="48bb1-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="48bb1-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="48bb1-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="48bb1-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="48bb1-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="48bb1-107">Ez a témakör ismerteti, hogyan tooinstall hello Azure CLI 1.0, amely nodeJs épül, és minden klasszikus üzembe helyezési API támogatja továbbá az erőforrás-kezelő telepítési műveletek nagy számú hívja.</span><span class="sxs-lookup"><span data-stu-id="48bb1-107">This topic describes how tooinstall hello Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="48bb1-108">Használjon hello [Azure CLI 2.0](/cli/azure/overview) új vagy előretekintő CLI-telepítések és kezelésére.</span><span class="sxs-lookup"><span data-stu-id="48bb1-108">You should use hello [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="48bb1-109">Gyors telepítés hello Azure parancssori felület (Azure CLI 1.0) toouse nyílt forráskódú-alapú parancsokat létrehozására és kezelésére a Microsoft Azure erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="48bb1-109">Quickly install hello Azure Command-Line Interface (Azure CLI 1.0) toouse a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="48bb1-110">Van több beállítások tooinstall ezen platformfüggetlen-eszközök a számítógépen:</span><span class="sxs-lookup"><span data-stu-id="48bb1-110">You have several options tooinstall these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="48bb1-111">**npm csomag** - futtatási npm (hello Csomagkezelőt a JavaScript) tooinstall hello legújabb Azure CLI 1.0 csomag a Linux-disztribúció vagy az operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="48bb1-111">**npm package** - Run npm (hello package manager for JavaScript) tooinstall hello latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="48bb1-112">Szükséges, node.js és npm a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="48bb1-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="48bb1-113">**Telepítő** -könnyen Mac vagy Windows-telepítést telepítőjének letöltése.</span><span class="sxs-lookup"><span data-stu-id="48bb1-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="48bb1-114">**Docker-tároló** – hello segítségével Start legújabb CLI készen áll a futásra Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="48bb1-114">**Docker container** - Start using hello latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="48bb1-115">A számítógépen a Docker gazdagépet igényel.</span><span class="sxs-lookup"><span data-stu-id="48bb1-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="48bb1-116">A további beállítások és a háttérben, tekintse meg hello projekt tárház [GitHub](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="48bb1-116">For more options and background, see hello project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="48bb1-117">Hello Azure CLI 1.0-s telepítése után [csatlakoztassa az Azure előfizetéssel rendelkező](xplat-cli-connect.md) és futtatási hello **azure** a parancssori felület (Bash, Terminálszolgáltatások, parancssort, és így tovább) parancsok a toowork az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="48bb1-117">Once hello Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run hello **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) toowork with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="48bb1-118">1. lehetőség: Egy npm csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="48bb1-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="48bb1-119">tooinstall hello CLI az npm-csomagból, győződjön meg arról, letöltött és telepített hello [legújabb Node.js és npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="48bb1-119">tooinstall hello CLI from an npm package, make sure you have downloaded and installed hello [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="48bb1-120">Ezután futtassa **npm telepítése** tooinstall hello azure-cli csomagot:</span><span class="sxs-lookup"><span data-stu-id="48bb1-120">Then, run **npm install** tooinstall hello azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="48bb1-121">A Linux terjesztéseket, szükség lehet a toouse **sudo** futtatása toosuccessfully hello **npm** parancs, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="48bb1-121">On Linux distributions, you might need toouse **sudo** toosuccessfully run hello **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="48bb1-122">Ha tooinstall kell, vagy frissítse a Node.js és a Linux-disztribúció vagy az operációs rendszer npm, ajánlott hello legutóbbi Node.js LTS verzió (4.x) telepítését.</span><span class="sxs-lookup"><span data-stu-id="48bb1-122">If you need tooinstall or update Node.js and npm on your Linux distribution or OS, we recommend that you install hello most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="48bb1-123">Ha egy régebbi verzióját használja, előfordulhat, hogy hibaüzenet telepítését.</span><span class="sxs-lookup"><span data-stu-id="48bb1-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="48bb1-124">Tetszés szerint töltse le a legfrissebb Linux hello [bont fájl] [ linux-installer] hello npm a csomag helyileg.</span><span class="sxs-lookup"><span data-stu-id="48bb1-124">If you prefer, download hello latest Linux [tar file][linux-installer] for hello npm package locally.</span></span> <span data-ttu-id="48bb1-125">Majd telepítse az alábbiak szerint hello npm letöltött csomagot (a Linux terjesztéseket szükség lehet a toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="48bb1-125">Then, install hello downloaded npm package as follows (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="48bb1-126">2. lehetőség: Egy telepítővel</span><span class="sxs-lookup"><span data-stu-id="48bb1-126">Option 2: Use an installer</span></span>
<span data-ttu-id="48bb1-127">Ha egy Mac vagy Windows-számítógépet használ, a következő parancssori Felületet telepítőcsomagokat hello letölthetők:</span><span class="sxs-lookup"><span data-stu-id="48bb1-127">If you use a Mac or Windows computer, hello following CLI installers are available for download:</span></span>

* <span data-ttu-id="48bb1-128">[Mac OS X telepítőjét][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="48bb1-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="48bb1-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="48bb1-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="48bb1-130">A Windows rendszeren is letöltheti hello [Webplatform-telepítő](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI-t.</span><span class="sxs-lookup"><span data-stu-id="48bb1-130">On Windows, you can also download hello [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI.</span></span> <span data-ttu-id="48bb1-131">A telepítő által biztosított lehetőséget tooinstall hello meg további Azure SDK-t és parancssori eszközök telepítése után hello CLI-t.</span><span class="sxs-lookup"><span data-stu-id="48bb1-131">This installer gives you hello option tooinstall additional Azure SDK and command-line tools after installing hello CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="48bb1-132">3. lehetőség: Egy Docker-tároló használata</span><span class="sxs-lookup"><span data-stu-id="48bb1-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="48bb1-133">Ha úgy állította be a számítógépre, a [Docker](https://docs.docker.com/engine/understanding-docker/) állomás, futtathatja egy Docker-tároló az Azure CLI legújabb 1.0 hello.</span><span class="sxs-lookup"><span data-stu-id="48bb1-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run hello latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="48bb1-134">Futtatási hello következő parancsot (a Linux terjesztéseket szükség lehet a toouse **sudo**):</span><span class="sxs-lookup"><span data-stu-id="48bb1-134">Run hello following command (on Linux distributions you might need toouse **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="48bb1-135">Azure CLI 1.0-parancsok futtatása</span><span class="sxs-lookup"><span data-stu-id="48bb1-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="48bb1-136">Hello Azure CLI 1.0-s telepítése után futtassa a hello **azure** parancsot a parancssori felületén (Bash, Terminálszolgáltatások, parancssort, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="48bb1-136">After hello Azure CLI 1.0 is installed, run hello **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="48bb1-137">Például a toorun hello help parancsot, írja be a hello következő:</span><span class="sxs-lookup"><span data-stu-id="48bb1-137">For example, toorun hello help command, type hello following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="48bb1-138">Az egyes Linux terjesztéseket, előfordulhat, hogy hibaüzenet hasonló túl`/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="48bb1-138">On some Linux distributions, you may receive an error similar too`/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="48bb1-139">Ez a hiba a legújabb telepítendő /usr/bin/nodejs, Node.js-telepítések származik.</span><span class="sxs-lookup"><span data-stu-id="48bb1-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="48bb1-140">toofix, a szimbolikus hivatkozást túl/usr/bin/csomópont létrehozása a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="48bb1-140">toofix it, create a symbolic link too/usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="48bb1-141">hello Azure CLI 1.0 telepíti, akkor a következő típus hello toosee hello verziója:</span><span class="sxs-lookup"><span data-stu-id="48bb1-141">toosee hello version of hello Azure CLI 1.0 you installed, type hello following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="48bb1-142">Most már készen áll!</span><span class="sxs-lookup"><span data-stu-id="48bb1-142">Now you are ready!</span></span> <span data-ttu-id="48bb1-143">minden tooaccess hello CLI parancsok toowork saját erőforrásokkal [hello Azure CLI Azure-előfizetés tooyour kapcsolódó](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="48bb1-143">tooaccess all hello CLI commands toowork with your own resources, [connect tooyour Azure subscription from hello Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="48bb1-144">Első használata alkalmával érdemes az Azure parancssori felület, megjelenik egy üzenet megkérdezi, tooallow Microsoft toocollect használati adatait.</span><span class="sxs-lookup"><span data-stu-id="48bb1-144">When you first use Azure CLI, you see a message asking if you want tooallow Microsoft toocollect usage information.</span></span> <span data-ttu-id="48bb1-145">Részvétel önkéntes történik.</span><span class="sxs-lookup"><span data-stu-id="48bb1-145">Participation is voluntary.</span></span> <span data-ttu-id="48bb1-146">Ha úgy dönt, hogy tooparticipate, le is bármikor futtatásával `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="48bb1-146">If you choose tooparticipate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="48bb1-147">bármikor, tooenable részvételét futtassa `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="48bb1-147">tooenable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-hello-cli"></a><span data-ttu-id="48bb1-148">Frissítés hello parancssori felület</span><span class="sxs-lookup"><span data-stu-id="48bb1-148">Update hello CLI</span></span>
<span data-ttu-id="48bb1-149">Microsoft gyakran kiadott hello Azure CLI frissített verzióit.</span><span class="sxs-lookup"><span data-stu-id="48bb1-149">Microsoft frequently releases updated versions of hello Azure CLI.</span></span> <span data-ttu-id="48bb1-150">Telepítse újra a CLI hello installer használatával az operációs rendszer hello, vagy futtassa az hello legújabb Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="48bb1-150">Reinstall hello CLI using hello installer for your operating system, or run hello latest Docker container.</span></span> <span data-ttu-id="48bb1-151">Vagy, ha legújabb Node.js és npm telepítve rendelkezik hello, frissítse a következő hello (a Linux terjesztéseket szükség lehet a toouse **sudo**).</span><span class="sxs-lookup"><span data-stu-id="48bb1-151">Or, if you have hello latest Node.js and npm installed, update by typing hello following (on Linux distributions you might need toouse **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="48bb1-152">Kiegészítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="48bb1-152">Enable tab completion</span></span>
<span data-ttu-id="48bb1-153">A parancssori felület parancsait kiegészítést Mac és Linux esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="48bb1-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="48bb1-154">tooenable zsh, a futtatni:</span><span class="sxs-lookup"><span data-stu-id="48bb1-154">tooenable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="48bb1-155">tooenable a bash futtatni:</span><span class="sxs-lookup"><span data-stu-id="48bb1-155">tooenable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="48bb1-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="48bb1-156">Next steps</span></span>
* <span data-ttu-id="48bb1-157">[Csatlakoztatja a hello CLI tooyour Azure-előfizetés](xplat-cli-connect.md) toocreate és az Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="48bb1-157">[Connect from hello CLI tooyour Azure subscription](xplat-cli-connect.md) toocreate and manage Azure resources.</span></span>
* <span data-ttu-id="48bb1-158">toolearn hello Azure parancssori Felülettel kapcsolatos további információkért töltse le a forráskódot, jelentse az esetleges problémákat, vagy toohello projekt közre, látogasson el a hello [hello Azure CLI GitHub-tárházban](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="48bb1-158">toolearn more about hello Azure CLI, download source code, report problems, or contribute toohello project, visit hello [GitHub repository for hello Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="48bb1-159">Ha az Azure parancssori felület hello, vagy Azure használatával kapcsolatos kérdésekre, keresse fel a hello [Azure fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="48bb1-159">If you have questions about using hello Azure CLI, or Azure, visit hello [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
