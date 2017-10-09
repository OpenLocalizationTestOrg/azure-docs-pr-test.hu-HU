1. <span data-ttu-id="eeee6-101">Másolja hello telepítő tooa helyi mappát (például /tmp), amelyet az tooprotect hello kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="eeee6-101">Copy hello installer tooa local folder (for example, /tmp) on hello server that you want tooprotect.</span></span> <span data-ttu-id="eeee6-102">Egy terminált futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="eeee6-102">In a terminal, run hello following commands:</span></span>
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. <span data-ttu-id="eeee6-103">tooinstall mobilitási szolgáltatást, futtassa a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="eeee6-103">tooinstall Mobility Service, run hello following command:</span></span>

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. <span data-ttu-id="eeee6-104">Telepítés befejezése után hello Mobilitásiszolgáltatás kell tooget regisztrált toohello konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="eeee6-104">Once installation is complete, hello Mobility Service needs tooget registered toohello configuration server.</span></span> <span data-ttu-id="eeee6-105">Futtassa a következő parancs tooregister hello mobilitási szolgáltatás konfigurációs kiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="eeee6-105">Run hello following command tooregister hello Mobility Service with Configuration server.</span></span>

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a><span data-ttu-id="eeee6-106">Mobilitási szolgáltatások telepítőjének parancssori</span><span class="sxs-lookup"><span data-stu-id="eeee6-106">Mobility Service installer command-line</span></span>

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|<span data-ttu-id="eeee6-107">Paraméter</span><span class="sxs-lookup"><span data-stu-id="eeee6-107">Parameter</span></span>|<span data-ttu-id="eeee6-108">Típus</span><span class="sxs-lookup"><span data-stu-id="eeee6-108">Type</span></span>|<span data-ttu-id="eeee6-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="eeee6-109">Description</span></span>|<span data-ttu-id="eeee6-110">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="eeee6-110">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="eeee6-111">-r</span><span class="sxs-lookup"><span data-stu-id="eeee6-111">-r</span></span> |<span data-ttu-id="eeee6-112">Kötelező</span><span class="sxs-lookup"><span data-stu-id="eeee6-112">Mandatory</span></span>|<span data-ttu-id="eeee6-113">Megadja, hogy kell telepíteni a mobilitási szolgáltatás (MS), vagy MasterTarget(MT) kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="eeee6-113">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="eeee6-114">MS</span><span class="sxs-lookup"><span data-stu-id="eeee6-114">MS</span></span> </br> <span data-ttu-id="eeee6-115">FŐ CÉLKISZOLGÁLÓ</span><span class="sxs-lookup"><span data-stu-id="eeee6-115">MT</span></span>|
|<span data-ttu-id="eeee6-116">-d</span><span class="sxs-lookup"><span data-stu-id="eeee6-116">-d</span></span> |<span data-ttu-id="eeee6-117">Optional</span><span class="sxs-lookup"><span data-stu-id="eeee6-117">Optional</span></span>|<span data-ttu-id="eeee6-118">Hely, ahol a mobilitási szolgáltatás telepítve lesz</span><span class="sxs-lookup"><span data-stu-id="eeee6-118">Location where Mobility Service will be installed</span></span>|<span data-ttu-id="eeee6-119">/usr/local/ASR</span><span class="sxs-lookup"><span data-stu-id="eeee6-119">/usr/local/ASR</span></span>|
|<span data-ttu-id="eeee6-120">-v</span><span class="sxs-lookup"><span data-stu-id="eeee6-120">-v</span></span>|<span data-ttu-id="eeee6-121">Kötelező</span><span class="sxs-lookup"><span data-stu-id="eeee6-121">Mandatory</span></span>|<span data-ttu-id="eeee6-122">Meghatározza, melyik hello a mobilitási szolgáltatás telepítve van első hello platform</span><span class="sxs-lookup"><span data-stu-id="eeee6-122">Specifies hello platform on which hello Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="eeee6-123">- **VMware** : használja ezt az értéket, ha egy virtuális gépen futó mobilitási szolgáltatás telepít *VMware vSphere ESXi-gazdagépek*, *Hyper-V-gazdagépek* és *Phsyical kiszolgálók*</span><span class="sxs-lookup"><span data-stu-id="eeee6-123">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="eeee6-124">- **Azure** : használja ezt az értéket, ha telepíti az ügynököt egy Azure IaaS virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="eeee6-124">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="eeee6-125">VMware</span><span class="sxs-lookup"><span data-stu-id="eeee6-125">VMware</span></span> </br> <span data-ttu-id="eeee6-126">Azure</span><span class="sxs-lookup"><span data-stu-id="eeee6-126">Azure</span></span>|
|<span data-ttu-id="eeee6-127">-k</span><span class="sxs-lookup"><span data-stu-id="eeee6-127">-q</span></span>|<span data-ttu-id="eeee6-128">Optional</span><span class="sxs-lookup"><span data-stu-id="eeee6-128">Optional</span></span>|<span data-ttu-id="eeee6-129">Adja meg a toorun telepítő csendes módban</span><span class="sxs-lookup"><span data-stu-id="eeee6-129">Specifies toorun installer in silent mode</span></span>| <span data-ttu-id="eeee6-130">N/A</span><span class="sxs-lookup"><span data-stu-id="eeee6-130">N/A</span></span>|


#### <a name="mobility-service-configuration-command-line"></a><span data-ttu-id="eeee6-131">Parancssori mobilitási szolgáltatás konfigurációja</span><span class="sxs-lookup"><span data-stu-id="eeee6-131">Mobility Service configuration command-line</span></span>

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|<span data-ttu-id="eeee6-132">Paraméter</span><span class="sxs-lookup"><span data-stu-id="eeee6-132">Parameter</span></span>|<span data-ttu-id="eeee6-133">Típus</span><span class="sxs-lookup"><span data-stu-id="eeee6-133">Type</span></span>|<span data-ttu-id="eeee6-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="eeee6-134">Description</span></span>|<span data-ttu-id="eeee6-135">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="eeee6-135">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="eeee6-136">-i</span><span class="sxs-lookup"><span data-stu-id="eeee6-136">-i</span></span> |<span data-ttu-id="eeee6-137">Kötelező</span><span class="sxs-lookup"><span data-stu-id="eeee6-137">Mandatory</span></span>|<span data-ttu-id="eeee6-138">IP-címe hello konfigurációs kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="eeee6-138">IP of hello Configuration Server</span></span>|<span data-ttu-id="eeee6-139">Bármilyen érvényes IP-cím</span><span class="sxs-lookup"><span data-stu-id="eeee6-139">Any valid IP Address</span></span>|
|<span data-ttu-id="eeee6-140">-P</span><span class="sxs-lookup"><span data-stu-id="eeee6-140">-P</span></span> |<span data-ttu-id="eeee6-141">Kötelező</span><span class="sxs-lookup"><span data-stu-id="eeee6-141">Mandatory</span></span>|<span data-ttu-id="eeee6-142">Hello kapcsolat jelszava mentett tartalmazó fájl teljes elérési útja hello fájl</span><span class="sxs-lookup"><span data-stu-id="eeee6-142">Full file path hello file where hello connection passphrase is saved</span></span>|<span data-ttu-id="eeee6-143">Bármilyen érvényes mappa</span><span class="sxs-lookup"><span data-stu-id="eeee6-143">Any valid folder</span></span>|
