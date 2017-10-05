---
title: "Hozzon létre egy Jupyter/IPython Notebook |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítheti a Jupyter/IPython Notebook létrehozása az Azure resource manager üzembe helyezési modellben a Linux virtuális gépen."
services: virtual-machines-linux
documentationcenter: python
author: crwilcox
manager: wpickett
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 519f36dd-865e-4c1d-abe7-b87037796aa7
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: python
ms.topic: article
ms.date: 11/10/2015
ms.author: crwilcox
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5940190822cd5c5b78ea0e8f5c8695608d351d6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="c173e-103">Egy Azure virtuális gép létrehozása, Jupyter telepítése és futtatása a Jupyter Notebook az Azure-on</span><span class="sxs-lookup"><span data-stu-id="c173e-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="c173e-104">A [Jupyter projekt](http://jupyter.org), korábbi nevén a [IPython projekt](http://ipython.org), olyan eszközöket biztosít tudományos számítástechnikai kód végrehajtása miatt a létrehozása olyan élő számítási dokumentum felhasználó hatékony interaktív rendszerhéjának használatával.</span><span class="sxs-lookup"><span data-stu-id="c173e-104">The [Jupyter project](http://jupyter.org), formerly the [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with the creation of a live computational document.</span></span> <span data-ttu-id="c173e-105">A notebook fájlok tetszőleges szöveg, matematikai képletek, bemeneti kódot, eredményeket, grafikus, videók és bármely más típusú, hogy a modern webböngésző megjelenítésére alkalmas adathordozó is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="c173e-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="c173e-106">Feltétlenül ismerkedik Python, és ismerje meg, hogy szórakoztató, interaktív környezetben vagy annak egy bizonyos súlyos párhuzamos és technikai számítástechnikai szeretné, hogy a Jupyter Notebook-e kiváló választás.</span><span class="sxs-lookup"><span data-stu-id="c173e-106">Whether you're absolutely new to Python and want to learn it in a fun, interactive environment or do some serious parallel/technical computing, the Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="c173e-107">![Képernyőkép](./media/jupyter-notebook/ipy-notebook-spectral.png) használatával SciPy és Matplotlib csomagot kell elemezni egy hangrögzítés szerkezete.</span><span class="sxs-lookup"><span data-stu-id="c173e-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages to analyze the structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="c173e-108">Jupyter két módon: Azure notebookok vagy egyéni központi telepítés</span><span class="sxs-lookup"><span data-stu-id="c173e-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="c173e-109">Azure kínál egy szolgáltatást, amely segítségével [Jupyter használatának gyors megkezdéséhez ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="c173e-109">Azure provides a service that you can use to [quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="c173e-110">Az Azure Notebook szolgáltatás használatával egyszerűen hozzáférhetünk Jupyter tartozó interneten elérhető felület méretezhető számítási erőforrások eléréséről a Python a teljesítmény és a sok szalagtárak.</span><span class="sxs-lookup"><span data-stu-id="c173e-110">By using the Azure Notebook Service, you can easily gain access to Jupyter's web-accessible interface to scalable computational resources with all the power of Python and its many libraries.</span></span>  <span data-ttu-id="c173e-111">Mivel telepítését végzi el a szolgáltatást, felhasználók férhetnek hozzá ezekhez az erőforrásokhoz, felügyelete és konfigurálása a felhasználó által szükségessége nélkül.</span><span class="sxs-lookup"><span data-stu-id="c173e-111">Since the installation is handled by the service, users can access these resources without the need for administration and configuration by the user.</span></span>

<span data-ttu-id="c173e-112">Ha a notebook szolgáltatás nem működik a forgatókönyvnek ezentúl is ebben a cikkben, amely fog bemutatja, hogyan központi telepítése a Microsoft Azure-ban a Jupyter Notebook Linux virtuális gépek (VM) használatával.</span><span class="sxs-lookup"><span data-stu-id="c173e-112">If the notebook service does not work for your scenario please continue to read this article which will will show you how to deploy the Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="c173e-113">Hozza létre és konfigurálja a virtuális gépek az Azure-on</span><span class="sxs-lookup"><span data-stu-id="c173e-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="c173e-114">Az első lépés az Azure-on futó virtuális gép (VM) létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c173e-114">The first step is to create a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="c173e-115">Ez a virtuális gép a felhőben található teljes operációs rendszer, és a Jupyter Notebook futtatásához használandó.</span><span class="sxs-lookup"><span data-stu-id="c173e-115">This VM is a complete operating system in the cloud and will be used to run the Jupyter Notebook.</span></span> <span data-ttu-id="c173e-116">Azure Linux és a Windows virtuális gépek futtatására képes, vagyis, és bemutatjuk, hogy a virtuális gépek mindkét típusú Jupyter telepítését.</span><span class="sxs-lookup"><span data-stu-id="c173e-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover the setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="c173e-117">Hozzon létre egy Linux virtuális Gépet, és a Jupyter port megnyitása</span><span class="sxs-lookup"><span data-stu-id="c173e-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="c173e-118">Kövesse az utasításokat, megadott [Itt] [ portal-vm-linux] a virtuális gép létrehozásához a *Ubuntu* terjesztési.</span><span class="sxs-lookup"><span data-stu-id="c173e-118">Follow the instructions given [here][portal-vm-linux] to create a virtual machine of the *Ubuntu* distribution.</span></span> <span data-ttu-id="c173e-119">Ez az oktatóanyag Ubuntu Server 14.04 LTS használja.</span><span class="sxs-lookup"><span data-stu-id="c173e-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="c173e-120">Feltételezzük, hogy a felhasználónév *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="c173e-120">We'll assume the user name *azureuser*.</span></span>

<span data-ttu-id="c173e-121">Miután a virtuális gépet telepít kell nyissa meg a szabály a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="c173e-121">After the virtual machine deploys we need to open up a security rule on the network security group.</span></span>  <span data-ttu-id="c173e-122">Ugrás az Azure portálról **hálózati biztonsági csoportok** , és nyissa meg a lap a biztonsági csoport a virtuális gép megfelelő.</span><span class="sxs-lookup"><span data-stu-id="c173e-122">From the Azure portal, go to **Network Security Groups** and open the tab for the Security Group corresponding to your VM.</span></span> <span data-ttu-id="c173e-123">Hozzá kell adnia egy bejövő biztonsági szabály a következő beállításokkal: **TCP** a protokoll  **\***  a forrás (nyilvános) port és **9999** a célport (személyes).</span><span class="sxs-lookup"><span data-stu-id="c173e-123">You need to add an Inbound Security rule with the following settings: **TCP** for the protocol, **\*** for the source (public) port and **9999** for the destination (private) port.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="c173e-125">Az a hálózati biztonsági csoporthoz, kattintson a **hálózati illesztők** meg és jegyezze fel a **nyilvános IP-cím** csatlakozni a virtuális Gépet a következő lépésben lesz szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="c173e-125">While in your Network Security Group, click on **Network Interfaces** and note the **Public IP Address** as it will be needed to connect to your VM in the next step.</span></span>

## <a name="install-required-software-on-the-vm"></a><span data-ttu-id="c173e-126">Szükséges szoftverek telepítése a virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="c173e-126">Install required software on the VM</span></span>
<span data-ttu-id="c173e-127">A Jupyter Notebook futtatásához a virtuális gépen, hogy először telepítenie kell Jupyter és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="c173e-127">To run the Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="c173e-128">Kapcsolódás a linux virtuális gép ssh és a felhasználónév/jelszó párosítsa azt a virtuális gép megadásakor.</span><span class="sxs-lookup"><span data-stu-id="c173e-128">Connect to your linux vm using ssh and the username/password pair you chose when you created the vm.</span></span> <span data-ttu-id="c173e-129">Ez az oktatóanyag a rendszer a PuTTY használata és csatlakozás Windows.</span><span class="sxs-lookup"><span data-stu-id="c173e-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="c173e-130">Ubuntu Jupyter telepítése</span><span class="sxs-lookup"><span data-stu-id="c173e-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="c173e-131">Anaconda, olyan népszerű adatok tudományos python elosztási, a megadott hivatkozások valamelyikével telepítése [alakíthatnak ki olyan Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="c173e-131">Install Anaconda, a popular data science python distribution, using one of the links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="c173e-132">A jelen dokumentum írásáig a hivatkozásokat az alábbiakban a legtöbb legfeljebb dátum verziók.</span><span class="sxs-lookup"><span data-stu-id="c173e-132">As of the writing of this document, the below links are the most up to date versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="c173e-133">Linux anaconda telepíti</span><span class="sxs-lookup"><span data-stu-id="c173e-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="c173e-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="c173e-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="c173e-135">Python 2.7-es</span><span class="sxs-lookup"><span data-stu-id="c173e-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="c173e-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="c173e-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="c173e-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="c173e-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="c173e-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="c173e-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="c173e-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="c173e-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="c173e-140">Anaconda3 telepítése 2.3.0 Ubuntu 64 bites</span><span class="sxs-lookup"><span data-stu-id="c173e-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="c173e-141">Például azt, hogyan telepíthető Anaconda Ubuntu</span><span class="sxs-lookup"><span data-stu-id="c173e-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter to the latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Képernyőfelvétel](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="c173e-143">Jupyter konfigurálása és az SSL használata</span><span class="sxs-lookup"><span data-stu-id="c173e-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="c173e-144">Telepítése után kell beállítani a konfigurációs fájlokat a Jupyter néhány percet.</span><span class="sxs-lookup"><span data-stu-id="c173e-144">After installing we need to take a moment to setup the configuration files for Jupyter.</span></span> <span data-ttu-id="c173e-145">A Jupyter konfigurálása során fellépő problémák tapasztal lássunk hasznos lehet a [Jupyter Notebook-kiszolgálót futtató dokumentációja](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="c173e-145">If you experience troubles with configuring Jupyter it may be helpful to look at the [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="c173e-146">Tovább a Microsoft `cd` a profil könyvtár az SSL-tanúsítvány létrehozásához és szerkesztéséhez a profilok konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="c173e-146">Next we `cd` to the profile directory to create our SSL certificate and edit the profiles configuration file.</span></span>

<span data-ttu-id="c173e-147">Linux rendszeren használja az alábbi parancsot.</span><span class="sxs-lookup"><span data-stu-id="c173e-147">On Linux use the following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="c173e-148">A következő paranccsal (Linux és Windows rendszerekhez) az SSL-tanúsítvány létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="c173e-148">Use the following command to create the SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="c173e-149">Vegye figyelembe, hogy a beállítást, mivel egy önaláírt SSL-tanúsítványt, amikor csatlakozik a notebook a böngésző meg biztonsági figyelmeztetést ad.</span><span class="sxs-lookup"><span data-stu-id="c173e-149">Note that since we are creating a self-signed SSL certificate, when connecting to the notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="c173e-150">A hosszú távú éles környezetben való használathoz érdemes a szervezetéhez tartozó megfelelően aláírt tanúsítványát használja.</span><span class="sxs-lookup"><span data-stu-id="c173e-150">For long-term production use, you will want to use a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="c173e-151">Mivel ez a bemutató túlmutató tanúsítványkezelés, azt fogja anyagot egy önaláírt tanúsítványt most.</span><span class="sxs-lookup"><span data-stu-id="c173e-151">Since certificate management is beyond the scope of this demo, we will stick to a self-signed certificate for now.</span></span>

<span data-ttu-id="c173e-152">Mellett olyan tanúsítványt használ, meg kell adnia egy jelszót a notebook védelmében az illetéktelen használattól is.</span><span class="sxs-lookup"><span data-stu-id="c173e-152">In addition to using a certificate, you must also provide a password to protect your notebook from unauthorized use.</span></span>  <span data-ttu-id="c173e-153">Biztonsági okokból a Jupyter titkosított jelszavak a konfigurációs fájlban használ, ezért először titkosítja a jelszót kell be a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="c173e-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need to encrypt your password first.</span></span>  <span data-ttu-id="c173e-154">IPython biztosít egy segédprogram ehhez; a parancssorba a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="c173e-154">IPython provides a utility to do so; at a command prompt run the following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="c173e-155">Kérni fogja a jelszó és a megerősítés, és azt nyomtassa ki a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c173e-155">This will prompt you for a password and confirmation, and will then print the password.</span></span> <span data-ttu-id="c173e-156">Megjegyzés: Ez a következő lépéssel.</span><span class="sxs-lookup"><span data-stu-id="c173e-156">Note this for the following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided the rest for security)

<span data-ttu-id="c173e-157">A következő fogjuk módosítani a profil konfigurációs fájl, amely a `jupyter_notebook_config.py` fájl a könyvtárban.</span><span class="sxs-lookup"><span data-stu-id="c173e-157">Next, we will edit the profile's configuration file, which is the `jupyter_notebook_config.py` file in the directory you are in.</span></span>  <span data-ttu-id="c173e-158">Vegye figyelembe, hogy a fájl nem létezik – most létrehozták a esetben.</span><span class="sxs-lookup"><span data-stu-id="c173e-158">Note that this file may not exist -- just create it if that is the case.</span></span>  <span data-ttu-id="c173e-159">Ez a fájl rendelkezik mezők számát, és alapértelmezés szerint az összes jelenleg megjegyzésként szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="c173e-159">This file has a number of fields and by default all are commented out.</span></span>  <span data-ttu-id="c173e-160">Ez a fájl megnyitható tetszőlegesen a szövegszerkesztőben, és bizonyosodjon meg arról, hogy van legalább a következő tartalmat.</span><span class="sxs-lookup"><span data-stu-id="c173e-160">You can open this file with any text editor of your liking, and you should ensure that it has at least the following content.</span></span> <span data-ttu-id="c173e-161">**Ügyeljen arra, hogy a Config c.NotebookApp.password cserélje le az előző lépésben sha1**.</span><span class="sxs-lookup"><span data-stu-id="c173e-161">**Be sure to replace the c.NotebookApp.password in the config with the sha1 from the previous step**.</span></span>

    c = get_config()

    # You must give the path to the certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-the-jupyter-notebook"></a><span data-ttu-id="c173e-162">Futtassa a Jupyter Notebook</span><span class="sxs-lookup"><span data-stu-id="c173e-162">Run the Jupyter Notebook</span></span>
<span data-ttu-id="c173e-163">Ezen a ponton a Jupyter Notebook készen áll azt.</span><span class="sxs-lookup"><span data-stu-id="c173e-163">At this point we are ready to start the Jupyter Notebook.</span></span> <span data-ttu-id="c173e-164">Ehhez keresse meg a kívánt jegyzetfüzeteket tárolja, és indítsa el a Jupyter Notebook kiszolgáló a következő paranccsal könyvtárát.</span><span class="sxs-lookup"><span data-stu-id="c173e-164">To do this, navigate to the directory you want to store notebooks in and start the Jupyter Notebook server with the following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="c173e-165">Meg kell tudni elérni a Jupyter Notebook a címen `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="c173e-165">You should now be able to access your Jupyter Notebook at the address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="c173e-166">Amikor először hozzáfér a notebook, a bejelentkezési oldal kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="c173e-166">When you first access your notebook, the login page asks for your password.</span></span> <span data-ttu-id="c173e-167">És miután jelentkezik be, megjelenik a "Jupyter Notebook irányítópult", amely az összes jegyzetfüzet művelet vezérlőkarakterek központnak.</span><span class="sxs-lookup"><span data-stu-id="c173e-167">And once you log in, you will see the "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="c173e-168">Ezen a lapon létrehozhat új, jegyzetfüzeteket és nyissa meg a már meglévőket.</span><span class="sxs-lookup"><span data-stu-id="c173e-168">From this page you can create new notebooks and open existing ones.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-the-jupyter-notebook"></a><span data-ttu-id="c173e-170">A Jupyter Notebook használatával</span><span class="sxs-lookup"><span data-stu-id="c173e-170">Using the Jupyter Notebook</span></span>
<span data-ttu-id="c173e-171">Ha a **új** gomb, látni fogja a következő lap megnyitása.</span><span class="sxs-lookup"><span data-stu-id="c173e-171">If you click the **New** button, you will see the following opening page.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="c173e-173">A terület jelölésű egy `In []:` kérdezzen rá a beviteli terület, és itt adhatja meg egyetlen érvényes Python kódját, és azt fogja hajtható végre, ha kattint `Shift-Enter` vagy kattintson a "Play" (a jobbra mutató háromszög az eszköztárban) ikonra.</span><span class="sxs-lookup"><span data-stu-id="c173e-173">The area marked with an `In []:` prompt is the input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on the "Play" icon (the right-pointing triangle in the toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="c173e-174">A hatékony paradigma: élő multimédiás számítási dokumentumok</span><span class="sxs-lookup"><span data-stu-id="c173e-174">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="c173e-175">A notebook magának kell érzi, hogy nagyon természetes számára, akik használják a Python és a szövegszerkesztőt, mert bizonyos értelemben vegyesen mindkét: végrehajthat kódblokkokat, Python, de is megtarthatja megjegyzések és egyéb szöveg egy cella "Markdown" a "Kód" stílus módosításával az eszköztáron a legördülő menü segítségével.</span><span class="sxs-lookup"><span data-stu-id="c173e-175">The notebook itself should feel very natural to anyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing the style of a cell from "Code" to "Markdown" using the drop-down menu in the toolbar.</span></span>

<span data-ttu-id="c173e-176">Jupyter sokkal több mint egy szövegszerkesztőt, lehetővé teszi a számítási és a gazdag media (szöveg, képek, videó és gyakorlatilag bármi modern webböngésző megjelenítheti) keverése.</span><span class="sxs-lookup"><span data-stu-id="c173e-176">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="c173e-177">Kombinálhatja, szöveg, kód, videókat és egyéb!</span><span class="sxs-lookup"><span data-stu-id="c173e-177">You can mix, text, code, videos and more!</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="c173e-179">És a Python tartozó sok kiváló szalagtárak műszaki és tudományos power számítástechnikai, az alábbi képernyőképen egyszerű számítás hajtható végre egy összetett hálózati elemzést követően az összes egy környezetben, azonos könnyű.</span><span class="sxs-lookup"><span data-stu-id="c173e-179">And with the power of Python's many excellent libraries for scientific and technical computing, in the following screenshot, a simple calculation can be performed with the same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="c173e-180">Ez a modern webes hatványa keverési az élő számítási paradigma számos lehetőséget kínál, és kiválóan alkalmas a felhőalapú; a Notebook használhatók:</span><span class="sxs-lookup"><span data-stu-id="c173e-180">This paradigm of mixing the power of the modern web with live computation offers many possibilities, and is ideally suited for the cloud; the Notebook can be used:</span></span>

* <span data-ttu-id="c173e-181">A számítási firkatömb felderítő rögzítéséhez, működnek. a probléma.</span><span class="sxs-lookup"><span data-stu-id="c173e-181">As a computational scratchpad to record exploratory work on a problem.</span></span>
* <span data-ttu-id="c173e-182">Osztani az eredményeket, munkatársakat, vagy a "live" számítási formában, vagy vezetékhelyettesítési formátumokban (HTML, PDF).</span><span class="sxs-lookup"><span data-stu-id="c173e-182">To share results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="c173e-183">Terjesztéséhez, és élő oktatási anyagok, például a számítási, így a diákok azonnal kísérletezhet, és a tényleges kód van, módosítsa azt, és hajtsa végre újból az interaktív módon.</span><span class="sxs-lookup"><span data-stu-id="c173e-183">To distribute and present live teaching materials that involve computation, so students can immediately experiment with the real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="c173e-184">Arra, hogy a "végrehajtható által írt cikkeket", hogy az eredmények kutatási oly módon, hogy melyek azonnal másolható, érvényesítése és bővítése mások számára.</span><span class="sxs-lookup"><span data-stu-id="c173e-184">To provide "executable papers" that present the results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="c173e-185">Az együttműködési számítástechnikai platformként: több felhasználó bejelentkezhet egy élő számítási munkamenet megosztásához ugyanazon notebook a kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="c173e-185">As a platform for collaborative computing: multiple users can log in to the same notebook server to share a live computational session.</span></span>

<span data-ttu-id="c173e-186">Ha a IPython forráskód [tárház][repository], töltse le, és a saját Azure Jupyter virtuális gépen kísérletezhet notebook példák egy teljes könyvtár található.</span><span class="sxs-lookup"><span data-stu-id="c173e-186">If you go to the IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="c173e-187">Egyszerűen csak töltse le a `.ipynb` -fájlok a helyről, és feltöltheti ezeket a notebook Azure virtuális gép irányítópultján alakzatot (vagy letöltheti a fájlokat közvetlenül a virtuális gép).</span><span class="sxs-lookup"><span data-stu-id="c173e-187">Simply download the `.ipynb` files from the site and upload them onto the dashboard of your notebook Azure VM (or download them directly into the VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="c173e-188">Összegzés</span><span class="sxs-lookup"><span data-stu-id="c173e-188">Conclusion</span></span>
<span data-ttu-id="c173e-189">A Jupyter Notebook hatékony felületet biztosít a hatványra emelésének Azure a Python ökoszisztémájának párbeszédes formában történő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c173e-189">The Jupyter Notebook provides a powerful interface for accessing interactively the power of the Python ecosystem on Azure.</span></span>  <span data-ttu-id="c173e-190">Használati esetek beleértve egyszerű feltárása és Python, adatelemzés és a képi megjelenítés, szimuláció és párhuzamos számítástechnikai számos magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="c173e-190">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="c173e-191">Az eredményül kapott Notebook dokumentumok tartalmazzák a hajtja végre, és más Jupyter felhasználókkal való megosztás számításokat teljes rekordja.</span><span class="sxs-lookup"><span data-stu-id="c173e-191">The resulting Notebook documents contain a complete record of the computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="c173e-192">A Jupyter Notebook egy helyi alkalmazás használható, de ez akkor ajánlott, ha az Azure felhőben történő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="c173e-192">The Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="c173e-193">A core Jupyter számára is elérhető belül a Visual Studio használatával a [a Python Tools for Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="c173e-193">The core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="c173e-194">PTVS egy ingyenes és nyílt forráskódú beépülő modul a Microsoft Visual Studio be olyan speciális Python fejlesztői környezetben, amely tartalmaz egy speciális szerkesztőt az IntelliSense hibakeresési információ-profilkészítési és párhuzamos számítástechnikai integráció.</span><span class="sxs-lookup"><span data-stu-id="c173e-194">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c173e-195">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c173e-195">Next steps</span></span>
<span data-ttu-id="c173e-196">További információ: [Python fejlesztői központban](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="c173e-196">For more information, see the [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
