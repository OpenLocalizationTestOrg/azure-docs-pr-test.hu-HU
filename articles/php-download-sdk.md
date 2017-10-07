---
title: aaaDownload hello Azure SDK for PHP
description: "Ismerje meg, hogyan toodownload és a telepítés hello Azure SDK-t a PHP."
documentationcenter: php
services: app-service\web
author: allclark
manager: douge
editor: 
ms.assetid: bac355ac-4c25-42f4-8273-c5112eafa8d4
ms.service: app-service-web
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 06/01/2016
ms.author: allclark;yaqiyang
ms.openlocfilehash: 94f56fc4f91bb175c08b9f7a43cb221c827694a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="download-hello-azure-sdk-for-php"></a>Php-hez tartozó hello Azure SDK letöltése
## <a name="overview"></a>Áttekintés
hello Azure SDK for PHP összetevői, amelyek lehetővé teszik toodevelop, telepítéséhez és felügyeletéhez a PHP-alkalmazások az Azure-bA. Hello Azure SDK for PHP konkrétan az alábbi hello:

* **PHP-ügyfél függvénytárainak hello Azure**. Ezek osztálykönyvtárakhoz egy felületet biztosít az Azure-szolgáltatások, például az adatok szolgáltatások eléréséhez, és a felhőalapú szolgáltatások.  
* **hello Azure parancssori felület Mac, Linux és Windows (Azure CLI)**. Ez a központi telepítésére és felügyeletére az Azure-szolgáltatások, például az Azure Websites és az Azure virtuális gépek parancsokat. az Azure parancssori felület munkahelyi bármilyen platformon, beleértve a Mac, Linux és a Windows hello.
* **(Csak Windows) Azure PowerShell**. Ez olyan PowerShell-parancsmagok telepítése és kezelése az Azure-szolgáltatások, például a Cloud Services és a virtuális gépek.
* **(csak Windows) Azure-emulátor hello**. hello számítási és tárolási emulátorok felhőszolgáltatások és adatok szolgáltatások, amelyek lehetővé teszik az alkalmazás helyi tootest helyi emulátorok. hello Azure emulátorok csak futtassa a Windows.

hello alábbiakban megtudhatja, hogyan toodownload és a telepítés hello fenti összetevők.

hello ebben a témakörben található utasítások azt feltételezik, hogy rendelkezik [PHP] [ install-php] telepítve.

> [!NOTE]
> Az Azure PHP 5.5 vagy magasabb toouse hello PHP klienskódtárak kell rendelkeznie.
> 
> 

## <a name="php-client-libraries-for-azure"></a>PHP-klienskódtárak Azure-hoz
hello PHP Klienskódtárak segítségével az Azure-bA egy felületet biztosít az Azure-szolgáltatások, például az adatok szolgáltatások eléréséhez, és a felhőalapú szolgáltatások, az operációs rendszert. Ezek a kódtárak hello szerkesztő használatával is telepíthető.

Hogyan toouse hello Azure PHP-ügyfél könyvtárakban kapcsolatos információkért lásd: [hogyan tooUse hello Blob szolgáltatás][blob-service], [hogyan tooUse hello Table szolgáltatás] [ table-service] és [hogyan tooUse hello Queue szolgáltatás][queue-service].

### <a name="install-via-composer"></a>Keresztül szerkesztő telepítése
1. [Telepítse a Git][install-git].

    > [AZURE.NOTE] A Windows rendszeren is szüksége lesz tooadd hello Git végrehajtható tooyour PATH környezeti változóhoz.

1. Hozzon létre egy fájlt **composer.json** a hello a projekt gyökérkönyvtárában, és adja hozzá a következő kód tooit hello:
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. Töltse le  **[composer.phar] [ composer-phar]**  a projekt gyökérkönyvtárában.
3. Nyisson meg egy parancssort, és hajtsa végre ezt a projekt legfelső szintű
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a>Az Azure PowerShell és az Azure emulátorok
Az Azure PowerShell olyan PowerShell-parancsmagok telepítése és kezelése az Azure-szolgáltatások (például a Felhőszolgáltatások és virtuális gépek). hello Azure emulátorok emulátorok felhőszolgáltatások és adatok szolgáltatások, amelyek lehetővé teszik az alkalmazás helyi tootest. Ezek az összetevők támogatottak csak Windows.

ajánlott tooinstall Azure PowerShell hello és hello Azure emulátorok toouse hello [Microsoft Webplatform-telepítő][download-wpi]. Vegye figyelembe, hogy is beállíthatja tooinstall további fejlesztési összetevők, például a PHP, SQL Server, SQL Server és a WebMatrix hello Microsoft Drivers.

További információ az Azure PowerShell toouse lásd: [hogyan tooUse Azure PowerShell][powershell-tools].

## <a name="azure-cli"></a>Azure CLI
hello Azure CLI telepítése és kezelése az Azure-szolgáltatások, például az Azure Websites és az Azure virtuális gépek parancsokat. Az Azure parancssori felület telepítésével kapcsolatos információkért lásd: [telepítés hello Azure CLI](cli-install-nodejs.md).

## <a name="next-steps"></a>Következő lépések
További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).

[install-php]: http://www.php.net/manual/en/install.php
[composer-github]: https://github.com/composer/composer
[composer-phar]: http://getcomposer.org/composer.phar
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[download-wpi]: http://go.microsoft.com/fwlink/?LinkId=253447
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[blob-service]: http://go.microsoft.com/fwlink/?LinkId=252714
[table-service]: http://go.microsoft.com/fwlink/?LinkId=252715
[queue-service]: http://go.microsoft.com/fwlink/?LinkId=252716
[azure cli]: http://go.microsoft.com/fwlink/?LinkId=252717
[powershell-tools]: http://go.microsoft.com/fwlink/?LinkId=252718
[php-sdk-github]: http://go.microsoft.com/fwlink/?LinkId=252719
[install-git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
