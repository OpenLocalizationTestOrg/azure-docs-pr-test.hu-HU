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
# <a name="install-hello-azure-cli-10"></a>Hello Azure CLI 1.0 telepítése
> [!div class="op_single_selector"]
> * [PowerShell](/powershell/azure/overview)
> * [Azure CLI 1.0](cli-install-nodejs.md)
> * [Azure CLI 2.0](/cli/azure/install-azure-cli)

> [!IMPORTANT]
> Ez a témakör ismerteti, hogyan tooinstall hello Azure CLI 1.0, amely nodeJs épül, és minden klasszikus üzembe helyezési API támogatja továbbá az erőforrás-kezelő telepítési műveletek nagy számú hívja. Használjon hello [Azure CLI 2.0](/cli/azure/overview) új vagy előretekintő CLI-telepítések és kezelésére.

Gyors telepítés hello Azure parancssori felület (Azure CLI 1.0) toouse nyílt forráskódú-alapú parancsokat létrehozására és kezelésére a Microsoft Azure erőforrásait. Van több beállítások tooinstall ezen platformfüggetlen-eszközök a számítógépen:

* **npm csomag** - futtatási npm (hello Csomagkezelőt a JavaScript) tooinstall hello legújabb Azure CLI 1.0 csomag a Linux-disztribúció vagy az operációs rendszer. Szükséges, node.js és npm a számítógépen.
* **Telepítő** -könnyen Mac vagy Windows-telepítést telepítőjének letöltése.
* **Docker-tároló** – hello segítségével Start legújabb CLI készen áll a futásra Docker-tároló. A számítógépen a Docker gazdagépet igényel.

A további beállítások és a háttérben, tekintse meg hello projekt tárház [GitHub](https://github.com/azure/azure-xplat-cli).

Hello Azure CLI 1.0-s telepítése után [csatlakoztassa az Azure előfizetéssel rendelkező](xplat-cli-connect.md) és futtatási hello **azure** a parancssori felület (Bash, Terminálszolgáltatások, parancssort, és így tovább) parancsok a toowork az Azure-erőforrások.

## <a name="option-1-install-an-npm-package"></a>1. lehetőség: Egy npm csomag telepítése
tooinstall hello CLI az npm-csomagból, győződjön meg arról, letöltött és telepített hello [legújabb Node.js és npm](https://nodejs.org/en/download/package-manager/). Ezután futtassa **npm telepítése** tooinstall hello azure-cli csomagot:

```bash
npm install -g azure-cli
```

A Linux terjesztéseket, szükség lehet a toouse **sudo** futtatása toosuccessfully hello **npm** parancs, az alábbiak szerint:

```bash
sudo npm install -g azure-cli
```

> [!NOTE]
> Ha tooinstall kell, vagy frissítse a Node.js és a Linux-disztribúció vagy az operációs rendszer npm, ajánlott hello legutóbbi Node.js LTS verzió (4.x) telepítését. Ha egy régebbi verzióját használja, előfordulhat, hogy hibaüzenet telepítését.

Tetszés szerint töltse le a legfrissebb Linux hello [bont fájl] [ linux-installer] hello npm a csomag helyileg. Majd telepítse az alábbiak szerint hello npm letöltött csomagot (a Linux terjesztéseket szükség lehet a toouse **sudo**):

```bash
npm install -g <path toodownloaded tar file>
```

## <a name="option-2-use-an-installer"></a>2. lehetőség: Egy telepítővel
Ha egy Mac vagy Windows-számítógépet használ, a következő parancssori Felületet telepítőcsomagokat hello letölthetők:

* [Mac OS X telepítőjét][mac-installer]
* [Windows MSI][windows-installer]

> [!TIP]
> A Windows rendszeren is letöltheti hello [Webplatform-telepítő](https://go.microsoft.com/?linkid=9828653) tooinstall hello CLI-t. A telepítő által biztosított lehetőséget tooinstall hello meg további Azure SDK-t és parancssori eszközök telepítése után hello CLI-t.

## <a name="option-3-use-a-docker-container"></a>3. lehetőség: Egy Docker-tároló használata
Ha úgy állította be a számítógépre, a [Docker](https://docs.docker.com/engine/understanding-docker/) állomás, futtathatja egy Docker-tároló az Azure CLI legújabb 1.0 hello. Futtatási hello következő parancsot (a Linux terjesztéseket szükség lehet a toouse **sudo**):

```bash
docker run -it microsoft/azure-cli
```

## <a name="run-azure-cli-10-commands"></a>Azure CLI 1.0-parancsok futtatása
Hello Azure CLI 1.0-s telepítése után futtassa a hello **azure** parancsot a parancssori felületén (Bash, Terminálszolgáltatások, parancssort, és így tovább). Például a toorun hello help parancsot, írja be a hello következő:

```azurecli
azure help
```

> [!NOTE]
> Az egyes Linux terjesztéseket, előfordulhat, hogy hibaüzenet hasonló túl`/usr/bin/env: ‘node’: No such file or directory`. Ez a hiba a legújabb telepítendő /usr/bin/nodejs, Node.js-telepítések származik. toofix, a szimbolikus hivatkozást túl/usr/bin/csomópont létrehozása a következő parancs futtatásával:

```bash
sudo ln -s /usr/bin/nodejs /usr/bin/node
```

hello Azure CLI 1.0 telepíti, akkor a következő típus hello toosee hello verziója:

```azurecli
azure --version
```

Most már készen áll! minden tooaccess hello CLI parancsok toowork saját erőforrásokkal [hello Azure CLI Azure-előfizetés tooyour kapcsolódó](xplat-cli-connect.md).

> [!NOTE]
> Első használata alkalmával érdemes az Azure parancssori felület, megjelenik egy üzenet megkérdezi, tooallow Microsoft toocollect használati adatait. Részvétel önkéntes történik. Ha úgy dönt, hogy tooparticipate, le is bármikor futtatásával `azure telemetry --disable`. bármikor, tooenable részvételét futtassa `azure telemetry --enable`.

## <a name="update-hello-cli"></a>Frissítés hello parancssori felület
Microsoft gyakran kiadott hello Azure CLI frissített verzióit. Telepítse újra a CLI hello installer használatával az operációs rendszer hello, vagy futtassa az hello legújabb Docker-tároló. Vagy, ha legújabb Node.js és npm telepítve rendelkezik hello, frissítse a következő hello (a Linux terjesztéseket szükség lehet a toouse **sudo**).

```bash
npm update -g azure-cli
```

## <a name="enable-tab-completion"></a>Kiegészítés engedélyezése
A parancssori felület parancsait kiegészítést Mac és Linux esetén támogatott.

tooenable zsh, a futtatni:

```bash
echo '. <(azure --completion)' >> .zshrc
```

tooenable a bash futtatni:

```bash
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.bash_profile
```


## <a name="next-steps"></a>Következő lépések
* [Csatlakoztatja a hello CLI tooyour Azure-előfizetés](xplat-cli-connect.md) toocreate és az Azure-erőforrások kezeléséhez.
* toolearn hello Azure parancssori Felülettel kapcsolatos további információkért töltse le a forráskódot, jelentse az esetleges problémákat, vagy toohello projekt közre, látogasson el a hello [hello Azure CLI GitHub-tárházban](https://github.com/azure/azure-xplat-cli).
* Ha az Azure parancssori felület hello, vagy Azure használatával kapcsolatos kérdésekre, keresse fel a hello [Azure fórumok](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurescripting).


[mac-installer]: http://aka.ms/mac-azure-cli
[windows-installer]: http://aka.ms/webpi-azure-cli
[linux-installer]: http://aka.ms/linux-azure-cli
[cliasm]: /cli/azure/get-started-with-az-cli2
[cliarm]: ./virtual-machines/azure-cli-arm-commands.md
