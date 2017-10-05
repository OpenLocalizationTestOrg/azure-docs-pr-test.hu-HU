---
title: "Python és az SDK - Azure telepítéséhez"
description: "Megtudhatja, hogyan telepítse a Python és az SDK az Azure-ral használatára."
services: 
documentationcenter: python
author: lmazuel
manager: wpickett
editor: 
ms.assetid: f36294be-daeb-4caf-9129-fce18130f552
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 09/06/2016
ms.author: lmazuel
ms.openlocfilehash: c9df4e1f7677b2ed10684f6f3c981f2abf64f171
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="installing-python-and-the-sdk"></a><span data-ttu-id="997c9-103">Python és az SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="997c9-103">Installing Python and the SDK</span></span>
<span data-ttu-id="997c9-104">Python egyszerűen állítsa be a Windows és a Mac, Linux, előre előre telepített és [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="997c9-104">Python is easy to set up on Windows and comes pre-installed on Mac, Linux, and [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span> <span data-ttu-id="997c9-105">Ez az útmutató bemutatja, hogyan telepítési és a gép az Azure-ral használatra kész beolvasása.</span><span class="sxs-lookup"><span data-stu-id="997c9-105">This guide walks you through installation and getting your machine ready for use with Azure.</span></span>

## <a name="whats-in-the-python-azure-sdk"></a><span data-ttu-id="997c9-106">Mi a Python az Azure SDK?</span><span class="sxs-lookup"><span data-stu-id="997c9-106">What's in the Python Azure SDK?</span></span>
<span data-ttu-id="997c9-107">Az Azure SDK for Python tartalmaz, amelyek lehetővé teszik a fejlesztés, telepítéséhez és felügyeletéhez a Python-alkalmazások az Azure-összetevők.</span><span class="sxs-lookup"><span data-stu-id="997c9-107">The Azure SDK for Python includes components that allow you to develop, deploy, and manage Python applications for Azure.</span></span> <span data-ttu-id="997c9-108">Az Azure SDK for Python konkrétan a következő:</span><span class="sxs-lookup"><span data-stu-id="997c9-108">Specifically, the Azure SDK for Python includes the following:</span></span>

* <span data-ttu-id="997c9-109">**Kezelési kódtárakat**.</span><span class="sxs-lookup"><span data-stu-id="997c9-109">**Management libraries**.</span></span> <span data-ttu-id="997c9-110">Ezek osztálykönyvtárakhoz biztosít egy Azure-erőforrások, például a tárfiókokat, virtuális gépek kezelése felületet.</span><span class="sxs-lookup"><span data-stu-id="997c9-110">These class libraries provide an interface managing Azure resources, such as storage accounts, virtual machines.</span></span>
* <span data-ttu-id="997c9-111">**Végrehajtási könyvtárak**.</span><span class="sxs-lookup"><span data-stu-id="997c9-111">**Runtime libraries**.</span></span> <span data-ttu-id="997c9-112">Ezek osztálykönyvtárakhoz tevő interfészt biztosítanak az Azure-szolgáltatások, például a tárolás és a service bus eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="997c9-112">These class libraries provide an interface for accessing Azure features, such as storage and service bus.</span></span>

## <a name="which-python-and-which-version-to-use"></a><span data-ttu-id="997c9-113">Mely Python és melyik verziót kell használni</span><span class="sxs-lookup"><span data-stu-id="997c9-113">Which Python and which version to use</span></span>
<span data-ttu-id="997c9-114">Nincsenek elérhető Python tolmácsoknak több verziója – például:</span><span class="sxs-lookup"><span data-stu-id="997c9-114">There are several flavors of Python interpreters available - examples include:</span></span>

* <span data-ttu-id="997c9-115">CPython - a standard és a leggyakrabban használt Python értelmező</span><span class="sxs-lookup"><span data-stu-id="997c9-115">CPython - the standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="997c9-116">PyPy - gyors, megfelelő alternatív megvalósítását, hogy CPython</span><span class="sxs-lookup"><span data-stu-id="997c9-116">PyPy - fast, compliant alternative implementation to CPython</span></span>
* <span data-ttu-id="997c9-117">IronPython - .net/CLR futó Python parancsértelmező</span><span class="sxs-lookup"><span data-stu-id="997c9-117">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="997c9-118">Jython - Python parancsértelmező, amely a Java virtuális gép fut</span><span class="sxs-lookup"><span data-stu-id="997c9-118">Jython - Python interpreter that runs on the Java Virtual Machine</span></span>

<span data-ttu-id="997c9-119">**CPython** v2.7 vagy v3.3 + és PyPy 5.4.0 tesztelése, és az Azure Python SDK támogatott.</span><span class="sxs-lookup"><span data-stu-id="997c9-119">**CPython** v2.7 or v3.3+ and PyPy 5.4.0 are tested and supported for the Python Azure SDK.</span></span>

## <a name="where-to-get-python"></a><span data-ttu-id="997c9-120">Honnan szerezhetők be a Python?</span><span class="sxs-lookup"><span data-stu-id="997c9-120">Where to get Python?</span></span>
<span data-ttu-id="997c9-121">Több módon is CPython beolvasása:</span><span class="sxs-lookup"><span data-stu-id="997c9-121">There are several ways to get CPython:</span></span>

* <span data-ttu-id="997c9-122">Közvetlenül a [www.python.org][www.python.org]</span><span class="sxs-lookup"><span data-stu-id="997c9-122">Directly from [www.python.org][www.python.org]</span></span>
* <span data-ttu-id="997c9-123">Egy megbízható distro többek között a [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] vagy [www.activestate.com][www.activestate.com]</span><span class="sxs-lookup"><span data-stu-id="997c9-123">From a reputable distro such as [www.continuum.io][www.continuum.io], [www.enthought.com][www.enthought.com] or [www.activestate.com][www.activestate.com]</span></span>
* <span data-ttu-id="997c9-124">Build forrásból!</span><span class="sxs-lookup"><span data-stu-id="997c9-124">Build from source!</span></span>

<span data-ttu-id="997c9-125">Ha egy konkrét OK, ajánlott az első két lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="997c9-125">Unless you have a specific need, we recommend the first two options.</span></span>

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a><span data-ttu-id="997c9-126">A Windows, Linux és MacOS (csak az ügyféloldali kódtáraknál) SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="997c9-126">SDK Installation on Windows, Linux, and MacOS (client libraries only)</span></span>
<span data-ttu-id="997c9-127">Ha már rendelkezik telepített Python, a pip használatával egy csomagot az összes könyvtár fanézetét az ügyfél telepíti a meglévő Python 2.7-es vagy 3.3-as + Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="997c9-127">If you already have Python installed, you can use pip to install a bundle of all the client libraries in your existing Python 2.7 or Python 3.3+ environment.</span></span> <span data-ttu-id="997c9-128">Ezzel letölti a csomagot a [Python-Csomagindexet] [ Python Package Index] (PyPI).</span><span class="sxs-lookup"><span data-stu-id="997c9-128">This downloads the packages from the [Python Package Index][Python Package Index] (PyPI).</span></span>

<span data-ttu-id="997c9-129">Rendszergazdai jogosultsággal is kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="997c9-129">You may need administrator rights:</span></span>

* <span data-ttu-id="997c9-130">Linux- és MacOS, használja a `sudo` parancs: `sudo pip install azure-mgmt-compute`.</span><span class="sxs-lookup"><span data-stu-id="997c9-130">Linux and MacOS, use the `sudo` command: `sudo pip install azure-mgmt-compute`.</span></span>
* <span data-ttu-id="997c9-131">Windows: Nyissa meg a PowerShell vagy parancssor rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="997c9-131">Windows: open your PowerShell/Command prompt as an administrator</span></span>

<span data-ttu-id="997c9-132">Minden egyes Azure szolgáltatás külön-külön egyes tárak telepítése:</span><span class="sxs-lookup"><span data-stu-id="997c9-132">You can install individually each library for each Azure service:</span></span>

```console
   $ pip install azure-batch          # Install the latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install the latest Storage management library
```

<span data-ttu-id="997c9-133">Előzetes csomagokat telepítheti a `--pre` jelző:</span><span class="sxs-lookup"><span data-stu-id="997c9-133">Preview packages can be installed using the `--pre` flag:</span></span>

```console
   $ pip install --pre azure-mgmt-compute # installs only the latest Compute Management library
```

<span data-ttu-id="997c9-134">Azure szalagtárak készlete egy egysoros használatával is telepíthet a `azure` meta-csomag.</span><span class="sxs-lookup"><span data-stu-id="997c9-134">You can also install a set of Azure libraries in a single line using the `azure` meta-package.</span></span> <span data-ttu-id="997c9-135">Mivel a metaadat-csomag nem az összes csomag táblaként vannak közzétéve stabil, a `azure` meta-csomag még előzetes verzió van.</span><span class="sxs-lookup"><span data-stu-id="997c9-135">Since not all packages in this meta-package are published as stable yet, the `azure` meta-package is still in preview.</span></span>
<span data-ttu-id="997c9-136">Azonban a fő csomagoknak kód minőségi/teljességét nézőpontból is tekinthető "stable" most</span><span class="sxs-lookup"><span data-stu-id="997c9-136">However, the core packages, from code quality/completeness perspectives can be considered "stable" at this time</span></span>

* <span data-ttu-id="997c9-137">hivatalos címkéjén ilyen szinkronban más nyelvek lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="997c9-137">it is officially labeled as such in sync with other languages as soon as possible.</span></span>
  <span data-ttu-id="997c9-138">Jelenleg nem tervezi, addig a további lényegesen módosul.</span><span class="sxs-lookup"><span data-stu-id="997c9-138">We are not planning on any further major changes until then.</span></span>

<span data-ttu-id="997c9-139">Mivel ez egy előzetes, szeretné-e használni a `--pre` jelző:</span><span class="sxs-lookup"><span data-stu-id="997c9-139">Since it's a preview release, you need to use the `--pre` flag:</span></span>

```console
   $ pip install --pre azure
```

<span data-ttu-id="997c9-140">vagy közvetlenül</span><span class="sxs-lookup"><span data-stu-id="997c9-140">or directly</span></span>

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a><span data-ttu-id="997c9-141">További csomagok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="997c9-141">Getting More Packages</span></span>
<span data-ttu-id="997c9-142">A [Python-Csomagindexet] [ Python Package Index] (PyPI) rendelkezik a gazdag kijelölt Python szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="997c9-142">The [Python Package Index][Python Package Index] (PyPI) has a rich selection of Python libraries.</span></span>  <span data-ttu-id="997c9-143">Ha egy Distro telepítését választotta, az érdekes bit, az a műszaki számítástechnikai webes fejlesztési különböző lehetőségeket többsége már összekapcsolta.</span><span class="sxs-lookup"><span data-stu-id="997c9-143">If you chose to install a Distro, you'll already have most of the interesting bits for various scenarios from web development to Technical Computing.</span></span>

## <a name="python-tools-for-visual-studio"></a><span data-ttu-id="997c9-144">Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="997c9-144">Python Tools for Visual Studio</span></span>
<span data-ttu-id="997c9-145">[A Python Tools for Visual Studio][a Python Tools for Visual Studio] (PTVS) egy ingyenes/OSS beépülő modul Microsoft Visual STUDIO alakítja át a teljes körű Python IDE:</span><span class="sxs-lookup"><span data-stu-id="997c9-145">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) is a free/OSS plugin from Microsoft, which turns VS into a full-fledged Python IDE:</span></span>

![How-to-telepítés – python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

<span data-ttu-id="997c9-147">PTVS használata nem kötelező, de ajánlott, mivel biztosít Python és a webes projekt/megoldás támogatja, hibakeresés, profilkészítési, interaktív ablak, Sablonszerkesztés és Intellisense.</span><span class="sxs-lookup"><span data-stu-id="997c9-147">Using PTVS is optional, but is recommended as it gives you Python and Web Project/Solution support, debugging, profiling, interactive window, Template editing, and Intellisense.</span></span>

<span data-ttu-id="997c9-148">PTVS is megkönnyíti a Microsoft Azure-támogatással történő központi telepítéséhez [Felhőszolgáltatások](cloud-services/cloud-services-python-ptvs.md) és [webhelyek](app-service-web/web-sites-python-ptvs-django-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="997c9-148">PTVS also makes it easy to deploy to Microsoft Azure, with support for deployment to [Cloud Services](cloud-services/cloud-services-python-ptvs.md) and [Websites](app-service-web/web-sites-python-ptvs-django-mysql.md).</span></span>

<span data-ttu-id="997c9-149">PTVS a Visual Studio 2013, 2015-öt vagy 2017 példánya működik.</span><span class="sxs-lookup"><span data-stu-id="997c9-149">PTVS works with your existing Visual Studio 2013, 2015, or 2017 installation.</span></span>  <span data-ttu-id="997c9-150">Dokumentáció, letöltéseket és vitafórumok: [a Python Tools for Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="997c9-150">For documentation, downloads and discussions, see [Python Tools for Visual Studio].</span></span>  

## <a name="python-azure-scenarios-for-linux-and-macos"></a><span data-ttu-id="997c9-151">Python Linux és MacOS Azure forgatókönyvei</span><span class="sxs-lookup"><span data-stu-id="997c9-151">Python Azure Scenarios for Linux and MacOS</span></span>
<span data-ttu-id="997c9-152">A Linux vagy MacOS, fő Azure forgatókönyvek támogatottak:</span><span class="sxs-lookup"><span data-stu-id="997c9-152">For Linux or MacOS, main Azure scenarios that are supported:</span></span>

1. <span data-ttu-id="997c9-153">Azure Services fel a klienskódtárak segítségével Python használatával</span><span class="sxs-lookup"><span data-stu-id="997c9-153">Consuming Azure Services by using the client libraries for Python</span></span>
2. <span data-ttu-id="997c9-154">Az alkalmazás futó Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="997c9-154">Running your app in a Linux VM</span></span>
3. <span data-ttu-id="997c9-155">Fejlesztéséhez és a Git használatával Azure Websitesra történő közzététel</span><span class="sxs-lookup"><span data-stu-id="997c9-155">Developing and publishing to Azure Websites using Git</span></span>

<span data-ttu-id="997c9-156">Az első forgatókönyv lehetővé teszi, hogy az Azure PaaS lehetőségeinek kihasználásához, mint ahol webalkalmazások készíthet [blob-tároló](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [várólista tárolási](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) keresztül stb. Pythonic burkolóit a(z) az Azure REST API-k.</span><span class="sxs-lookup"><span data-stu-id="997c9-156">The first scenario enables you to author rich web apps that take advantage of the Azure PaaS capabilities such as [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [queue storage](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic wrappers for the Azure REST APIs.</span></span> <span data-ttu-id="997c9-157">Ezek azonos Windows, Mac és Linux rendszeren működik.</span><span class="sxs-lookup"><span data-stu-id="997c9-157">These work identically on Windows, Mac, and Linux.</span></span>  <span data-ttu-id="997c9-158">A helyi fejlesztési számítógépén vagy az Azure-on futó Linux virtuális gép is használhatja a ügyfél tárak.</span><span class="sxs-lookup"><span data-stu-id="997c9-158">You can also use these client libraries from your local development machine or a Linux VM running on Azure.</span></span>

<span data-ttu-id="997c9-159">A virtuális gép esetben egyszerűen indítsa el a Linux virtuális gép a kiválasztott (Ubuntu, CentOS, Suse), és futtassa, felügyeletéhez, például.</span><span class="sxs-lookup"><span data-stu-id="997c9-159">For the VM scenario, you simply start a Linux VM of your choice (Ubuntu, CentOS, Suse) and run/manage what you like.</span></span>  <span data-ttu-id="997c9-160">Például futtathatja [IPython] [ IPython] REPL/notebook a Windows vagy Mac/Linux számítógép- és Linux vagy a IPython motor Azure-on futó Windows multi-proc VM irányítsa a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="997c9-160">As an example, you can run [IPython][IPython] REPL/notebook on your Windows/Mac/Linux machine and point your browser to a Linux or Windows multi-proc VM running the IPython Engine on Azure.</span></span>

<span data-ttu-id="997c9-161">Linux virtuális gépet beállításával kapcsolatos információkért lásd: a [hozzon létre egy virtuális gép futó Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="997c9-161">For information on how to set up a Linux VM, see the [Create a Virtual Machine Running Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tutorial.</span></span>

<span data-ttu-id="997c9-162">Git üzemelő példányt használ, a Python webalkalmazás fejlesztése és az operációs rendszert egy Azure-webhelyre történő közzétételhez.</span><span class="sxs-lookup"><span data-stu-id="997c9-162">Using Git deployment, you can develop a Python web application and publish it to an Azure Website from any operating system.</span></span>  <span data-ttu-id="997c9-163">Amikor a tárházat az Azure-ba, automatikusan létrehoz egy virtuális környezethez, és a pip telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="997c9-163">When you push your repository to Azure, it automatically creates a virtual environment and pip installs your required packages.</span></span>

<span data-ttu-id="997c9-164">Kifejlesztett és közzététele az Azure Websitesra további információkért lásd: az oktatóanyag [webhelyek létrehozása a djangóval](app-service-web/web-sites-python-create-deploy-django-app.md), [Bottle webhelyek létrehozásának](app-service-web/web-sites-python-create-deploy-bottle-app.md), és [webhelyek létrehozása Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span><span class="sxs-lookup"><span data-stu-id="997c9-164">For more information on developing and publishing Azure Websites, see the tutorials for [Creating Websites with Django](app-service-web/web-sites-python-create-deploy-django-app.md), [Creating Websites with Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), and [Creating Websites with Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span></span> <span data-ttu-id="997c9-165">Bármely WSGI-kompatibilis keretrendszer használatával további általános információkért lásd: [Python konfigurálása az Azure Websitesra](app-service-web/web-sites-python-configure.md).</span><span class="sxs-lookup"><span data-stu-id="997c9-165">For more general information on using any WSGI-compliant framework, see [Configuring Python with Azure Websites](app-service-web/web-sites-python-configure.md).</span></span>

## <a name="additional-software-and-resources"></a><span data-ttu-id="997c9-166">További szoftverek és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="997c9-166">Additional Software and Resources:</span></span>
* [<span data-ttu-id="997c9-167">Python ReadTheDocs készült Azure SDK</span><span class="sxs-lookup"><span data-stu-id="997c9-167">Azure SDK for Python ReadTheDocs</span></span>](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [<span data-ttu-id="997c9-168">A Python github használatára az Azure SDK</span><span class="sxs-lookup"><span data-stu-id="997c9-168">Azure SDK for Python GitHub</span></span>](https://github.com/Azure/azure-sdk-for-python)
* [<span data-ttu-id="997c9-169">Python hivatalos Azure-minták</span><span class="sxs-lookup"><span data-stu-id="997c9-169">Official Azure samples for Python</span></span>](https://azure.microsoft.com/documentation/samples/?platform=python)
* <span data-ttu-id="997c9-170">[Alakíthatnak ki olyan Analytics Python elosztási][Continuum Analytics Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="997c9-170">[Continuum Analytics Python Distribution][Continuum Analytics Python Distribution]</span></span>
* <span data-ttu-id="997c9-171">[Enthought Python elosztási][Enthought Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="997c9-171">[Enthought Python Distribution][Enthought Python Distribution]</span></span>
* <span data-ttu-id="997c9-172">[ActiveState Python elosztási][ActiveState Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="997c9-172">[ActiveState Python Distribution][ActiveState Python Distribution]</span></span>
* <span data-ttu-id="997c9-173">[SciPy - tudományos Python szalagtárak együttese][SciPy - A suite of Scientific Python libraries]</span><span class="sxs-lookup"><span data-stu-id="997c9-173">[SciPy - A suite of Scientific Python libraries][SciPy - A suite of Scientific Python libraries]</span></span>
* <span data-ttu-id="997c9-174">[NumPy - írhatók kódtára a Pythonhoz][NumPy - A numerics library for Python]</span><span class="sxs-lookup"><span data-stu-id="997c9-174">[NumPy - A numerics library for Python][NumPy - A numerics library for Python]</span></span>
* <span data-ttu-id="997c9-175">[A Django-projekt - érett webes keretrendszer/CMS][Django Project - A mature web framework/CMS]</span><span class="sxs-lookup"><span data-stu-id="997c9-175">[Django Project - A mature web framework/CMS][Django Project - A mature web framework/CMS]</span></span>
* <span data-ttu-id="997c9-176">[IPython – egy speciális REPL/Notebook Python-hez][IPython - an advanced REPL/Notebook for Python]</span><span class="sxs-lookup"><span data-stu-id="997c9-176">[IPython - an advanced REPL/Notebook for Python][IPython - an advanced REPL/Notebook for Python]</span></span>
* <span data-ttu-id="997c9-177">[A Python Tools for Visual Studio a Githubon][Python Tools for Visual Studio on GitHub]</span><span class="sxs-lookup"><span data-stu-id="997c9-177">[Python Tools for Visual Studio on GitHub][Python Tools for Visual Studio on GitHub]</span></span>
* [<span data-ttu-id="997c9-178">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="997c9-178">Python Developer Center</span></span>](/develop/python/)

[Continuum Analytics Python Distribution]: http://continuum.io
[Enthought Python Distribution]: http://www.enthought.com
[ActiveState Python Distribution]: http://www.activestate.com
[www.python.org]: http://www.python.org
[www.continuum.io]: http://continuum.io
[www.enthought.com]: http://www.enthought.com
[www.activestate.com]: http://www.activestate.com
[SciPy - A suite of Scientific Python libraries]: http://www.scipy.org
[NumPy - A numerics library for Python]: http://www.numpy.org
[Django Project - A mature web framework/CMS]: http://www.djangoproject.com
[IPython - an advanced REPL/Notebook for Python]: http://ipython.org
[IPython]: http://ipython.org
[IPython Notebook on Azure]: virtual-machines-linux-jupyter-notebook.md
[Cloud Services]: cloud-services-python-ptvs.md
[Websites]: web-sites-python-ptvs-django-mysql.md
[a Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools for Visual Studio on GitHub]: https://github.com/microsoft/ptvs
[Python Package Index]: http://pypi.python.org/pypi
[Microsoft Azure SDK for Python 2.7]: http://go.microsoft.com/fwlink/?LinkId=254281
[Microsoft Azure SDK for Python 3.4]: http://go.microsoft.com/fwlink/?LinkID=516990
[Setting up a Linux VM via the Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How to use the Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
