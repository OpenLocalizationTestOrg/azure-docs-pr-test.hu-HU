---
title: "aaaInstall Python és az Azure SDK - hello"
description: "Megtudhatja, hogyan tooinstall Python és hello SDK toouse az Azure-ral."
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
ms.openlocfilehash: c1b394770f9abd3e654a23d79ae179a9af89e2fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-python-and-hello-sdk"></a><span data-ttu-id="9045c-103">Python és hello SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="9045c-103">Installing Python and hello SDK</span></span>
<span data-ttu-id="9045c-104">Python könnyen tooset működik-e a Windows és a Mac, Linux, előre előre telepített és [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span><span class="sxs-lookup"><span data-stu-id="9045c-104">Python is easy tooset up on Windows and comes pre-installed on Mac, Linux, and [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about).</span></span> <span data-ttu-id="9045c-105">Ez az útmutató bemutatja, hogyan telepítési és a gép az Azure-ral használatra kész beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9045c-105">This guide walks you through installation and getting your machine ready for use with Azure.</span></span>

## <a name="whats-in-hello-python-azure-sdk"></a><span data-ttu-id="9045c-106">Mi az a hello Python Azure SDK-t?</span><span class="sxs-lookup"><span data-stu-id="9045c-106">What's in hello Python Azure SDK?</span></span>
<span data-ttu-id="9045c-107">hello Azure SDK for Python összetevői, amelyek lehetővé teszik toodevelop, telepítéséhez és felügyeletéhez a Python-alkalmazások az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="9045c-107">hello Azure SDK for Python includes components that allow you toodevelop, deploy, and manage Python applications for Azure.</span></span> <span data-ttu-id="9045c-108">Hello Azure SDK for Python konkrétan az alábbi hello:</span><span class="sxs-lookup"><span data-stu-id="9045c-108">Specifically, hello Azure SDK for Python includes hello following:</span></span>

* <span data-ttu-id="9045c-109">**Kezelési kódtárakat**.</span><span class="sxs-lookup"><span data-stu-id="9045c-109">**Management libraries**.</span></span> <span data-ttu-id="9045c-110">Ezek osztálykönyvtárakhoz biztosít egy Azure-erőforrások, például a tárfiókokat, virtuális gépek kezelése felületet.</span><span class="sxs-lookup"><span data-stu-id="9045c-110">These class libraries provide an interface managing Azure resources, such as storage accounts, virtual machines.</span></span>
* <span data-ttu-id="9045c-111">**Végrehajtási könyvtárak**.</span><span class="sxs-lookup"><span data-stu-id="9045c-111">**Runtime libraries**.</span></span> <span data-ttu-id="9045c-112">Ezek osztálykönyvtárakhoz tevő interfészt biztosítanak az Azure-szolgáltatások, például a tárolás és a service bus eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="9045c-112">These class libraries provide an interface for accessing Azure features, such as storage and service bus.</span></span>

## <a name="which-python-and-which-version-toouse"></a><span data-ttu-id="9045c-113">Mely Python és melyik verzió toouse</span><span class="sxs-lookup"><span data-stu-id="9045c-113">Which Python and which version toouse</span></span>
<span data-ttu-id="9045c-114">Nincsenek elérhető Python tolmácsoknak több verziója – például:</span><span class="sxs-lookup"><span data-stu-id="9045c-114">There are several flavors of Python interpreters available - examples include:</span></span>

* <span data-ttu-id="9045c-115">CPython - hello standard és a leggyakrabban használt Python parancsértelmező</span><span class="sxs-lookup"><span data-stu-id="9045c-115">CPython - hello standard and most commonly used Python interpreter</span></span>
* <span data-ttu-id="9045c-116">PyPy - gyors, megfelelő alternatív implementáció tooCPython</span><span class="sxs-lookup"><span data-stu-id="9045c-116">PyPy - fast, compliant alternative implementation tooCPython</span></span>
* <span data-ttu-id="9045c-117">IronPython - .net/CLR futó Python parancsértelmező</span><span class="sxs-lookup"><span data-stu-id="9045c-117">IronPython - Python interpreter that runs on .Net/CLR</span></span>
* <span data-ttu-id="9045c-118">Jython - Python parancsértelmező hello Java virtuális gép futó</span><span class="sxs-lookup"><span data-stu-id="9045c-118">Jython - Python interpreter that runs on hello Java Virtual Machine</span></span>

<span data-ttu-id="9045c-119">**CPython** v2.7 vagy v3.3 + és PyPy 5.4.0 tesztelése, és a Python Azure SDK hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="9045c-119">**CPython** v2.7 or v3.3+ and PyPy 5.4.0 are tested and supported for hello Python Azure SDK.</span></span>

## <a name="where-tooget-python"></a><span data-ttu-id="9045c-120">Ha tooget Python?</span><span class="sxs-lookup"><span data-stu-id="9045c-120">Where tooget Python?</span></span>
<span data-ttu-id="9045c-121">Számos módon tooget CPython van:</span><span class="sxs-lookup"><span data-stu-id="9045c-121">There are several ways tooget CPython:</span></span>

* <span data-ttu-id="9045c-122">Közvetlenül a [www.python.org][www.python.org]</span><span class="sxs-lookup"><span data-stu-id="9045c-122">Directly from [www.python.org][www.python.org]</span></span>
* <span data-ttu-id="9045c-123">Egy megbízható distro többek között a [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] vagy [www.activestate.com][www.activestate.com]</span><span class="sxs-lookup"><span data-stu-id="9045c-123">From a reputable distro such as [www.continuum.io][www.continuum.io], [www.enthought.com][www.enthought.com] or [www.activestate.com][www.activestate.com]</span></span>
* <span data-ttu-id="9045c-124">Build forrásból!</span><span class="sxs-lookup"><span data-stu-id="9045c-124">Build from source!</span></span>

<span data-ttu-id="9045c-125">Ha egy konkrét OK, ajánlott hello első két lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9045c-125">Unless you have a specific need, we recommend hello first two options.</span></span>

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a><span data-ttu-id="9045c-126">A Windows, Linux és MacOS (csak az ügyféloldali kódtáraknál) SDK telepítése</span><span class="sxs-lookup"><span data-stu-id="9045c-126">SDK Installation on Windows, Linux, and MacOS (client libraries only)</span></span>
<span data-ttu-id="9045c-127">Ha már rendelkezik telepített Python, használhatja a pip tooinstall egy kötegelt hello ügyfél tárakat a meglévő Python 2.7-es vagy 3.3-as + Python-környezetben.</span><span class="sxs-lookup"><span data-stu-id="9045c-127">If you already have Python installed, you can use pip tooinstall a bundle of all hello client libraries in your existing Python 2.7 or Python 3.3+ environment.</span></span> <span data-ttu-id="9045c-128">Ez letölti hello csomagok hello [Python-Csomagindexet] [ Python Package Index] (PyPI).</span><span class="sxs-lookup"><span data-stu-id="9045c-128">This downloads hello packages from hello [Python Package Index][Python Package Index] (PyPI).</span></span>

<span data-ttu-id="9045c-129">Rendszergazdai jogosultsággal is kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="9045c-129">You may need administrator rights:</span></span>

* <span data-ttu-id="9045c-130">Linux- és MacOS, használja a hello `sudo` parancs: `sudo pip install azure-mgmt-compute`.</span><span class="sxs-lookup"><span data-stu-id="9045c-130">Linux and MacOS, use hello `sudo` command: `sudo pip install azure-mgmt-compute`.</span></span>
* <span data-ttu-id="9045c-131">Windows: Nyissa meg a PowerShell vagy parancssor rendszergazdaként</span><span class="sxs-lookup"><span data-stu-id="9045c-131">Windows: open your PowerShell/Command prompt as an administrator</span></span>

<span data-ttu-id="9045c-132">Minden egyes Azure szolgáltatás külön-külön egyes tárak telepítése:</span><span class="sxs-lookup"><span data-stu-id="9045c-132">You can install individually each library for each Azure service:</span></span>

```console
   $ pip install azure-batch          # Install hello latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install hello latest Storage management library
```

<span data-ttu-id="9045c-133">Előzetes csomagok hello telepíthető `--pre` jelző:</span><span class="sxs-lookup"><span data-stu-id="9045c-133">Preview packages can be installed using hello `--pre` flag:</span></span>

```console
   $ pip install --pre azure-mgmt-compute # installs only hello latest Compute Management library
```

<span data-ttu-id="9045c-134">Azure szalagtárak készlete hello segítségével egyetlen sorban is telepíthet `azure` meta-csomag.</span><span class="sxs-lookup"><span data-stu-id="9045c-134">You can also install a set of Azure libraries in a single line using hello `azure` meta-package.</span></span> <span data-ttu-id="9045c-135">A metaadat-csomag nem az összes csomag táblaként vannak közzétéve stabil még, mivel hello `azure` meta-csomag még előzetes verzió van.</span><span class="sxs-lookup"><span data-stu-id="9045c-135">Since not all packages in this meta-package are published as stable yet, hello `azure` meta-package is still in preview.</span></span>
<span data-ttu-id="9045c-136">Azonban hello fő csomagoknak kód minőségi/teljességét nézőpontból is tekinthető "stable" most</span><span class="sxs-lookup"><span data-stu-id="9045c-136">However, hello core packages, from code quality/completeness perspectives can be considered "stable" at this time</span></span>

* <span data-ttu-id="9045c-137">hivatalos címkéjén ilyen szinkronban más nyelvek lehető legrövidebb időn belül.</span><span class="sxs-lookup"><span data-stu-id="9045c-137">it is officially labeled as such in sync with other languages as soon as possible.</span></span>
  <span data-ttu-id="9045c-138">Jelenleg nem tervezi, addig a további lényegesen módosul.</span><span class="sxs-lookup"><span data-stu-id="9045c-138">We are not planning on any further major changes until then.</span></span>

<span data-ttu-id="9045c-139">Mivel ez egy előzetes, toouse hello kell `--pre` jelző:</span><span class="sxs-lookup"><span data-stu-id="9045c-139">Since it's a preview release, you need toouse hello `--pre` flag:</span></span>

```console
   $ pip install --pre azure
```

<span data-ttu-id="9045c-140">vagy közvetlenül</span><span class="sxs-lookup"><span data-stu-id="9045c-140">or directly</span></span>

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a><span data-ttu-id="9045c-141">További csomagok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="9045c-141">Getting More Packages</span></span>
<span data-ttu-id="9045c-142">Hello [Python-Csomagindexet] [ Python Package Index] (PyPI) rendelkezik a gazdag kijelölt Python szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="9045c-142">hello [Python Package Index][Python Package Index] (PyPI) has a rich selection of Python libraries.</span></span>  <span data-ttu-id="9045c-143">Ha úgy dönt, tooinstall egy Distro, összekapcsolta bit, webes fejlesztési tooTechnical különböző lehetőségeket az érdekes hello többsége már Computing.</span><span class="sxs-lookup"><span data-stu-id="9045c-143">If you chose tooinstall a Distro, you'll already have most of hello interesting bits for various scenarios from web development tooTechnical Computing.</span></span>

## <a name="python-tools-for-visual-studio"></a><span data-ttu-id="9045c-144">Python Tools for Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9045c-144">Python Tools for Visual Studio</span></span>
<span data-ttu-id="9045c-145">[A Python Tools for Visual Studio][a Python Tools for Visual Studio] (PTVS) egy ingyenes/OSS beépülő modul Microsoft Visual STUDIO alakítja át a teljes körű Python IDE:</span><span class="sxs-lookup"><span data-stu-id="9045c-145">[Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS) is a free/OSS plugin from Microsoft, which turns VS into a full-fledged Python IDE:</span></span>

![How-to-telepítés – python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

<span data-ttu-id="9045c-147">PTVS használata nem kötelező, de ajánlott, mivel biztosít Python és a webes projekt/megoldás támogatja, hibakeresés, profilkészítési, interaktív ablak, Sablonszerkesztés és Intellisense.</span><span class="sxs-lookup"><span data-stu-id="9045c-147">Using PTVS is optional, but is recommended as it gives you Python and Web Project/Solution support, debugging, profiling, interactive window, Template editing, and Intellisense.</span></span>

<span data-ttu-id="9045c-148">PTVS is teszi, hogy könnyen toodeploy tooMicrosoft Azure, az üzembe helyezés támogatása túl[Felhőszolgáltatások](cloud-services/cloud-services-python-ptvs.md) és [webhelyek](app-service-web/web-sites-python-ptvs-django-mysql.md).</span><span class="sxs-lookup"><span data-stu-id="9045c-148">PTVS also makes it easy toodeploy tooMicrosoft Azure, with support for deployment too[Cloud Services](cloud-services/cloud-services-python-ptvs.md) and [Websites](app-service-web/web-sites-python-ptvs-django-mysql.md).</span></span>

<span data-ttu-id="9045c-149">PTVS a Visual Studio 2013, 2015-öt vagy 2017 példánya működik.</span><span class="sxs-lookup"><span data-stu-id="9045c-149">PTVS works with your existing Visual Studio 2013, 2015, or 2017 installation.</span></span>  <span data-ttu-id="9045c-150">Dokumentáció, letöltéseket és vitafórumok: [a Python Tools for Visual Studio].</span><span class="sxs-lookup"><span data-stu-id="9045c-150">For documentation, downloads and discussions, see [Python Tools for Visual Studio].</span></span>  

## <a name="python-azure-scenarios-for-linux-and-macos"></a><span data-ttu-id="9045c-151">Python Linux és MacOS Azure forgatókönyvei</span><span class="sxs-lookup"><span data-stu-id="9045c-151">Python Azure Scenarios for Linux and MacOS</span></span>
<span data-ttu-id="9045c-152">A Linux vagy MacOS, fő Azure forgatókönyvek támogatottak:</span><span class="sxs-lookup"><span data-stu-id="9045c-152">For Linux or MacOS, main Azure scenarios that are supported:</span></span>

1. <span data-ttu-id="9045c-153">Azure Services fel Python hello klienskódtárak használatával</span><span class="sxs-lookup"><span data-stu-id="9045c-153">Consuming Azure Services by using hello client libraries for Python</span></span>
2. <span data-ttu-id="9045c-154">Az alkalmazás futó Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="9045c-154">Running your app in a Linux VM</span></span>
3. <span data-ttu-id="9045c-155">Fejlesztés és a közzétételi tooAzure webhelyek Git használatával</span><span class="sxs-lookup"><span data-stu-id="9045c-155">Developing and publishing tooAzure Websites using Git</span></span>

<span data-ttu-id="9045c-156">első forgatókönyv hello lehetővé teszi előnyeit hello Azure PaaS képességeket például tooauthor gazdag webalkalmazások [blob-tároló](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [várólista tárolási](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) Pythonic burkolóit a(z) hello Azure REST API-kon keresztül stb.</span><span class="sxs-lookup"><span data-stu-id="9045c-156">hello first scenario enables you tooauthor rich web apps that take advantage of hello Azure PaaS capabilities such as [blob storage](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [queue storage](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) etc. via Pythonic wrappers for hello Azure REST APIs.</span></span> <span data-ttu-id="9045c-157">Ezek azonos Windows, Mac és Linux rendszeren működik.</span><span class="sxs-lookup"><span data-stu-id="9045c-157">These work identically on Windows, Mac, and Linux.</span></span>  <span data-ttu-id="9045c-158">A helyi fejlesztési számítógépén vagy az Azure-on futó Linux virtuális gép is használhatja a ügyfél tárak.</span><span class="sxs-lookup"><span data-stu-id="9045c-158">You can also use these client libraries from your local development machine or a Linux VM running on Azure.</span></span>

<span data-ttu-id="9045c-159">Hello VM forgatókönyvhöz egyszerűen indítsa el az Ön által választott (Ubuntu, CentOS, Suse) Linux virtuális gép, és futtassa, felügyeletéhez van lehetősége.</span><span class="sxs-lookup"><span data-stu-id="9045c-159">For hello VM scenario, you simply start a Linux VM of your choice (Ubuntu, CentOS, Suse) and run/manage what you like.</span></span>  <span data-ttu-id="9045c-160">Például futtathatja [IPython] [ IPython] REPL/notebook a Windows vagy Mac/Linux számítógép és a böngésző tooa Linux vagy a Windows multi-proc virtuális gép, amelyen hello Azure IPython motor pont.</span><span class="sxs-lookup"><span data-stu-id="9045c-160">As an example, you can run [IPython][IPython] REPL/notebook on your Windows/Mac/Linux machine and point your browser tooa Linux or Windows multi-proc VM running hello IPython Engine on Azure.</span></span>

<span data-ttu-id="9045c-161">Információ tooset Linux virtuális gépet, lásd: hello [hozzon létre egy virtuális gép futó Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="9045c-161">For information on how tooset up a Linux VM, see hello [Create a Virtual Machine Running Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tutorial.</span></span>

<span data-ttu-id="9045c-162">Git üzemelő példányt használ, a Python webalkalmazás fejlesztése és közzéteszi az Azure-webhely tooan bármely operációs rendszerből.</span><span class="sxs-lookup"><span data-stu-id="9045c-162">Using Git deployment, you can develop a Python web application and publish it tooan Azure Website from any operating system.</span></span>  <span data-ttu-id="9045c-163">Amikor a tárház tooAzure, automatikusan létrehoz egy virtuális környezethez, és a pip telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="9045c-163">When you push your repository tooAzure, it automatically creates a virtual environment and pip installs your required packages.</span></span>

<span data-ttu-id="9045c-164">Kifejlesztett és közzététele az Azure Websitesra további információkért lásd: hello oktatóanyag [webhelyek létrehozása a djangóval](app-service-web/web-sites-python-create-deploy-django-app.md), [Bottle webhelyek létrehozásának](app-service-web/web-sites-python-create-deploy-bottle-app.md), és [webhelyek létrehozása Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span><span class="sxs-lookup"><span data-stu-id="9045c-164">For more information on developing and publishing Azure Websites, see hello tutorials for [Creating Websites with Django](app-service-web/web-sites-python-create-deploy-django-app.md), [Creating Websites with Bottle](app-service-web/web-sites-python-create-deploy-bottle-app.md), and [Creating Websites with Flask](app-service-web/web-sites-python-create-deploy-flask-app.md).</span></span> <span data-ttu-id="9045c-165">Bármely WSGI-kompatibilis keretrendszer használatával további általános információkért lásd: [Python konfigurálása az Azure Websitesra](app-service-web/web-sites-python-configure.md).</span><span class="sxs-lookup"><span data-stu-id="9045c-165">For more general information on using any WSGI-compliant framework, see [Configuring Python with Azure Websites](app-service-web/web-sites-python-configure.md).</span></span>

## <a name="additional-software-and-resources"></a><span data-ttu-id="9045c-166">További szoftverek és erőforrások:</span><span class="sxs-lookup"><span data-stu-id="9045c-166">Additional Software and Resources:</span></span>
* [<span data-ttu-id="9045c-167">Python ReadTheDocs készült Azure SDK</span><span class="sxs-lookup"><span data-stu-id="9045c-167">Azure SDK for Python ReadTheDocs</span></span>](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [<span data-ttu-id="9045c-168">A Python github használatára az Azure SDK</span><span class="sxs-lookup"><span data-stu-id="9045c-168">Azure SDK for Python GitHub</span></span>](https://github.com/Azure/azure-sdk-for-python)
* [<span data-ttu-id="9045c-169">Python hivatalos Azure-minták</span><span class="sxs-lookup"><span data-stu-id="9045c-169">Official Azure samples for Python</span></span>](https://azure.microsoft.com/documentation/samples/?platform=python)
* <span data-ttu-id="9045c-170">[Alakíthatnak ki olyan Analytics Python elosztási][Continuum Analytics Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="9045c-170">[Continuum Analytics Python Distribution][Continuum Analytics Python Distribution]</span></span>
* <span data-ttu-id="9045c-171">[Enthought Python elosztási][Enthought Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="9045c-171">[Enthought Python Distribution][Enthought Python Distribution]</span></span>
* <span data-ttu-id="9045c-172">[ActiveState Python elosztási][ActiveState Python Distribution]</span><span class="sxs-lookup"><span data-stu-id="9045c-172">[ActiveState Python Distribution][ActiveState Python Distribution]</span></span>
* <span data-ttu-id="9045c-173">[SciPy - tudományos Python szalagtárak együttese][SciPy - A suite of Scientific Python libraries]</span><span class="sxs-lookup"><span data-stu-id="9045c-173">[SciPy - A suite of Scientific Python libraries][SciPy - A suite of Scientific Python libraries]</span></span>
* <span data-ttu-id="9045c-174">[NumPy - írhatók kódtára a Pythonhoz][NumPy - A numerics library for Python]</span><span class="sxs-lookup"><span data-stu-id="9045c-174">[NumPy - A numerics library for Python][NumPy - A numerics library for Python]</span></span>
* <span data-ttu-id="9045c-175">[A Django-projekt - érett webes keretrendszer/CMS][Django Project - A mature web framework/CMS]</span><span class="sxs-lookup"><span data-stu-id="9045c-175">[Django Project - A mature web framework/CMS][Django Project - A mature web framework/CMS]</span></span>
* <span data-ttu-id="9045c-176">[IPython – egy speciális REPL/Notebook Python-hez][IPython - an advanced REPL/Notebook for Python]</span><span class="sxs-lookup"><span data-stu-id="9045c-176">[IPython - an advanced REPL/Notebook for Python][IPython - an advanced REPL/Notebook for Python]</span></span>
* <span data-ttu-id="9045c-177">[A Python Tools for Visual Studio a Githubon][Python Tools for Visual Studio on GitHub]</span><span class="sxs-lookup"><span data-stu-id="9045c-177">[Python Tools for Visual Studio on GitHub][Python Tools for Visual Studio on GitHub]</span></span>
* [<span data-ttu-id="9045c-178">Python fejlesztői központ</span><span class="sxs-lookup"><span data-stu-id="9045c-178">Python Developer Center</span></span>](/develop/python/)

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
[Setting up a Linux VM via hello Azure portal]: create-and-configure-opensuse-vm-in-portal.md
[How toouse hello Azure Command-Line Interface]: crossplat-cmd-tools.md
[Create a Virtual Machine Running Linux]: virtual-machines-linux-quick-create-cli.md
[Creating Websites with Django]: web-sites-python-create-deploy-django-app.md
[Creating Websites with Bottle]: web-sites-python-create-deploy-bottle-app.md
[Creating Websites with Flask]: web-sites-python-create-deploy-flask-app.md
[Configuring Python with Azure Websites]: web-sites-python-configure.md
[table storage]: storage-python-how-to-use-table-storage.md
[queue storage]: storage-python-how-to-use-queue-storage.md
[blob storage]:storage/blobs/storage-python-how-to-use-blob-storage.md
