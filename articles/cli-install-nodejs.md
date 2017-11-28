---
title: "Telepítse az Azure parancssori felület 1.0 |} Microsoft Docs"
description: "Telepítse az Azure CLI 1.0 Mac, Linux és a Windows Azure-szolgáltatások használatának megkezdéséhez"
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
ms.openlocfilehash: 63b35ed25b809a16b61b685fd35aa67474b0a369
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="install-the-azure-cli-10"></a><span data-ttu-id="f0e46-103">Az Azure parancssori felület 1.0-s telepítése</span><span class="sxs-lookup"><span data-stu-id="f0e46-103">Install the Azure CLI 1.0</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0e46-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f0e46-104">PowerShell</span></span>](/powershell/azure/overview)
> * [<span data-ttu-id="f0e46-105">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="f0e46-105">Azure CLI 1.0</span></span>](cli-install-nodejs.md)
> * [<span data-ttu-id="f0e46-106">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="f0e46-106">Azure CLI 2.0</span></span>](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> <span data-ttu-id="f0e46-107">Ez a témakör ismerteti, hogyan telepítheti az Azure CLI 1.0, amely nodeJs épül, és támogatja a klasszikus üzembe helyezési minden API-hívások, valamint a nagy mennyiségű erőforrás-kezelő telepítési tevékenységet.</span><span class="sxs-lookup"><span data-stu-id="f0e46-107">This topic describes how to install the Azure CLI 1.0, which is built on nodeJs and supports all classic deployment API calls as well as a large number of Resource Manager deployment activities.</span></span> <span data-ttu-id="f0e46-108">Használjon a [Azure CLI 2.0](/cli/azure/overview) új vagy előretekintő CLI-telepítések és kezelésére.</span><span class="sxs-lookup"><span data-stu-id="f0e46-108">You should use the [Azure CLI 2.0](/cli/azure/overview) for new or forward-looking CLI deployments and management.</span></span>

<span data-ttu-id="f0e46-109">Gyorsan telepíti az Azure parancssori felület (Azure CLI 1.0-s)-alapú parancsokat nyílt forráskódú használandó létrehozása és kezelése a Microsoft Azure erőforrásait.</span><span class="sxs-lookup"><span data-stu-id="f0e46-109">Quickly install the Azure Command-Line Interface (Azure CLI 1.0) to use a set of open-source shell-based commands for creating and managing resources in Microsoft Azure.</span></span> <span data-ttu-id="f0e46-110">Az platformfüggetlen-eszközök telepítése a számítógépre több lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="f0e46-110">You have several options to install these cross-platform tools on your computer:</span></span>

* <span data-ttu-id="f0e46-111">**npm csomag** -npm (a Csomagkezelőt a JavaScript) futtassa a Linux-disztribúció vagy az operációs rendszer a legújabb Azure CLI 1.0 csomag telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f0e46-111">**npm package** - Run npm (the package manager for JavaScript) to install the latest Azure CLI 1.0 package on your Linux distribution or OS.</span></span> <span data-ttu-id="f0e46-112">Szükséges, node.js és npm a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="f0e46-112">Requires node.js and npm on your computer.</span></span>
* <span data-ttu-id="f0e46-113">**Telepítő** -könnyen Mac vagy Windows-telepítést telepítőjének letöltése.</span><span class="sxs-lookup"><span data-stu-id="f0e46-113">**Installer** - Download an installer for easy installation on Mac or Windows.</span></span>
* <span data-ttu-id="f0e46-114">**Docker-tároló** - Start készen áll a futásra Docker-tároló a legújabb parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="f0e46-114">**Docker container** - Start using the latest CLI in a ready-to-run Docker container.</span></span> <span data-ttu-id="f0e46-115">A számítógépen a Docker gazdagépet igényel.</span><span class="sxs-lookup"><span data-stu-id="f0e46-115">Requires Docker host on your computer.</span></span>

<span data-ttu-id="f0e46-116">A további beállítások és a háttérben, tekintse meg a projekt tárház [GitHub](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="f0e46-116">For more options and background, see the project repository on [GitHub](https://github.com/azure/azure-xplat-cli).</span></span>

<span data-ttu-id="f0e46-117">Az Azure CLI 1.0-s telepítése után [csatlakoztassa az Azure előfizetéssel rendelkező](xplat-cli-connect.md) , és futtassa a **azure** az Azure-erőforrások kezelésére használható a parancssori felületről (Bash, Terminálszolgáltatások, parancssort, és így tovább) parancsokat.</span><span class="sxs-lookup"><span data-stu-id="f0e46-117">Once the Azure CLI 1.0 is installed, [connect it with your Azure subscription](xplat-cli-connect.md) and run the **azure** commands from your command-line interface (Bash, Terminal, Command prompt, and so on) to work with your Azure resources.</span></span>

## <a name="option-1-install-an-npm-package"></a><span data-ttu-id="f0e46-118">1. lehetőség: Egy npm csomag telepítése</span><span class="sxs-lookup"><span data-stu-id="f0e46-118">Option 1: Install an npm package</span></span>
<span data-ttu-id="f0e46-119">A parancssori felület telepítése npm-csomagból, ellenőrizze, hogy már letöltötte és telepítette a [legújabb Node.js és npm](https://nodejs.org/en/download/package-manager/).</span><span class="sxs-lookup"><span data-stu-id="f0e46-119">To install the CLI from an npm package, make sure you have downloaded and installed the [latest Node.js and npm](https://nodejs.org/en/download/package-manager/).</span></span> <span data-ttu-id="f0e46-120">Ezután futtassa **npm telepítése** az azure-cli csomag telepítése:</span><span class="sxs-lookup"><span data-stu-id="f0e46-120">Then, run **npm install** to install the azure-cli package:</span></span>

```bash
npm install -g azure-cli
```

<span data-ttu-id="f0e46-121">A Linux terjesztéseket, előfordulhat, hogy szüksége **sudo** sikeresen futtatni a **npm** parancs, az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="f0e46-121">On Linux distributions, you might need to use **sudo** to successfully run the **npm** command, as follows:</span></span>

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> <span data-ttu-id="f0e46-122">Ha telepíteni vagy frissíteni a Node.js és a Linux-disztribúció vagy az operációs rendszer npm van szüksége, azt javasoljuk, hogy telepítse a Node.js LTS legfrissebb (4.x).</span><span class="sxs-lookup"><span data-stu-id="f0e46-122">If you need to install or update Node.js and npm on your Linux distribution or OS, we recommend that you install the most recent Node.js LTS version (4.x).</span></span> <span data-ttu-id="f0e46-123">Ha egy régebbi verzióját használja, előfordulhat, hogy hibaüzenet telepítését.</span><span class="sxs-lookup"><span data-stu-id="f0e46-123">If you use an older version, you might get installation errors.</span></span>

<span data-ttu-id="f0e46-124">Ha szeretné, töltse le a legfrissebb Linux [bont fájl] [ linux-installer] helyileg az npm-csomag.</span><span class="sxs-lookup"><span data-stu-id="f0e46-124">If you prefer, download the latest Linux [tar file][linux-installer] for the npm package locally.</span></span> <span data-ttu-id="f0e46-125">Majd telepítse az alábbiak szerint a letöltött npm csomagot (a Linux terjesztéseket, előfordulhat, hogy szeretné használni, **sudo**):</span><span class="sxs-lookup"><span data-stu-id="f0e46-125">Then, install the downloaded npm package as follows (on Linux distributions you might need to use **sudo**):</span></span>

```bash
npm install -g <path to downloaded tar file>
```

## <a name="option-2-use-an-installer"></a><span data-ttu-id="f0e46-126">2. lehetőség: Egy telepítővel</span><span class="sxs-lookup"><span data-stu-id="f0e46-126">Option 2: Use an installer</span></span>
<span data-ttu-id="f0e46-127">Ha egy Mac vagy Windows-számítógépet használ, a következő parancssori Felületet telepítőcsomagokat letölthetők:</span><span class="sxs-lookup"><span data-stu-id="f0e46-127">If you use a Mac or Windows computer, the following CLI installers are available for download:</span></span>

* <span data-ttu-id="f0e46-128">[Mac OS X telepítőjét][mac-installer]</span><span class="sxs-lookup"><span data-stu-id="f0e46-128">[Mac OS X installer][mac-installer]</span></span>
* <span data-ttu-id="f0e46-129">[Windows MSI][windows-installer]</span><span class="sxs-lookup"><span data-stu-id="f0e46-129">[Windows MSI][windows-installer]</span></span>

> [!TIP]
> <span data-ttu-id="f0e46-130">A Windows rendszeren is letöltheti a [Webplatform-telepítő](https://go.microsoft.com/?linkid=9828653) a parancssori felület telepítése.</span><span class="sxs-lookup"><span data-stu-id="f0e46-130">On Windows, you can also download the [Web Platform Installer](https://go.microsoft.com/?linkid=9828653) to install the CLI.</span></span> <span data-ttu-id="f0e46-131">A telepítő lehetővé teszi a parancssori felület telepítése után további Azure SDK-t és parancssori eszközök telepítése lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="f0e46-131">This installer gives you the option to install additional Azure SDK and command-line tools after installing the CLI.</span></span>

## <a name="option-3-use-a-docker-container"></a><span data-ttu-id="f0e46-132">3. lehetőség: Egy Docker-tároló használata</span><span class="sxs-lookup"><span data-stu-id="f0e46-132">Option 3: Use a Docker container</span></span>
<span data-ttu-id="f0e46-133">Ha úgy állította be a számítógépre, a [Docker](https://docs.docker.com/engine/understanding-docker/) állomás, futtathatja az Azure CLI legújabb 1.0 egy Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="f0e46-133">If you have set up your computer as a [Docker](https://docs.docker.com/engine/understanding-docker/) host, you can run the latest Azure CLI 1.0 in a Docker container.</span></span> <span data-ttu-id="f0e46-134">A következő parancsot (a Linux terjesztéseket, előfordulhat, hogy szeretné használni, **sudo**):</span><span class="sxs-lookup"><span data-stu-id="f0e46-134">Run the following command (on Linux distributions you might need to use **sudo**):</span></span>

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a><span data-ttu-id="f0e46-135">Azure CLI 1.0-parancsok futtatása</span><span class="sxs-lookup"><span data-stu-id="f0e46-135">Run Azure CLI 1.0 commands</span></span>
<span data-ttu-id="f0e46-136">Az Azure CLI 1.0-s telepítése után futtassa a **azure** parancsot a parancssori felületén (Bash, Terminálszolgáltatások, parancssort, és így tovább).</span><span class="sxs-lookup"><span data-stu-id="f0e46-136">After the Azure CLI 1.0 is installed, run the **azure** command from your command-line user interface (Bash, Terminal, Command prompt, and so on).</span></span> <span data-ttu-id="f0e46-137">A Súgó parancs futtatásához írja be például a következőket:</span><span class="sxs-lookup"><span data-stu-id="f0e46-137">For example, to run the help command, type the following:</span></span>

```azurecli
azure help
```

> [!NOTE]
> <span data-ttu-id="f0e46-138">Egyes Linux terjesztésekről hasonló hiba jelenhet `/usr/bin/env: ‘node’: No such file or directory`.</span><span class="sxs-lookup"><span data-stu-id="f0e46-138">On some Linux distributions, you may receive an error similar to `/usr/bin/env: ‘node’: No such file or directory`.</span></span> <span data-ttu-id="f0e46-139">Ez a hiba a legújabb telepítendő /usr/bin/nodejs, Node.js-telepítések származik.</span><span class="sxs-lookup"><span data-stu-id="f0e46-139">This error comes from recent installations of Node.js being installed at /usr/bin/nodejs.</span></span> <span data-ttu-id="f0e46-140">Megjavítani, ez a parancs futtatásával hozzon létre /usr/bin/node a szimbolikus hivatkozást:</span><span class="sxs-lookup"><span data-stu-id="f0e46-140">To fix it, create a symbolic link to /usr/bin/node by running this command:</span></span>

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

<span data-ttu-id="f0e46-141">Az Azure CLI 1.0 telepített verziójának megtekintéséhez írja be a következőt:</span><span class="sxs-lookup"><span data-stu-id="f0e46-141">To see the version of the Azure CLI 1.0 you installed, type the following:</span></span>

```azurecli
azure --version
```

<span data-ttu-id="f0e46-142">Most már készen áll!</span><span class="sxs-lookup"><span data-stu-id="f0e46-142">Now you are ready!</span></span> <span data-ttu-id="f0e46-143">A saját erőforrásokat parancssori felület parancsai eléréséhez [csatlakozzon az Azure-előfizetéshez az Azure parancssori felületen](xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="f0e46-143">To access all the CLI commands to work with your own resources, [connect to your Azure subscription from the Azure CLI](xplat-cli-connect.md).</span></span>

> [!NOTE]
> <span data-ttu-id="f0e46-144">Ha először használja az Azure parancssori felület, megjelenik egy üzenet rákérdez, hogy a Microsoft használati adatokat gyűjthet a.</span><span class="sxs-lookup"><span data-stu-id="f0e46-144">When you first use Azure CLI, you see a message asking if you want to allow Microsoft to collect usage information.</span></span> <span data-ttu-id="f0e46-145">Részvétel önkéntes történik.</span><span class="sxs-lookup"><span data-stu-id="f0e46-145">Participation is voluntary.</span></span> <span data-ttu-id="f0e46-146">Ha a részvétel mellett dönt, le is bármikor futtatásával `azure telemetry --disable`.</span><span class="sxs-lookup"><span data-stu-id="f0e46-146">If you choose to participate, you can stop at any time by running `azure telemetry --disable`.</span></span> <span data-ttu-id="f0e46-147">Részvétel bármikor engedélyezéséhez futtassa `azure telemetry --enable`.</span><span class="sxs-lookup"><span data-stu-id="f0e46-147">To enable participation at any time, run `azure telemetry --enable`.</span></span>

## <a name="update-the-cli"></a><span data-ttu-id="f0e46-148">A parancssori felület frissítése</span><span class="sxs-lookup"><span data-stu-id="f0e46-148">Update the CLI</span></span>
<span data-ttu-id="f0e46-149">A Microsoft gyakran ad az Azure parancssori felület frissített verzióit.</span><span class="sxs-lookup"><span data-stu-id="f0e46-149">Microsoft frequently releases updated versions of the Azure CLI.</span></span> <span data-ttu-id="f0e46-150">Telepítse újra a telepítő használatával az operációs rendszer CLI, vagy futtassa a legújabb Docker-tároló.</span><span class="sxs-lookup"><span data-stu-id="f0e46-150">Reinstall the CLI using the installer for your operating system, or run the latest Docker container.</span></span> <span data-ttu-id="f0e46-151">Vagy, ha a legfrissebb Node.js és npm telepítve van, frissítse a következő (a Linux terjesztéseket, előfordulhat, hogy szeretné használni, **sudo**).</span><span class="sxs-lookup"><span data-stu-id="f0e46-151">Or, if you have the latest Node.js and npm installed, update by typing the following (on Linux distributions you might need to use **sudo**).</span></span>

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a><span data-ttu-id="f0e46-152">Kiegészítés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0e46-152">Enable tab completion</span></span>
<span data-ttu-id="f0e46-153">A parancssori felület parancsait kiegészítést Mac és Linux esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="f0e46-153">Tab completion of CLI commands is supported for Mac and Linux.</span></span>

<span data-ttu-id="f0e46-154">Engedélyezze a zsh, futtassa:</span><span class="sxs-lookup"><span data-stu-id="f0e46-154">To enable it in zsh, run:</span></span>

```bash
echo '. <(azure --completion)' >> .zshrc
```

<span data-ttu-id="f0e46-155">Engedélyezze a bash, futtassa:</span><span class="sxs-lookup"><span data-stu-id="f0e46-155">To enable it in bash, run:</span></span>

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a><span data-ttu-id="f0e46-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f0e46-156">Next steps</span></span>
* <span data-ttu-id="f0e46-157">[Csatlakoztassa a CLI Azure-előfizetése](xplat-cli-connect.md) létrehozásához és kezeléséhez az Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="f0e46-157">[Connect from the CLI to your Azure subscription](xplat-cli-connect.md) to create and manage Azure resources.</span></span>
* <span data-ttu-id="f0e46-158">További tudnivalók az Azure parancssori felület, letöltési forráskódját, kapcsolatos problémákat, vagy a projekt hozzájárul, látogasson el a [az Azure parancssori felület GitHub-tárházban](https://github.com/azure/azure-xplat-cli).</span><span class="sxs-lookup"><span data-stu-id="f0e46-158">To learn more about the Azure CLI, download source code, report problems, or contribute to the project, visit the [GitHub repository for the Azure CLI](https://github.com/azure/azure-xplat-cli).</span></span>
* <span data-ttu-id="f0e46-159">Ha az Azure parancssori felület, vagy az Azure használatával kapcsolatos kérdésekre, keresse fel a [Azure fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span><span class="sxs-lookup"><span data-stu-id="f0e46-159">If you have questions about using the Azure CLI, or Azure, visit the [Azure Forums](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).</span></span>


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
