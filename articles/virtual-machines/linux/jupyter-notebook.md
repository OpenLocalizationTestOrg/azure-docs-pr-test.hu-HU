---
title: egy Jupyter/IPython Notebook aaaCreate |} Microsoft Docs
description: "Ismerje meg, hogyan toodeploy hello Jupyter/IPython Notebook Linux virtuális gépeken létre hello resource manager üzembe helyezési modellben az Azure-ban."
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
ms.openlocfilehash: d7f2e45a8ba95163ebfb0f10babc91a2b3fd9390
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-vm-installing-jupyter-and-running-a-jupyter-notebook-on-azure"></a><span data-ttu-id="5da0e-103">Egy Azure virtuális gép létrehozása, Jupyter telepítése és futtatása a Jupyter Notebook az Azure-on</span><span class="sxs-lookup"><span data-stu-id="5da0e-103">Creating an Azure VM, installing Jupyter, and running a Jupyter Notebook on Azure</span></span>
<span data-ttu-id="5da0e-104">Hello [Jupyter projekt](http://jupyter.org), korábbi nevén hello [IPython projekt](http://ipython.org), olyan eszközöket biztosít tudományos számítástechnikai kód végrehajtását hello létrehozását a felhasználó hatékony interaktív rendszerhéjának használatával egy élő számítási dokumentumot.</span><span class="sxs-lookup"><span data-stu-id="5da0e-104">hello [Jupyter project](http://jupyter.org), formerly hello [IPython project](http://ipython.org), provides a collection of tools for scientific computing using powerful interactive shells that combine code execution with hello creation of a live computational document.</span></span> <span data-ttu-id="5da0e-105">A notebook fájlok tetszőleges szöveg, matematikai képletek, bemeneti kódot, eredményeket, grafikus, videók és bármely más típusú, hogy a modern webböngésző megjelenítésére alkalmas adathordozó is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5da0e-105">These notebook files can contain arbitrary text, mathematical formulas, input code, results, graphics, videos and any other kind of media that a modern web browser is capable of displaying.</span></span> <span data-ttu-id="5da0e-106">E teljesen új tooPython van, és szeretné, hogy toolearn visszatöltött, interaktív környezetben vagy néhány súlyos párhuzamos és technikai computing, hello Jupyter Notebook kiváló választás tegye azt.</span><span class="sxs-lookup"><span data-stu-id="5da0e-106">Whether you're absolutely new tooPython and want toolearn it in a fun, interactive environment or do some serious parallel/technical computing, hello Jupyter Notebook is a great choice.</span></span>

<span data-ttu-id="5da0e-107">![Képernyőkép](./media/jupyter-notebook/ipy-notebook-spectral.png) használatával SciPy és Matplotlib csomagok tooanalyze hello szerkezete egy Hangrögzítés.</span><span class="sxs-lookup"><span data-stu-id="5da0e-107">![Screenshot](./media/jupyter-notebook/ipy-notebook-spectral.png) Using SciPy and Matplotlib packages tooanalyze hello structure of a sound recording.</span></span>

## <a name="jupyter-two-ways-azure-notebooks-or-custom-deployment"></a><span data-ttu-id="5da0e-108">Jupyter két módon: Azure notebookok vagy egyéni központi telepítés</span><span class="sxs-lookup"><span data-stu-id="5da0e-108">Jupyter Two Ways: Azure Notebooks or Custom Deployment</span></span>
<span data-ttu-id="5da0e-109">Túl is használhatja a szolgáltatást biztosít az Azure[Jupyter használatának gyors megkezdéséhez ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span><span class="sxs-lookup"><span data-stu-id="5da0e-109">Azure provides a service that you can use too[quickly start using Jupyter ](http://blogs.technet.com/b/machinelearning/archive/2015/07/24/introducing-jupyter-notebooks-in-azure-ml-studio.aspx).</span></span>  <span data-ttu-id="5da0e-110">Hello Azure Notebook szolgáltatás használatával egyszerűen ettől kezdve a hozzáférési tooJupyter interneten elérhető felület tartalmazó összes hello power a Python és a sok szalagtárak méretezhető számítási erőforrások.</span><span class="sxs-lookup"><span data-stu-id="5da0e-110">By using hello Azure Notebook Service, you can easily gain access tooJupyter's web-accessible interface to scalable computational resources with all hello power of Python and its many libraries.</span></span>  <span data-ttu-id="5da0e-111">Hello telepítési hello szolgáltatás kezeli, mivel a felhasználók elérhetik ezeket az erőforrásokat hello felügyelete és konfigurálása hello felhasználó nélkül.</span><span class="sxs-lookup"><span data-stu-id="5da0e-111">Since hello installation is handled by hello service, users can access these resources without hello need for administration and configuration by hello user.</span></span>

<span data-ttu-id="5da0e-112">Ha hello notebook szolgáltatás nem működik a forgatókönyvnek ezentúl tooread ebben a cikkben, amely fog bemutatja, hogyan toodeploy hello Jupyter Notebook a Microsoft Azure Linux virtuális gépek (VM) használatával.</span><span class="sxs-lookup"><span data-stu-id="5da0e-112">If hello notebook service does not work for your scenario please continue tooread this article which will will show you how toodeploy hello Jupyter Notebook on Microsoft Azure, using Linux virtual machines (VMs).</span></span>

[!INCLUDE [create-account-and-vms-note](../../../includes/create-account-and-vms-note.md)]

## <a name="create-and-configure-a-vm-on-azure"></a><span data-ttu-id="5da0e-113">Hozza létre és konfigurálja a virtuális gépek az Azure-on</span><span class="sxs-lookup"><span data-stu-id="5da0e-113">Create and configure a VM on Azure</span></span>
<span data-ttu-id="5da0e-114">első lépés hello toocreate van Azure-on futó virtuális gép (VM).</span><span class="sxs-lookup"><span data-stu-id="5da0e-114">hello first step is toocreate a virtual machine (VM)  running on Azure.</span></span>
<span data-ttu-id="5da0e-115">Ez a virtuális gép egy teljes operációs rendszer hello felhőben, és hello Jupyter Notebook futtatásához használandó.</span><span class="sxs-lookup"><span data-stu-id="5da0e-115">This VM is a complete operating system in hello cloud and will be used to run hello Jupyter Notebook.</span></span> <span data-ttu-id="5da0e-116">Azure képes a Linux és a Windows virtuális gépek futtatását, és bemutatjuk, hogy a virtuális gépek mindkét fajtája Jupyter hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="5da0e-116">Azure is capable of running both Linux and Windows virtual machines, and we will cover hello setup of Jupyter on both types of virtual machines.</span></span>

### <a name="create-a-linux-vm-and-open-a-port-for-jupyter"></a><span data-ttu-id="5da0e-117">Hozzon létre egy Linux virtuális Gépet, és a Jupyter port megnyitása</span><span class="sxs-lookup"><span data-stu-id="5da0e-117">Create a Linux VM and open a port for Jupyter</span></span>
<span data-ttu-id="5da0e-118">Hajtsa végre az adott hello utasítások [Itt] [ portal-vm-linux] toocreate hello a virtuális gép *Ubuntu* terjesztési.</span><span class="sxs-lookup"><span data-stu-id="5da0e-118">Follow hello instructions given [here][portal-vm-linux] toocreate a virtual machine of hello *Ubuntu* distribution.</span></span> <span data-ttu-id="5da0e-119">Ez az oktatóanyag Ubuntu Server 14.04 LTS használja.</span><span class="sxs-lookup"><span data-stu-id="5da0e-119">This tutorial uses Ubuntu Server 14.04 LTS.</span></span> <span data-ttu-id="5da0e-120">Feltételezzük, hogy hello felhasználónév *azureuser*.</span><span class="sxs-lookup"><span data-stu-id="5da0e-120">We'll assume hello user name *azureuser*.</span></span>

<span data-ttu-id="5da0e-121">Miután telepíti a virtuális gép hello igazolnia kell a tooopen hello hálózati biztonsági csoport biztonsági szabály mentése.</span><span class="sxs-lookup"><span data-stu-id="5da0e-121">After hello virtual machine deploys we need tooopen up a security rule on hello network security group.</span></span>  <span data-ttu-id="5da0e-122">Hello Azure-portálon, a go túl**hálózati biztonsági csoportok** és hello biztonsági csoport megfelelő tooyour VM nyitott hello lapján.</span><span class="sxs-lookup"><span data-stu-id="5da0e-122">From hello Azure portal, go too**Network Security Groups** and open hello tab for hello Security Group corresponding tooyour VM.</span></span> <span data-ttu-id="5da0e-123">Egy bejövő biztonsági szabály tooadd van szüksége a következő beállítások hello: **TCP** hello protokoll  **\***  a hello forrásport (nyilvános) és **9999** a hello (személyes) célport.</span><span class="sxs-lookup"><span data-stu-id="5da0e-123">You need tooadd an Inbound Security rule with hello following settings: **TCP** for hello protocol, **\*** for hello source (public) port and **9999** for hello destination (private) port.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/azure-add-endpoint.png)

<span data-ttu-id="5da0e-125">Az a hálózati biztonsági csoporthoz, kattintson a **hálózati illesztők** és megjegyzés hello **nyilvános IP-cím** , szükséges tooconnect tooyour VM hello következő lépésben lesz.</span><span class="sxs-lookup"><span data-stu-id="5da0e-125">While in your Network Security Group, click on **Network Interfaces** and note hello **Public IP Address** as it will be needed tooconnect tooyour VM in hello next step.</span></span>

## <a name="install-required-software-on-hello-vm"></a><span data-ttu-id="5da0e-126">Szükséges szoftverek telepítését hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="5da0e-126">Install required software on hello VM</span></span>
<span data-ttu-id="5da0e-127">toorun hello Jupyter Notebook a virtuális gépen, azt először telepítenie kell Jupyter és annak függőségeit.</span><span class="sxs-lookup"><span data-stu-id="5da0e-127">toorun hello Jupyter Notebook on our VM, we must first install Jupyter and its dependencies.</span></span> <span data-ttu-id="5da0e-128">Csatlakozás tooyour linux virtuális gép ssh, és úgy döntött, hogy mikor létre hello felhasználónév/jelszó pár hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="5da0e-128">Connect tooyour linux vm using ssh and hello username/password pair you chose when you created hello vm.</span></span> <span data-ttu-id="5da0e-129">Ez az oktatóanyag a rendszer a PuTTY használata és csatlakozás Windows.</span><span class="sxs-lookup"><span data-stu-id="5da0e-129">In this tutorial we will use PuTTY and connect from Windows.</span></span>

### <a name="installing-jupyter-on-ubuntu"></a><span data-ttu-id="5da0e-130">Ubuntu Jupyter telepítése</span><span class="sxs-lookup"><span data-stu-id="5da0e-130">Installing Jupyter on Ubuntu</span></span>
<span data-ttu-id="5da0e-131">Anaconda, olyan népszerű adatok tudományos python elosztási, a megadott hello hivatkozások valamelyikével telepítése [alakíthatnak ki olyan Analytics](https://www.continuum.io/downloads).</span><span class="sxs-lookup"><span data-stu-id="5da0e-131">Install Anaconda, a popular data science python distribution, using one of hello links provided from [Continuum Analytics](https://www.continuum.io/downloads).</span></span>  <span data-ttu-id="5da0e-132">Frissítésétől a jelen dokumentum írása hello, hello az alábbi hivatkozások olyan hello legtöbb toodate verziók fel.</span><span class="sxs-lookup"><span data-stu-id="5da0e-132">As of hello writing of this document, hello below links are hello most up toodate versions.</span></span>

#### <a name="anaconda-installs-for-linux"></a><span data-ttu-id="5da0e-133">Linux anaconda telepíti</span><span class="sxs-lookup"><span data-stu-id="5da0e-133">Anaconda Installs for Linux</span></span>
<table>
  <th><span data-ttu-id="5da0e-134">Python 3.4</span><span class="sxs-lookup"><span data-stu-id="5da0e-134">Python 3.4</span></span></th>
  <th><span data-ttu-id="5da0e-135">Python 2.7-es</span><span class="sxs-lookup"><span data-stu-id="5da0e-135">Python 2.7</span></span></th>
  <tr>
    <td><span data-ttu-id="5da0e-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="5da0e-136">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
    <td><span data-ttu-id="5da0e-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="5da0e-137">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86_64.sh'>64 bit</href>
    </span></span></td>
  </tr>
  <tr>
    <td><span data-ttu-id="5da0e-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="5da0e-138">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>
    <td><span data-ttu-id="5da0e-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bites</href>
    </span><span class="sxs-lookup"><span data-stu-id="5da0e-139">
        <a href='https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda-2.3.0-Linux-x86.sh'>32 bit</href>
    </span></span></td>  
  </tr>
</table>


#### <a name="installing-anaconda3-230-64-bit-on-ubuntu"></a><span data-ttu-id="5da0e-140">Anaconda3 telepítése 2.3.0 Ubuntu 64 bites</span><span class="sxs-lookup"><span data-stu-id="5da0e-140">Installing Anaconda3 2.3.0 64-bit on Ubuntu</span></span>
<span data-ttu-id="5da0e-141">Például azt, hogyan telepíthető Anaconda Ubuntu</span><span class="sxs-lookup"><span data-stu-id="5da0e-141">As an example, this is how you can install Anaconda on Ubuntu</span></span>

    # install anaconda
    cd ~
    mkdir -p anaconda
    cd anaconda/
    curl -O https://3230d63b5fc54e62148e-c95ac804525aac4b6dba79b00b39d1d3.ssl.cf1.rackcdn.com/Anaconda3-2.3.0-Linux-x86_64.sh
    sudo bash Anaconda3-2.3.0-Linux-x86_64.sh -b -f -p /anaconda3

    # clean up home directory
    cd ..
    rm -rf anaconda/

    # Update Jupyter toohello latest install and generate its config file
    sudo /anaconda3/bin/conda install jupyter -y
    /anaconda3/bin/jupyter-notebook --generate-config


![Képernyőfelvétel](./media/jupyter-notebook/anaconda-install.png)

### <a name="configuring-jupyter-and-using-ssl"></a><span data-ttu-id="5da0e-143">Jupyter konfigurálása és az SSL használata</span><span class="sxs-lookup"><span data-stu-id="5da0e-143">Configuring Jupyter and using SSL</span></span>
<span data-ttu-id="5da0e-144">Telepítése után kell tootake egy kicsit toosetup hello konfigurációs fájlokat a Jupyter.</span><span class="sxs-lookup"><span data-stu-id="5da0e-144">After installing we need tootake a moment toosetup hello configuration files for Jupyter.</span></span> <span data-ttu-id="5da0e-145">A Jupyter konfigurálása során fellépő problémák tapasztal lehet hasznos toolook: hello [Jupyter Notebook-kiszolgálót futtató dokumentációja](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span><span class="sxs-lookup"><span data-stu-id="5da0e-145">If you experience troubles with configuring Jupyter it may be helpful toolook at hello [Jupyter Documentation for Running a Notebook Server](http://jupyter-notebook.readthedocs.org/en/latest/public_server.html).</span></span>

<span data-ttu-id="5da0e-146">Tovább a Microsoft `cd` toohello profil directory toocreate az SSL-tanúsítványt, és szerkesztheti a hello-profil konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="5da0e-146">Next we `cd` toohello profile directory toocreate our SSL certificate and edit hello profiles configuration file.</span></span>

<span data-ttu-id="5da0e-147">Linux hello a következő parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="5da0e-147">On Linux use hello following command.</span></span>

    cd ~/.jupyter

<span data-ttu-id="5da0e-148">A következő parancs toocreate hello SSL-tanúsítvány (Linux és Windows rendszerekhez) hello használata.</span><span class="sxs-lookup"><span data-stu-id="5da0e-148">Use hello following command toocreate hello SSL certificate(Linux and Windows).</span></span>

    openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem

<span data-ttu-id="5da0e-149">Vegye figyelembe, hogy azóta létrehozzuk önaláírt SSL-tanúsítvány toohello notebook csatlakozáskor a böngésző biztonsági figyelmeztetést ad meg.</span><span class="sxs-lookup"><span data-stu-id="5da0e-149">Note that since we are creating a self-signed SSL certificate, when connecting toohello notebook your browser will give you a security warning.</span></span>  <span data-ttu-id="5da0e-150">Hosszú távú éles környezetben való használathoz érdemes toouse a szervezetéhez tartozó megfelelően aláírt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="5da0e-150">For long-term production use, you will want toouse a properly signed certificate associated with your organization.</span></span>  <span data-ttu-id="5da0e-151">Tanúsítványkezelés nem ebben a bemutatóban a hello terjed, mivel most azt fogja odatapadjon tooa önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="5da0e-151">Since certificate management is beyond hello scope of this demo, we will stick tooa self-signed certificate for now.</span></span>

<span data-ttu-id="5da0e-152">Ezenkívül toousing egy tanúsítványt, meg kell adni egy jelszót tooprotect a notebook jogosulatlan használatát.</span><span class="sxs-lookup"><span data-stu-id="5da0e-152">In addition toousing a certificate, you must also provide a password tooprotect your notebook from unauthorized use.</span></span>  <span data-ttu-id="5da0e-153">Biztonsági okokból Jupyter titkosított jelszavakat használ a konfigurációs fájlban, ezért tooencrypt kell a jelszót először.</span><span class="sxs-lookup"><span data-stu-id="5da0e-153">For security reasons Jupyter uses encrypted passwords in its configuration file, so you'll need tooencrypt your password first.</span></span>  <span data-ttu-id="5da0e-154">IPython biztosít egy segédprogram toodo a parancssorba futtassa a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="5da0e-154">IPython provides a utility toodo so; at a command prompt run hello following command.</span></span>

    /anaconda3/bin/python -c "import IPython;print(IPython.lib.passwd())"

<span data-ttu-id="5da0e-155">Kérni fogja a jelszó és a megerősítés, és azt nyomtassa ki a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="5da0e-155">This will prompt you for a password and confirmation, and will then print hello password.</span></span> <span data-ttu-id="5da0e-156">Megjegyzés: Ez a következő lépés hello.</span><span class="sxs-lookup"><span data-stu-id="5da0e-156">Note this for hello following step.</span></span>

    Enter password:
    Verify password:
    sha1:b86e933199ad:a02e9592e59723da722.. (elided hello rest for security)

<span data-ttu-id="5da0e-157">A következő fogjuk módosítani hello-profil konfigurációs fájl, amely a `jupyter_notebook_config.py` fájl hello használata során.</span><span class="sxs-lookup"><span data-stu-id="5da0e-157">Next, we will edit hello profile's configuration file, which is the `jupyter_notebook_config.py` file in hello directory you are in.</span></span>  <span data-ttu-id="5da0e-158">Vegye figyelembe, hogy a fájl nem létezik – most létrehozták hello esetben.</span><span class="sxs-lookup"><span data-stu-id="5da0e-158">Note that this file may not exist -- just create it if that is hello case.</span></span>  <span data-ttu-id="5da0e-159">Ez a fájl rendelkezik mezők számát, és alapértelmezés szerint az összes jelenleg megjegyzésként szerepelnek.  Ez a fájl megnyitható tetszőlegesen a szövegszerkesztőben, és biztosítania kell legalább rendelkezik hello következő tartalom.</span><span class="sxs-lookup"><span data-stu-id="5da0e-159">This file has a number of fields and by default all are commented out.  You can open this file with any text editor of your liking, and you should ensure that it has at least hello following content.</span></span> <span data-ttu-id="5da0e-160">**Lehet, hogy tooreplace hello c.NotebookApp.password hello config az előző lépésben hello hello sha1**.</span><span class="sxs-lookup"><span data-stu-id="5da0e-160">**Be sure tooreplace hello c.NotebookApp.password in hello config with hello sha1 from hello previous step**.</span></span>

    c = get_config()

    # You must give hello path toohello certificate file.
    c.NotebookApp.certfile = u'/home/azureuser/.jupyter/mycert.pem'

    # Create your own password as indicated above
    c.NotebookApp.password = u'sha1:b86e933199ad:a02e9592e5 etc... '

    # Network and browser details. We use a fixed port (9999) so it matches
    # our Azure setup, where we've allowed traffic on that port
    c.NotebookApp.ip = '*'
    c.NotebookApp.port = 9999
    c.NotebookApp.open_browser = False

### <a name="run-hello-jupyter-notebook"></a><span data-ttu-id="5da0e-161">Jupyter Notebook hello futtatása</span><span class="sxs-lookup"><span data-stu-id="5da0e-161">Run hello Jupyter Notebook</span></span>
<span data-ttu-id="5da0e-162">Ekkor készen áll a toostart hello Jupyter Notebook folyamatban.</span><span class="sxs-lookup"><span data-stu-id="5da0e-162">At this point we are ready toostart hello Jupyter Notebook.</span></span> <span data-ttu-id="5da0e-163">toodo, keresse meg a kívánt toostore notebookok, és indítsa el a következő parancs hello hello Jupyter Notebook server toohello directory.</span><span class="sxs-lookup"><span data-stu-id="5da0e-163">toodo this, navigate toohello directory you want toostore notebooks in and start hello Jupyter Notebook server with hello following command.</span></span>

    /anaconda3/bin/jupyter-notebook

<span data-ttu-id="5da0e-164">Most meg kell tudni tooaccess a Jupyter Notebook hello címen `https://[PUBLIC-IP-ADDRESS]:9999`.</span><span class="sxs-lookup"><span data-stu-id="5da0e-164">You should now be able tooaccess your Jupyter Notebook at hello address `https://[PUBLIC-IP-ADDRESS]:9999`.</span></span>

<span data-ttu-id="5da0e-165">A notebook első megnyitásakor hello bejelentkezési oldal kéri a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5da0e-165">When you first access your notebook, hello login page asks for your password.</span></span> <span data-ttu-id="5da0e-166">És jelentkezik be, miután látni fog hello "Jupyter Notebook irányítópult", amely az összes jegyzetfüzet művelet vezérlőkarakterek központnak.</span><span class="sxs-lookup"><span data-stu-id="5da0e-166">And once you log in, you will see hello "Jupyter Notebook Dashboard", which is the ontrol center for all notebook operations.</span></span>  <span data-ttu-id="5da0e-167">Ezen a lapon létrehozhat új, jegyzetfüzeteket és nyissa meg a már meglévőket.</span><span class="sxs-lookup"><span data-stu-id="5da0e-167">From this page you can create new notebooks and open existing ones.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-tree-view.png)

### <a name="using-hello-jupyter-notebook"></a><span data-ttu-id="5da0e-169">Hello Jupyter Notebook használatával</span><span class="sxs-lookup"><span data-stu-id="5da0e-169">Using hello Jupyter Notebook</span></span>
<span data-ttu-id="5da0e-170">Ha hello **új** gomb, látni fogja a következő lap megnyitása hello.</span><span class="sxs-lookup"><span data-stu-id="5da0e-170">If you click hello **New** button, you will see hello following opening page.</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-untitled-notebook.png)

<span data-ttu-id="5da0e-172">hello terület jelölésű egy `In []:` parancssorból hello beviteli terület, és itt adhatja meg bármelyik érvényes Python-kód, és akkor lesz hajtható végre, ha kattint `Shift-Enter` vagy kattintson a hello "Play" ikonra (hello jobbra mutató háromszög hello eszköztár).</span><span class="sxs-lookup"><span data-stu-id="5da0e-172">hello area marked with an `In []:` prompt is hello input area, and here you can type any valid Python code and it will execute when you hit `Shift-Enter` or click on hello "Play" icon (hello right-pointing triangle in hello toolbar).</span></span>

## <a name="a-powerful-paradigm-live-computational-documents-with-rich-media"></a><span data-ttu-id="5da0e-173">A hatékony paradigma: élő multimédiás számítási dokumentumok</span><span class="sxs-lookup"><span data-stu-id="5da0e-173">A powerful paradigm: live computational documents with rich media</span></span>
<span data-ttu-id="5da0e-174">hello notebook magának kell érzi, hogy nagyon természetes tooanyone, akik használják a Python és a szövegszerkesztőt, mert bizonyos értelemben vegyesen mindkét: végrehajthat kódblokkokat, Python, de is megtarthatja megjegyzések és egyéb szöveg túl egy cella "Kódból" hello stílus módosításával "Ma rkdown"hello legördülő menü segítségével az eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="5da0e-174">hello notebook itself should feel very natural tooanyone who has used Python and a word processor, because it is in some ways a mix of both: you can execute blocks of Python code, but you can also keep notes and other text by changing hello style of a cell from "Code" too"Markdown" using hello drop-down menu in the toolbar.</span></span>

<span data-ttu-id="5da0e-175">Jupyter sokkal több mint egy szövegszerkesztőt, lehetővé teszi a számítási és a gazdag media (szöveg, képek, videó és gyakorlatilag bármi modern webböngésző megjelenítheti) keverése.</span><span class="sxs-lookup"><span data-stu-id="5da0e-175">Jupyter is much more than a word processor as it allows the mixing of computation and rich media (text, graphics, video and virtually anything a modern web browser can display).</span></span> <span data-ttu-id="5da0e-176">Kombinálhatja, szöveg, kód, videókat és egyéb!</span><span class="sxs-lookup"><span data-stu-id="5da0e-176">You can mix, text, code, videos and more!</span></span>

![Képernyőfelvétel](./media/jupyter-notebook/jupyter-editing-experience.png)

<span data-ttu-id="5da0e-178">És a Python tartozó sok kiváló-könyvtárakban műszaki és tudományos számítógép-használatról, a következő képernyőfelvételen látható hello hello hatványa egyszerű számítás hajtható végre hello ugyanaz, mint egy összetett hálózati elemző egy környezetben megkönnyítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="5da0e-178">And with hello power of Python's many excellent libraries for scientific and technical computing, in hello following screenshot, a simple calculation can be performed with hello same ease as a complex network analysis, all in one environment.</span></span>

<span data-ttu-id="5da0e-179">A modern webes hello hello hatványa keverése élő számítási ez paradigma számos lehetőséget kínál, és kiválóan alkalmas hello felhő; hello Notebook használhatók:</span><span class="sxs-lookup"><span data-stu-id="5da0e-179">This paradigm of mixing hello power of hello modern web with live computation offers many possibilities, and is ideally suited for hello cloud; hello Notebook can be used:</span></span>

* <span data-ttu-id="5da0e-180">A felderítő számítási firkatömb toorecord, működnek. a probléma.</span><span class="sxs-lookup"><span data-stu-id="5da0e-180">As a computational scratchpad toorecord exploratory work on a problem.</span></span>
* <span data-ttu-id="5da0e-181">tooshare eredmények munkatársaival, "élő" számítási formában vagy vezetékhelyettesítési formátumokban (HTML, PDF).</span><span class="sxs-lookup"><span data-stu-id="5da0e-181">tooshare results with colleagues, either in 'live' computational form or in hardcopy formats (HTML, PDF).</span></span>
* <span data-ttu-id="5da0e-182">toodistribute és a jelen élő oktatóanyag, számítási, például az, a diákok azonnal kísérletezhet, hello valós kódot, és módosítsa azt, és hajtsa végre újból az interaktív módon.</span><span class="sxs-lookup"><span data-stu-id="5da0e-182">toodistribute and present live teaching materials that involve computation, so students can immediately experiment with hello real code, modify it and re-execute it interactively.</span></span>
* <span data-ttu-id="5da0e-183">"végrehajtható által írt cikkeket", hogy kutatási hello eredményeit, hogy azonnal reprodukálható, tooprovide érvényesítve, valamint a kiterjesztett mások számára.</span><span class="sxs-lookup"><span data-stu-id="5da0e-183">tooprovide "executable papers" that present hello results of research in a way that can be immediately reproduced, validated and extended by others.</span></span>
* <span data-ttu-id="5da0e-184">Az együttműködési számítástechnikai platformként: több felhasználó is bejelentkezhetnek toothe azonos notebook server tooshare élő számítási munkamenet.</span><span class="sxs-lookup"><span data-stu-id="5da0e-184">As a platform for collaborative computing: multiple users can log in toothe same notebook server tooshare a live computational session.</span></span>

<span data-ttu-id="5da0e-185">Ha toohello IPython forráskód [tárház][repository], töltse le, és a saját Azure Jupyter virtuális gépen kísérletezhet notebook példák egy teljes könyvtár talál.</span><span class="sxs-lookup"><span data-stu-id="5da0e-185">If you go toohello IPython source code [repository][repository], you will find an entire directory with notebook examples which you can download and then experiment with on your own Azure Jupyter VM.</span></span>  <span data-ttu-id="5da0e-186">Egyszerűen csak töltse le a hello `.ipynb` hello fájlok hely és feltöltésükhöz alakzatot hello irányítópult a jegyzetfüzet Azure virtuális gép (vagy letöltheti a fájlokat közvetlenül a virtuális gép hello).</span><span class="sxs-lookup"><span data-stu-id="5da0e-186">Simply download hello `.ipynb` files from hello site and upload them onto hello dashboard of your notebook Azure VM (or download them directly into hello VM).</span></span>

## <a name="conclusion"></a><span data-ttu-id="5da0e-187">Összegzés</span><span class="sxs-lookup"><span data-stu-id="5da0e-187">Conclusion</span></span>
<span data-ttu-id="5da0e-188">Jupyter Notebook hello hatékony felületet biztosít a hello hatványra emelésének Azure Python ökoszisztémájának hello párbeszédes formában történő eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="5da0e-188">hello Jupyter Notebook provides a powerful interface for accessing interactively hello power of hello Python ecosystem on Azure.</span></span>  <span data-ttu-id="5da0e-189">Használati esetek beleértve egyszerű feltárása és Python, adatelemzés és a képi megjelenítés, szimuláció és párhuzamos számítástechnikai számos magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="5da0e-189">It covers a wide range of usage cases including simple exploration and learning Python, data analysis and visualization, simulation and parallel computing.</span></span> <span data-ttu-id="5da0e-190">hello eredményül kapott Notebook dokumentumok hajtja végre, és más Jupyter felhasználókkal való megosztás hello számítások teljes rekordja tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="5da0e-190">hello resulting Notebook documents contain a complete record of hello computations that are performed and can be shared with other Jupyter users.</span></span>  <span data-ttu-id="5da0e-191">hello Jupyter Notebook egy helyi alkalmazás használható, de ez akkor ajánlott, ha az Azure felhőben történő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="5da0e-191">hello Jupyter Notebook can be used as a local application, but it is ideally suited for cloud deployments on Azure</span></span>

<span data-ttu-id="5da0e-192">hello core Jupyter számára is elérhető belül a Visual Studio használatával a [a Python Tools for Visual Studio] [ Python Tools for Visual Studio] (PTVS).</span><span class="sxs-lookup"><span data-stu-id="5da0e-192">hello core features of Jupyter are also available inside Visual Studio via the [Python Tools for Visual Studio][Python Tools for Visual Studio] (PTVS).</span></span> <span data-ttu-id="5da0e-193">PTVS egy ingyenes és nyílt forráskódú beépülő modul a Microsoft Visual Studio be olyan speciális Python fejlesztői környezetben, amely tartalmaz egy speciális szerkesztőt az IntelliSense hibakeresési információ-profilkészítési és párhuzamos számítástechnikai integráció.</span><span class="sxs-lookup"><span data-stu-id="5da0e-193">PTVS is a free and open-source plug-in from Microsoft that turns Visual Studio into an advanced Python development environment that includes an advanced editor with IntelliSense, debugging, profiling and parallel computing integration.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5da0e-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5da0e-194">Next steps</span></span>
<span data-ttu-id="5da0e-195">További információkért lásd: hello [Python fejlesztői központ](/develop/python/).</span><span class="sxs-lookup"><span data-stu-id="5da0e-195">For more information, see hello [Python Developer Center](/develop/python/).</span></span>

[portal-vm-linux]: https://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-tutorial-portal-rm/
[repository]: https://github.com/ipython/ipython
[Python Tools for Visual Studio]: http://aka.ms/ptvs
