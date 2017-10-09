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
# <a name="installing-python-and-hello-sdk"></a>Python és hello SDK telepítése
Python könnyen tooset működik-e a Windows és a Mac, Linux, előre előre telepített és [Bash for Windows](https://msdn.microsoft.com/commandline/wsl/about). Ez az útmutató bemutatja, hogyan telepítési és a gép az Azure-ral használatra kész beolvasása.

## <a name="whats-in-hello-python-azure-sdk"></a>Mi az a hello Python Azure SDK-t?
hello Azure SDK for Python összetevői, amelyek lehetővé teszik toodevelop, telepítéséhez és felügyeletéhez a Python-alkalmazások az Azure-bA. Hello Azure SDK for Python konkrétan az alábbi hello:

* **Kezelési kódtárakat**. Ezek osztálykönyvtárakhoz biztosít egy Azure-erőforrások, például a tárfiókokat, virtuális gépek kezelése felületet.
* **Végrehajtási könyvtárak**. Ezek osztálykönyvtárakhoz tevő interfészt biztosítanak az Azure-szolgáltatások, például a tárolás és a service bus eléréséhez.

## <a name="which-python-and-which-version-toouse"></a>Mely Python és melyik verzió toouse
Nincsenek elérhető Python tolmácsoknak több verziója – például:

* CPython - hello standard és a leggyakrabban használt Python parancsértelmező
* PyPy - gyors, megfelelő alternatív implementáció tooCPython
* IronPython - .net/CLR futó Python parancsértelmező
* Jython - Python parancsértelmező hello Java virtuális gép futó

**CPython** v2.7 vagy v3.3 + és PyPy 5.4.0 tesztelése, és a Python Azure SDK hello támogatott.

## <a name="where-tooget-python"></a>Ha tooget Python?
Számos módon tooget CPython van:

* Közvetlenül a [www.python.org][www.python.org]
* Egy megbízható distro többek között a [www.continuum.io][www.continuum.io], [www.enthought.com] [ www.enthought.com] vagy [www.activestate.com][www.activestate.com]
* Build forrásból!

Ha egy konkrét OK, ajánlott hello első két lehetőséget.

## <a name="sdk-installation-on-windows-linux-and-macos-client-libraries-only"></a>A Windows, Linux és MacOS (csak az ügyféloldali kódtáraknál) SDK telepítése
Ha már rendelkezik telepített Python, használhatja a pip tooinstall egy kötegelt hello ügyfél tárakat a meglévő Python 2.7-es vagy 3.3-as + Python-környezetben. Ez letölti hello csomagok hello [Python-Csomagindexet] [ Python Package Index] (PyPI).

Rendszergazdai jogosultsággal is kell rendelkeznie:

* Linux- és MacOS, használja a hello `sudo` parancs: `sudo pip install azure-mgmt-compute`.
* Windows: Nyissa meg a PowerShell vagy parancssor rendszergazdaként

Minden egyes Azure szolgáltatás külön-külön egyes tárak telepítése:

```console
   $ pip install azure-batch          # Install hello latest Batch runtime library
   $ pip install azure-mgmt-scheduler # Install hello latest Storage management library
```

Előzetes csomagok hello telepíthető `--pre` jelző:

```console
   $ pip install --pre azure-mgmt-compute # installs only hello latest Compute Management library
```

Azure szalagtárak készlete hello segítségével egyetlen sorban is telepíthet `azure` meta-csomag. A metaadat-csomag nem az összes csomag táblaként vannak közzétéve stabil még, mivel hello `azure` meta-csomag még előzetes verzió van.
Azonban hello fő csomagoknak kód minőségi/teljességét nézőpontból is tekinthető "stable" most

* hivatalos címkéjén ilyen szinkronban más nyelvek lehető legrövidebb időn belül.
  Jelenleg nem tervezi, addig a további lényegesen módosul.

Mivel ez egy előzetes, toouse hello kell `--pre` jelző:

```console
   $ pip install --pre azure
```

vagy közvetlenül

```console
   $ pip install azure==2.0.0rc6
```

## <a name="getting-more-packages"></a>További csomagok lekérdezése
Hello [Python-Csomagindexet] [ Python Package Index] (PyPI) rendelkezik a gazdag kijelölt Python szalagtárak.  Ha úgy dönt, tooinstall egy Distro, összekapcsolta bit, webes fejlesztési tooTechnical különböző lehetőségeket az érdekes hello többsége már Computing.

## <a name="python-tools-for-visual-studio"></a>Python Tools for Visual Studio
[A Python Tools for Visual Studio][a Python Tools for Visual Studio] (PTVS) egy ingyenes/OSS beépülő modul Microsoft Visual STUDIO alakítja át a teljes körű Python IDE:

![How-to-telepítés – python-ptvs](./media/python-how-to-install/how-to-install-python-ptvs.png)

PTVS használata nem kötelező, de ajánlott, mivel biztosít Python és a webes projekt/megoldás támogatja, hibakeresés, profilkészítési, interaktív ablak, Sablonszerkesztés és Intellisense.

PTVS is teszi, hogy könnyen toodeploy tooMicrosoft Azure, az üzembe helyezés támogatása túl[Felhőszolgáltatások](cloud-services/cloud-services-python-ptvs.md) és [webhelyek](app-service-web/web-sites-python-ptvs-django-mysql.md).

PTVS a Visual Studio 2013, 2015-öt vagy 2017 példánya működik.  Dokumentáció, letöltéseket és vitafórumok: [a Python Tools for Visual Studio].  

## <a name="python-azure-scenarios-for-linux-and-macos"></a>Python Linux és MacOS Azure forgatókönyvei
A Linux vagy MacOS, fő Azure forgatókönyvek támogatottak:

1. Azure Services fel Python hello klienskódtárak használatával
2. Az alkalmazás futó Linux virtuális gép
3. Fejlesztés és a közzétételi tooAzure webhelyek Git használatával

első forgatókönyv hello lehetővé teszi előnyeit hello Azure PaaS képességeket például tooauthor gazdag webalkalmazások [blob-tároló](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [várólista tárolási](storage/queues/storage-python-how-to-use-queue-storage.md), [table storage](cosmos-db/table-storage-how-to-use-python.md) Pythonic burkolóit a(z) hello Azure REST API-kon keresztül stb. Ezek azonos Windows, Mac és Linux rendszeren működik.  A helyi fejlesztési számítógépén vagy az Azure-on futó Linux virtuális gép is használhatja a ügyfél tárak.

Hello VM forgatókönyvhöz egyszerűen indítsa el az Ön által választott (Ubuntu, CentOS, Suse) Linux virtuális gép, és futtassa, felügyeletéhez van lehetősége.  Például futtathatja [IPython] [ IPython] REPL/notebook a Windows vagy Mac/Linux számítógép és a böngésző tooa Linux vagy a Windows multi-proc virtuális gép, amelyen hello Azure IPython motor pont.

Információ tooset Linux virtuális gépet, lásd: hello [hozzon létre egy virtuális gép futó Linux](virtual-machines/linux/quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) oktatóanyag.

Git üzemelő példányt használ, a Python webalkalmazás fejlesztése és közzéteszi az Azure-webhely tooan bármely operációs rendszerből.  Amikor a tárház tooAzure, automatikusan létrehoz egy virtuális környezethez, és a pip telepíti a szükséges csomagokat.

Kifejlesztett és közzététele az Azure Websitesra további információkért lásd: hello oktatóanyag [webhelyek létrehozása a djangóval](app-service-web/web-sites-python-create-deploy-django-app.md), [Bottle webhelyek létrehozásának](app-service-web/web-sites-python-create-deploy-bottle-app.md), és [webhelyek létrehozása Flask](app-service-web/web-sites-python-create-deploy-flask-app.md). Bármely WSGI-kompatibilis keretrendszer használatával további általános információkért lásd: [Python konfigurálása az Azure Websitesra](app-service-web/web-sites-python-configure.md).

## <a name="additional-software-and-resources"></a>További szoftverek és erőforrások:
* [Python ReadTheDocs készült Azure SDK](http://azure-sdk-for-python.readthedocs.io/en/latest/)
* [A Python github használatára az Azure SDK](https://github.com/Azure/azure-sdk-for-python)
* [Python hivatalos Azure-minták](https://azure.microsoft.com/documentation/samples/?platform=python)
* [Alakíthatnak ki olyan Analytics Python elosztási][Continuum Analytics Python Distribution]
* [Enthought Python elosztási][Enthought Python Distribution]
* [ActiveState Python elosztási][ActiveState Python Distribution]
* [SciPy - tudományos Python szalagtárak együttese][SciPy - A suite of Scientific Python libraries]
* [NumPy - írhatók kódtára a Pythonhoz][NumPy - A numerics library for Python]
* [A Django-projekt - érett webes keretrendszer/CMS][Django Project - A mature web framework/CMS]
* [IPython – egy speciális REPL/Notebook Python-hez][IPython - an advanced REPL/Notebook for Python]
* [A Python Tools for Visual Studio a Githubon][Python Tools for Visual Studio on GitHub]
* [Python fejlesztői központ](/develop/python/)

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
