1. <span data-ttu-id="431cf-101">A telepítő másolja egy helyi mappába (például /tmp) azon a kiszolgálón, amelyet védeni kíván.</span><span class="sxs-lookup"><span data-stu-id="431cf-101">Copy the installer to a local folder (for example, /tmp) on the server that you want to protect.</span></span> <span data-ttu-id="431cf-102">A terminálban a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="431cf-102">In a terminal, run the following commands:</span></span>
  ```
  cd /tmp
  tar -xvzf Microsoft-ASR_UA*release.tar.gz
  ```
2. <span data-ttu-id="431cf-103">Szeretné telepíteni a mobilitási szolgáltatást, futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="431cf-103">To install Mobility Service, run the following command:</span></span>

  ```
  sudo ./install -d <Install Location> -r MS -v VmWare -q
  ```
3. <span data-ttu-id="431cf-104">Telepítés befejezése után kell a mobilitási szolgáltatás és a konfigurációs kiszolgáló regisztrálva.</span><span class="sxs-lookup"><span data-stu-id="431cf-104">Once installation is complete, the Mobility Service needs to get registered to the configuration server.</span></span> <span data-ttu-id="431cf-105">A következő parancsot a mobilitási szolgáltatás regisztrálása a konfigurációs kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="431cf-105">Run the following command to register the Mobility Service with Configuration server.</span></span>

  ```
  /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <CSIP> -P /var/passphrase.txt
  ```

#### <a name="mobility-service-installer-command-line"></a><span data-ttu-id="431cf-106">Mobilitási szolgáltatások telepítőjének parancssori</span><span class="sxs-lookup"><span data-stu-id="431cf-106">Mobility Service installer command-line</span></span>

```
Usage:
./install -d <Install Location> -r <MS|MT> -v VmWare -q
```

|<span data-ttu-id="431cf-107">Paraméter</span><span class="sxs-lookup"><span data-stu-id="431cf-107">Parameter</span></span>|<span data-ttu-id="431cf-108">Típus</span><span class="sxs-lookup"><span data-stu-id="431cf-108">Type</span></span>|<span data-ttu-id="431cf-109">Leírás</span><span class="sxs-lookup"><span data-stu-id="431cf-109">Description</span></span>|<span data-ttu-id="431cf-110">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="431cf-110">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="431cf-111">-r</span><span class="sxs-lookup"><span data-stu-id="431cf-111">-r</span></span> |<span data-ttu-id="431cf-112">Kötelező</span><span class="sxs-lookup"><span data-stu-id="431cf-112">Mandatory</span></span>|<span data-ttu-id="431cf-113">Megadja, hogy kell telepíteni a mobilitási szolgáltatás (MS), vagy MasterTarget(MT) kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="431cf-113">Specifies whether Mobility Service (MS) should be installed or MasterTarget(MT) should be installed</span></span>|<span data-ttu-id="431cf-114">MS</span><span class="sxs-lookup"><span data-stu-id="431cf-114">MS</span></span> </br> <span data-ttu-id="431cf-115">FŐ CÉLKISZOLGÁLÓ</span><span class="sxs-lookup"><span data-stu-id="431cf-115">MT</span></span>|
|<span data-ttu-id="431cf-116">-d</span><span class="sxs-lookup"><span data-stu-id="431cf-116">-d</span></span> |<span data-ttu-id="431cf-117">Optional</span><span class="sxs-lookup"><span data-stu-id="431cf-117">Optional</span></span>|<span data-ttu-id="431cf-118">Hely, ahol a mobilitási szolgáltatás telepítve lesz</span><span class="sxs-lookup"><span data-stu-id="431cf-118">Location where Mobility Service will be installed</span></span>|<span data-ttu-id="431cf-119">/usr/local/ASR</span><span class="sxs-lookup"><span data-stu-id="431cf-119">/usr/local/ASR</span></span>|
|<span data-ttu-id="431cf-120">-v</span><span class="sxs-lookup"><span data-stu-id="431cf-120">-v</span></span>|<span data-ttu-id="431cf-121">Kötelező</span><span class="sxs-lookup"><span data-stu-id="431cf-121">Mandatory</span></span>|<span data-ttu-id="431cf-122">Adja meg a platform, amelyen a mobilitási szolgáltatás található első</span><span class="sxs-lookup"><span data-stu-id="431cf-122">Specifies the platform on which the Mobility Service is getting installed</span></span> </br> </br><span data-ttu-id="431cf-123">- **VMware** : használja ezt az értéket, ha egy virtuális gépen futó mobilitási szolgáltatás telepít *VMware vSphere ESXi-gazdagépek*, *Hyper-V-gazdagépek* és *Phsyical kiszolgálók*</span><span class="sxs-lookup"><span data-stu-id="431cf-123">- **VMware** : use this value if you are installing mobility service on a VM running on *VMware vSphere ESXi Hosts*, *Hyper-V Hosts* and *Phsyical Servers*</span></span> </br> <span data-ttu-id="431cf-124">- **Azure** : használja ezt az értéket, ha telepíti az ügynököt egy Azure IaaS virtuális Gépen</span><span class="sxs-lookup"><span data-stu-id="431cf-124">- **Azure** : use this value if you are installing agent on a Azure IaaS VM</span></span>| <span data-ttu-id="431cf-125">VMware</span><span class="sxs-lookup"><span data-stu-id="431cf-125">VMware</span></span> </br> <span data-ttu-id="431cf-126">Azure</span><span class="sxs-lookup"><span data-stu-id="431cf-126">Azure</span></span>|
|<span data-ttu-id="431cf-127">-k</span><span class="sxs-lookup"><span data-stu-id="431cf-127">-q</span></span>|<span data-ttu-id="431cf-128">Optional</span><span class="sxs-lookup"><span data-stu-id="431cf-128">Optional</span></span>|<span data-ttu-id="431cf-129">Meghatározza, hogy a telepítő futtatásához csendes módban</span><span class="sxs-lookup"><span data-stu-id="431cf-129">Specifies to run installer in silent mode</span></span>| <span data-ttu-id="431cf-130">N/A</span><span class="sxs-lookup"><span data-stu-id="431cf-130">N/A</span></span>|


#### <a name="mobility-service-configuration-command-line"></a><span data-ttu-id="431cf-131">Parancssori mobilitási szolgáltatás konfigurációja</span><span class="sxs-lookup"><span data-stu-id="431cf-131">Mobility Service configuration command-line</span></span>

```
Usage:
cd /usr/local/ASR/Vx/bin
UnifiedAgentConfigurator.sh -i <CSIP> -P <PassphraseFilePath>
```

|<span data-ttu-id="431cf-132">Paraméter</span><span class="sxs-lookup"><span data-stu-id="431cf-132">Parameter</span></span>|<span data-ttu-id="431cf-133">Típus</span><span class="sxs-lookup"><span data-stu-id="431cf-133">Type</span></span>|<span data-ttu-id="431cf-134">Leírás</span><span class="sxs-lookup"><span data-stu-id="431cf-134">Description</span></span>|<span data-ttu-id="431cf-135">Lehetséges értékek</span><span class="sxs-lookup"><span data-stu-id="431cf-135">Possible values</span></span>|
|-|-|-|-|
|<span data-ttu-id="431cf-136">-i</span><span class="sxs-lookup"><span data-stu-id="431cf-136">-i</span></span> |<span data-ttu-id="431cf-137">Kötelező</span><span class="sxs-lookup"><span data-stu-id="431cf-137">Mandatory</span></span>|<span data-ttu-id="431cf-138">IP-címe a konfigurációs kiszolgálón</span><span class="sxs-lookup"><span data-stu-id="431cf-138">IP of the Configuration Server</span></span>|<span data-ttu-id="431cf-139">Bármilyen érvényes IP-cím</span><span class="sxs-lookup"><span data-stu-id="431cf-139">Any valid IP Address</span></span>|
|<span data-ttu-id="431cf-140">-P</span><span class="sxs-lookup"><span data-stu-id="431cf-140">-P</span></span> |<span data-ttu-id="431cf-141">Kötelező</span><span class="sxs-lookup"><span data-stu-id="431cf-141">Mandatory</span></span>|<span data-ttu-id="431cf-142">A kapcsolat jelszava mentett tartalmazó fájl teljes elérési út</span><span class="sxs-lookup"><span data-stu-id="431cf-142">Full file path the file where the connection passphrase is saved</span></span>|<span data-ttu-id="431cf-143">Bármilyen érvényes mappa</span><span class="sxs-lookup"><span data-stu-id="431cf-143">Any valid folder</span></span>|
