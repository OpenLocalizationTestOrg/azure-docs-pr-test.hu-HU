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
# <a name="download-hello-azure-sdk-for-php"></a><span data-ttu-id="34f33-103">Php-hez tartozó hello Azure SDK letöltése</span><span class="sxs-lookup"><span data-stu-id="34f33-103">Download hello Azure SDK for PHP</span></span>
## <a name="overview"></a><span data-ttu-id="34f33-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="34f33-104">Overview</span></span>
<span data-ttu-id="34f33-105">hello Azure SDK for PHP összetevői, amelyek lehetővé teszik toodevelop, telepítéséhez és felügyeletéhez a PHP-alkalmazások az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="34f33-105">hello Azure SDK for PHP includes components that allow you toodevelop, deploy, and manage PHP applications for Azure.</span></span> <span data-ttu-id="34f33-106">Hello Azure SDK for PHP konkrétan az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="34f33-106">Specifically, hello Azure SDK for PHP includes hello following:</span></span>

* <span data-ttu-id="34f33-107">**PHP-ügyfél függvénytárainak hello Azure**.</span><span class="sxs-lookup"><span data-stu-id="34f33-107">**hello PHP client libraries for Azure**.</span></span> <span data-ttu-id="34f33-108">Ezek osztálykönyvtárakhoz egy felületet biztosít az Azure-szolgáltatások, például az adatok szolgáltatások eléréséhez, és a felhőalapú szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="34f33-108">These class libraries provide an interface for accessing Azure features, such as data management services and cloud services.</span></span>  
* <span data-ttu-id="34f33-109">**hello Azure parancssori felület Mac, Linux és Windows (Azure CLI)**.</span><span class="sxs-lookup"><span data-stu-id="34f33-109">**hello Azure Command-Line Interface for Mac, Linux, and Windows (Azure CLI)**.</span></span> <span data-ttu-id="34f33-110">Ez a központi telepítésére és felügyeletére az Azure-szolgáltatások, például az Azure Websites és az Azure virtuális gépek parancsokat.</span><span class="sxs-lookup"><span data-stu-id="34f33-110">This is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="34f33-111">az Azure parancssori felület munkahelyi bármilyen platformon, beleértve a Mac, Linux és a Windows hello.</span><span class="sxs-lookup"><span data-stu-id="34f33-111">hello Azure CLI work on any platform, including Mac, Linux, and Windows.</span></span>
* <span data-ttu-id="34f33-112">**(Csak Windows) Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="34f33-112">**Azure PowerShell (Windows Only)**.</span></span> <span data-ttu-id="34f33-113">Ez olyan PowerShell-parancsmagok telepítése és kezelése az Azure-szolgáltatások, például a Cloud Services és a virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="34f33-113">This is a set of PowerShell cmdlets for deploying and managing Azure Services, such as Cloud Services and Virtual Machines.</span></span>
* <span data-ttu-id="34f33-114">**(csak Windows) Azure-emulátor hello**.</span><span class="sxs-lookup"><span data-stu-id="34f33-114">**hello Azure Emulators (Windows Only)**.</span></span> <span data-ttu-id="34f33-115">hello számítási és tárolási emulátorok felhőszolgáltatások és adatok szolgáltatások, amelyek lehetővé teszik az alkalmazás helyi tootest helyi emulátorok.</span><span class="sxs-lookup"><span data-stu-id="34f33-115">hello compute and storage emulators are local emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="34f33-116">hello Azure emulátorok csak futtassa a Windows.</span><span class="sxs-lookup"><span data-stu-id="34f33-116">hello Azure Emulators run on Windows only.</span></span>

<span data-ttu-id="34f33-117">hello alábbiakban megtudhatja, hogyan toodownload és a telepítés hello fenti összetevők.</span><span class="sxs-lookup"><span data-stu-id="34f33-117">hello sections below describe how toodownload and install hello components described above.</span></span>

<span data-ttu-id="34f33-118">hello ebben a témakörben található utasítások azt feltételezik, hogy rendelkezik [PHP] [ install-php] telepítve.</span><span class="sxs-lookup"><span data-stu-id="34f33-118">hello instructions in this topic assume that you have [PHP][install-php] installed.</span></span>

> [!NOTE]
> <span data-ttu-id="34f33-119">Az Azure PHP 5.5 vagy magasabb toouse hello PHP klienskódtárak kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="34f33-119">You must have PHP 5.5 or higher toouse hello PHP client libraries for Azure.</span></span>
> 
> 

## <a name="php-client-libraries-for-azure"></a><span data-ttu-id="34f33-120">PHP-klienskódtárak Azure-hoz</span><span class="sxs-lookup"><span data-stu-id="34f33-120">PHP client libraries for Azure</span></span>
<span data-ttu-id="34f33-121">hello PHP Klienskódtárak segítségével az Azure-bA egy felületet biztosít az Azure-szolgáltatások, például az adatok szolgáltatások eléréséhez, és a felhőalapú szolgáltatások, az operációs rendszert.</span><span class="sxs-lookup"><span data-stu-id="34f33-121">hello PHP Client Libraries for Azure provide an interface for accessing Azure features, such as data management services and cloud services, from any operating system.</span></span> <span data-ttu-id="34f33-122">Ezek a kódtárak hello szerkesztő használatával is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="34f33-122">These libraries can be installed via hello Composer.</span></span>

<span data-ttu-id="34f33-123">Hogyan toouse hello Azure PHP-ügyfél könyvtárakban kapcsolatos információkért lásd: [hogyan tooUse hello Blob szolgáltatás][blob-service], [hogyan tooUse hello Table szolgáltatás] [ table-service] és [hogyan tooUse hello Queue szolgáltatás][queue-service].</span><span class="sxs-lookup"><span data-stu-id="34f33-123">For information about how toouse hello PHP Client Libraries for Azure, see [How tooUse hello Blob Service][blob-service], [How tooUse hello Table Service][table-service] and [How tooUse hello Queue Service][queue-service].</span></span>

### <a name="install-via-composer"></a><span data-ttu-id="34f33-124">Keresztül szerkesztő telepítése</span><span class="sxs-lookup"><span data-stu-id="34f33-124">Install via Composer</span></span>
1. <span data-ttu-id="34f33-125">[Telepítse a Git][install-git].</span><span class="sxs-lookup"><span data-stu-id="34f33-125">[Install Git][install-git].</span></span>

    > [AZURE.NOTE] <span data-ttu-id="34f33-126">A Windows rendszeren is szüksége lesz tooadd hello Git végrehajtható tooyour PATH környezeti változóhoz.</span><span class="sxs-lookup"><span data-stu-id="34f33-126">On Windows, you will also need tooadd hello Git executable tooyour PATH environment variable.</span></span>

1. <span data-ttu-id="34f33-127">Hozzon létre egy fájlt **composer.json** a hello a projekt gyökérkönyvtárában, és adja hozzá a következő kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="34f33-127">Create a file named **composer.json** in hello root of your project and add hello following code tooit:</span></span>
   
        {
            "require": {
                "microsoft/windowsazure": "^0.4"
            }
        }
2. <span data-ttu-id="34f33-128">Töltse le  **[composer.phar] [ composer-phar]**  a projekt gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="34f33-128">Download **[composer.phar][composer-phar]** in your project root.</span></span>
3. <span data-ttu-id="34f33-129">Nyisson meg egy parancssort, és hajtsa végre ezt a projekt legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="34f33-129">Open a command prompt and execute this in your project root</span></span>
   
        php composer.phar install

## <a name="azure-powershell-and-azure-emulators"></a><span data-ttu-id="34f33-130">Az Azure PowerShell és az Azure emulátorok</span><span class="sxs-lookup"><span data-stu-id="34f33-130">Azure PowerShell and Azure Emulators</span></span>
<span data-ttu-id="34f33-131">Az Azure PowerShell olyan PowerShell-parancsmagok telepítése és kezelése az Azure-szolgáltatások (például a Felhőszolgáltatások és virtuális gépek).</span><span class="sxs-lookup"><span data-stu-id="34f33-131">Azure PowerShell is a set of PowerShell cmdlets for deploying and managing Azure Services (such as Cloud Services and Virtual Machines).</span></span> <span data-ttu-id="34f33-132">hello Azure emulátorok emulátorok felhőszolgáltatások és adatok szolgáltatások, amelyek lehetővé teszik az alkalmazás helyi tootest.</span><span class="sxs-lookup"><span data-stu-id="34f33-132">hello Azure Emulators are emulators of cloud services and data management services that allow you tootest an application locally.</span></span> <span data-ttu-id="34f33-133">Ezek az összetevők támogatottak csak Windows.</span><span class="sxs-lookup"><span data-stu-id="34f33-133">These components are supported Windows only.</span></span>

<span data-ttu-id="34f33-134">ajánlott tooinstall Azure PowerShell hello és hello Azure emulátorok toouse hello [Microsoft Webplatform-telepítő][download-wpi].</span><span class="sxs-lookup"><span data-stu-id="34f33-134">hello recommended way tooinstall Azure PowerShell and hello Azure Emulators is toouse hello [Microsoft Web Platform Installer][download-wpi].</span></span> <span data-ttu-id="34f33-135">Vegye figyelembe, hogy is beállíthatja tooinstall további fejlesztési összetevők, például a PHP, SQL Server, SQL Server és a WebMatrix hello Microsoft Drivers.</span><span class="sxs-lookup"><span data-stu-id="34f33-135">Note that you can also choose tooinstall other development components, such as PHP, SQL Server, hello Microsoft Drivers for SQL Server for PHP, and WebMatrix.</span></span>

<span data-ttu-id="34f33-136">További információ az Azure PowerShell toouse lásd: [hogyan tooUse Azure PowerShell][powershell-tools].</span><span class="sxs-lookup"><span data-stu-id="34f33-136">For information about how toouse Azure PowerShell, see [How tooUse Azure PowerShell][powershell-tools].</span></span>

## <a name="azure-cli"></a><span data-ttu-id="34f33-137">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="34f33-137">Azure CLI</span></span>
<span data-ttu-id="34f33-138">hello Azure CLI telepítése és kezelése az Azure-szolgáltatások, például az Azure Websites és az Azure virtuális gépek parancsokat.</span><span class="sxs-lookup"><span data-stu-id="34f33-138">hello Azure CLI is a set of commands for deploying and managing Azure services, such as Azure Websites and Azure Virtual Machines.</span></span> <span data-ttu-id="34f33-139">Az Azure parancssori felület telepítésével kapcsolatos információkért lásd: [telepítés hello Azure CLI](cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="34f33-139">For information about installing Azure CLI, see [Install hello Azure CLI](cli-install-nodejs.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="34f33-140">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34f33-140">Next steps</span></span>
<span data-ttu-id="34f33-141">További információkért lásd: hello [PHP fejlesztői központ](/develop/php/).</span><span class="sxs-lookup"><span data-stu-id="34f33-141">For more information, see hello [PHP Developer Center](/develop/php/).</span></span>

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
