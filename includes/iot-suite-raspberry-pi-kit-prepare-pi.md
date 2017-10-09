## <a name="prepare-your-raspberry-pi"></a><span data-ttu-id="6296c-101">Készítse elő a Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="6296c-101">Prepare your Raspberry Pi</span></span>

### <a name="install-raspbian"></a><span data-ttu-id="6296c-102">Raspbian telepítése</span><span class="sxs-lookup"><span data-stu-id="6296c-102">Install Raspbian</span></span>

<span data-ttu-id="6296c-103">Ha ez hello első alkalommal használja a málna Pi, meg kell tooinstall hello Raspbian operációs rendszer NOOBS SD-kártyán hello hello Kit része.</span><span class="sxs-lookup"><span data-stu-id="6296c-103">If this is hello first time you are using your Raspberry Pi, you need tooinstall hello Raspbian operating system using NOOBS on hello SD card included in hello kit.</span></span> <span data-ttu-id="6296c-104">Hello [málna Pi szoftver útmutató] [ lnk-install-raspbian] ismerteti, hogyan tooinstall a málna Pi operációs rendszerének.</span><span class="sxs-lookup"><span data-stu-id="6296c-104">hello [Raspberry Pi Software Guide][lnk-install-raspbian] describes how tooinstall an operating system on your Raspberry Pi.</span></span> <span data-ttu-id="6296c-105">Ez az oktatóanyag feltételezi, hogy telepítette a málna Pi hello Raspbian operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="6296c-105">This tutorial assumes you have installed hello Raspbian operating system on your Raspberry Pi.</span></span>

> [!NOTE]
> <span data-ttu-id="6296c-106">hello található hello SD-kártyát [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] már rendelkezik telepített NOOBS.</span><span class="sxs-lookup"><span data-stu-id="6296c-106">hello SD card included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] already has NOOBS installed.</span></span> <span data-ttu-id="6296c-107">Rendszerindító hello málna Pi a kártyáról, és válassza ki a tooinstall hello Raspbian operációs rendszer.</span><span class="sxs-lookup"><span data-stu-id="6296c-107">You can boot hello Raspberry Pi from this card and choose tooinstall hello Raspbian OS.</span></span>

### <a name="set-up-hello-hardware"></a><span data-ttu-id="6296c-108">Hello hardver beállítása</span><span class="sxs-lookup"><span data-stu-id="6296c-108">Set up hello hardware</span></span>

<span data-ttu-id="6296c-109">Ez az oktatóanyag használja hello szereplő hello BME280 érzékelő [a Microsoft Azure IoT Starter Kit málna Pi 3] [ lnk-starter-kits] toogenerate telemetriai adatokat.</span><span class="sxs-lookup"><span data-stu-id="6296c-109">This tutorial uses hello BME280 sensor included in hello [Microsoft Azure IoT Starter Kit for Raspberry Pi 3][lnk-starter-kits] toogenerate telemetry data.</span></span> <span data-ttu-id="6296c-110">Hello málna Pi dolgozza fel a hello megoldás irányítópultról metódushívási egy LED tooindicate alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="6296c-110">It uses an LED tooindicate when hello Raspberry Pi processes a method invocation from hello solution dashboard.</span></span>

<span data-ttu-id="6296c-111">hello be táblán hello összetevők a következők:</span><span class="sxs-lookup"><span data-stu-id="6296c-111">hello components on hello bread board are:</span></span>

- <span data-ttu-id="6296c-112">Piros LED</span><span class="sxs-lookup"><span data-stu-id="6296c-112">Red LED</span></span>
- <span data-ttu-id="6296c-113">220 ohmos ellenállás (piros, a piros, a barna.)</span><span class="sxs-lookup"><span data-stu-id="6296c-113">220-Ohm resistor (red, red, brown)</span></span>
- <span data-ttu-id="6296c-114">BME280 érzékelő</span><span class="sxs-lookup"><span data-stu-id="6296c-114">BME280 sensor</span></span>

<span data-ttu-id="6296c-115">hello következő ábra azt mutatja, hogyan tooconnect a hardver:</span><span class="sxs-lookup"><span data-stu-id="6296c-115">hello following diagram shows how tooconnect your hardware:</span></span>

![Málna Pi hardver beállítása][img-connection-diagram]

<span data-ttu-id="6296c-117">hello következő táblázat összefoglalja hello málna Pi toohello-összetevőket hello breadboard hello kapcsolatokat:</span><span class="sxs-lookup"><span data-stu-id="6296c-117">hello following table summarizes hello connections from hello Raspberry Pi toohello components on hello breadboard:</span></span>

| <span data-ttu-id="6296c-118">Raspberry Pi</span><span class="sxs-lookup"><span data-stu-id="6296c-118">Raspberry Pi</span></span>            | <span data-ttu-id="6296c-119">Breadboard</span><span class="sxs-lookup"><span data-stu-id="6296c-119">Breadboard</span></span>             |<span data-ttu-id="6296c-120">Szín</span><span class="sxs-lookup"><span data-stu-id="6296c-120">Color</span></span>         |
| ----------------------- | ---------------------- | ------------- |
| <span data-ttu-id="6296c-121">GND (14 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="6296c-121">GND (Pin 14)</span></span>            | <span data-ttu-id="6296c-122">LED - nálói PIN-kódot (18A)</span><span class="sxs-lookup"><span data-stu-id="6296c-122">LED -ve pin (18A)</span></span>      | <span data-ttu-id="6296c-123">Lila</span><span class="sxs-lookup"><span data-stu-id="6296c-123">Purple</span></span>          |
| <span data-ttu-id="6296c-124">GPCLK0 (PIN-kód 7)</span><span class="sxs-lookup"><span data-stu-id="6296c-124">GPCLK0 (Pin 7)</span></span>          | <span data-ttu-id="6296c-125">Ellenállás [25]</span><span class="sxs-lookup"><span data-stu-id="6296c-125">Resistor (25A)</span></span>         | <span data-ttu-id="6296c-126">Narancssárga</span><span class="sxs-lookup"><span data-stu-id="6296c-126">Orange</span></span>          |
| <span data-ttu-id="6296c-127">SPI_CE0 (PIN-kód 24)</span><span class="sxs-lookup"><span data-stu-id="6296c-127">SPI_CE0 (Pin 24)</span></span>        | <span data-ttu-id="6296c-128">CS (39A)</span><span class="sxs-lookup"><span data-stu-id="6296c-128">CS (39A)</span></span>               | <span data-ttu-id="6296c-129">Kék</span><span class="sxs-lookup"><span data-stu-id="6296c-129">Blue</span></span>          |
| <span data-ttu-id="6296c-130">SPI_SCLK (PIN-kód 23)</span><span class="sxs-lookup"><span data-stu-id="6296c-130">SPI_SCLK (Pin 23)</span></span>       | <span data-ttu-id="6296c-131">SCK (36A)</span><span class="sxs-lookup"><span data-stu-id="6296c-131">SCK (36A)</span></span>              | <span data-ttu-id="6296c-132">Sárga</span><span class="sxs-lookup"><span data-stu-id="6296c-132">Yellow</span></span>        |
| <span data-ttu-id="6296c-133">SPI_MISO (21 PIN-kód)</span><span class="sxs-lookup"><span data-stu-id="6296c-133">SPI_MISO (Pin 21)</span></span>       | <span data-ttu-id="6296c-134">SDO (37A)</span><span class="sxs-lookup"><span data-stu-id="6296c-134">SDO (37A)</span></span>              | <span data-ttu-id="6296c-135">Fehér</span><span class="sxs-lookup"><span data-stu-id="6296c-135">White</span></span>         |
| <span data-ttu-id="6296c-136">SPI_MOSI (PIN-kód 19)</span><span class="sxs-lookup"><span data-stu-id="6296c-136">SPI_MOSI (Pin 19)</span></span>       | <span data-ttu-id="6296c-137">SDI (38A)</span><span class="sxs-lookup"><span data-stu-id="6296c-137">SDI (38A)</span></span>              | <span data-ttu-id="6296c-138">Zöld</span><span class="sxs-lookup"><span data-stu-id="6296c-138">Green</span></span>         |
| <span data-ttu-id="6296c-139">GND (PIN-kód 6)</span><span class="sxs-lookup"><span data-stu-id="6296c-139">GND (Pin 6)</span></span>             | <span data-ttu-id="6296c-140">GND (35A)</span><span class="sxs-lookup"><span data-stu-id="6296c-140">GND (35A)</span></span>              | <span data-ttu-id="6296c-141">Fekete</span><span class="sxs-lookup"><span data-stu-id="6296c-141">Black</span></span>         |
| <span data-ttu-id="6296c-142">3.3 V (1. Pin)</span><span class="sxs-lookup"><span data-stu-id="6296c-142">3.3 V (Pin 1)</span></span>           | <span data-ttu-id="6296c-143">3Vo (34A)</span><span class="sxs-lookup"><span data-stu-id="6296c-143">3Vo (34A)</span></span>              | <span data-ttu-id="6296c-144">Piros</span><span class="sxs-lookup"><span data-stu-id="6296c-144">Red</span></span>           |

<span data-ttu-id="6296c-145">toocomplete hello hardverbeállításokat, kell:</span><span class="sxs-lookup"><span data-stu-id="6296c-145">toocomplete hello hardware setup, you need to:</span></span>

- <span data-ttu-id="6296c-146">Csatlakoztassa a málna Pi toohello tápegység hello Kit része.</span><span class="sxs-lookup"><span data-stu-id="6296c-146">Connect your Raspberry Pi toohello power supply included in hello kit.</span></span>
- <span data-ttu-id="6296c-147">Csatlakoztassa a málna Pi tooyour hálózati kábellel hello Ethernet az Kit része.</span><span class="sxs-lookup"><span data-stu-id="6296c-147">Connect your Raspberry Pi tooyour network using hello Ethernet cable included in your kit.</span></span> <span data-ttu-id="6296c-148">Másik lehetőségként beállíthatja [vezeték nélküli kapcsolat] [ lnk-pi-wireless] a málna pi.</span><span class="sxs-lookup"><span data-stu-id="6296c-148">Alternatively, you can set up [Wireless Connectivity][lnk-pi-wireless] for your Raspberry Pi.</span></span>

<span data-ttu-id="6296c-149">Ezzel befejezte a málna pi hello hardverbeállításokat.</span><span class="sxs-lookup"><span data-stu-id="6296c-149">You have now completed hello hardware setup of your Raspberry Pi.</span></span>

### <a name="sign-in-and-access-hello-terminal"></a><span data-ttu-id="6296c-150">Bejelentkezhetnek és elérhetik a hello Terminálszolgáltatások</span><span class="sxs-lookup"><span data-stu-id="6296c-150">Sign in and access hello terminal</span></span>

<span data-ttu-id="6296c-151">Két beállítások tooaccess terminál környezettel rendelkezik a málna Pi meg:</span><span class="sxs-lookup"><span data-stu-id="6296c-151">You have two options tooaccess a terminal environment on your Raspberry Pi:</span></span>

- <span data-ttu-id="6296c-152">Ha a billentyűzet és csatlakoztatott tooyour málna Pi figyelése, használhatja a hello Raspbian grafikus felhasználói Felülettel tooaccess egy terminálablakot.</span><span class="sxs-lookup"><span data-stu-id="6296c-152">If you have a keyboard and monitor connected tooyour Raspberry Pi, you can use hello Raspbian GUI tooaccess a terminal window.</span></span>

- <span data-ttu-id="6296c-153">Hozzáférés hello parancs vonalát. az SSH használata a asztali gépen málna Pi.</span><span class="sxs-lookup"><span data-stu-id="6296c-153">Access hello command line on your Raspberry Pi using SSH from your desktop machine.</span></span>

#### <a name="use-a-terminal-window-in-hello-gui"></a><span data-ttu-id="6296c-154">A grafikus felhasználói Felülettel hello terminálablakot használni</span><span class="sxs-lookup"><span data-stu-id="6296c-154">Use a terminal Window in hello GUI</span></span>

<span data-ttu-id="6296c-155">hello alapértelmezett Raspbian a hitelesítő adatok felhasználónév **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="6296c-155">hello default credentials for Raspbian are username **pi** and password **raspberry**.</span></span> <span data-ttu-id="6296c-156">Hello tálcán hello grafikus felhasználói Felülettel, elindíthatja a hello **Terminálszolgáltatások** segédprogram használatával egy figyelő hello ikon.</span><span class="sxs-lookup"><span data-stu-id="6296c-156">In hello task bar in hello GUI, you can launch hello **Terminal** utility using hello icon that looks like a monitor.</span></span>

#### <a name="sign-in-with-ssh"></a><span data-ttu-id="6296c-157">SSH bejelentkezés</span><span class="sxs-lookup"><span data-stu-id="6296c-157">Sign in with SSH</span></span>

<span data-ttu-id="6296c-158">Parancssori hozzáférést tooyour málna Pi SSH használható.</span><span class="sxs-lookup"><span data-stu-id="6296c-158">You can use SSH for command-line access tooyour Raspberry Pi.</span></span> <span data-ttu-id="6296c-159">hello cikk [SSH (Secure Shell)] [ lnk-pi-ssh] ismerteti, hogyan tooconfigure a málna Pi az SSH és hogyan tooconnect a [Windows] [ lnk-ssh-windows] vagy [Linux és Mac OS][lnk-ssh-linux].</span><span class="sxs-lookup"><span data-stu-id="6296c-159">hello article [SSH (Secure Shell)][lnk-pi-ssh] describes how tooconfigure SSH on your Raspberry Pi, and how tooconnect from [Windows][lnk-ssh-windows] or [Linux & Mac OS][lnk-ssh-linux].</span></span>

<span data-ttu-id="6296c-160">Jelentkezzen be a felhasználónevet **pi** és a jelszó **málna**.</span><span class="sxs-lookup"><span data-stu-id="6296c-160">Sign in with username **pi** and password **raspberry**.</span></span>

#### <a name="optional-share-a-folder-on-your-raspberry-pi"></a><span data-ttu-id="6296c-161">Választható lehetőség: A málna Pi a mappa megosztása</span><span class="sxs-lookup"><span data-stu-id="6296c-161">Optional: Share a folder on your Raspberry Pi</span></span>

<span data-ttu-id="6296c-162">Ha szükséges érdemes lehet tooshare egy mappát a málna Pi a az asztali környezetet.</span><span class="sxs-lookup"><span data-stu-id="6296c-162">Optionally, you may want tooshare a folder on your Raspberry Pi with your desktop environment.</span></span> <span data-ttu-id="6296c-163">A mappa megosztása lehetővé teszi, hogy Ön toouse az előnyben részesített asztali szövegszerkesztőben (például [Visual Studio Code](https://code.visualstudio.com/) vagy [Sublime Text](http://www.sublimetext.com/)) használata helyett a málna Pi tooedit fájlok `nano` vagy `vi`.</span><span class="sxs-lookup"><span data-stu-id="6296c-163">Sharing a folder enables you toouse your preferred desktop text editor (such as [Visual Studio Code](https://code.visualstudio.com/) or [Sublime Text](http://www.sublimetext.com/)) tooedit files on your Raspberry Pi instead of using `nano` or `vi`.</span></span>

<span data-ttu-id="6296c-164">tooshare egy mappát a Windows hello málna Pi Samba kiszolgáló konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="6296c-164">tooshare a folder with Windows, configure a Samba server on hello Raspberry Pi.</span></span> <span data-ttu-id="6296c-165">Másik megoldásként használhatja a hello beépített [SFTP](https://www.raspberrypi.org/documentation/remote-access/) kiszolgáló az asztalon egy SFTP-ügyféllel.</span><span class="sxs-lookup"><span data-stu-id="6296c-165">Alternatively, use hello built-in [SFTP](https://www.raspberrypi.org/documentation/remote-access/) server with an SFTP client on your desktop.</span></span>

### <a name="enable-spi"></a><span data-ttu-id="6296c-166">SPI engedélyezése</span><span class="sxs-lookup"><span data-stu-id="6296c-166">Enable SPI</span></span>

<span data-ttu-id="6296c-167">Hello mintaalkalmazás futtatása előtt engedélyeznie kell a hello soros perifériák Interface (SPI) buszhoz hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="6296c-167">Before you can run hello sample application, you must enable hello Serial Peripheral Interface (SPI) bus on hello Raspberry Pi.</span></span> <span data-ttu-id="6296c-168">hello málna Pi hello SPI bus keresztül kommunikál a hello BME280 érzékelő eszköz.</span><span class="sxs-lookup"><span data-stu-id="6296c-168">hello Raspberry Pi communicates with hello BME280 sensor device over hello SPI bus.</span></span> <span data-ttu-id="6296c-169">Használja a következő parancs tooedit hello konfigurációs fájl hello:</span><span class="sxs-lookup"><span data-stu-id="6296c-169">Use hello following command tooedit hello configuration file:</span></span>

```sh
sudo nano /boot/config.txt
```

<span data-ttu-id="6296c-170">Hello sor keresése:</span><span class="sxs-lookup"><span data-stu-id="6296c-170">Find hello line:</span></span>

`#dtparam=spi=on`

- <span data-ttu-id="6296c-171">toouncomment hello sor törlése hello `#` hello indításkor.</span><span class="sxs-lookup"><span data-stu-id="6296c-171">toouncomment hello line, delete hello `#` at hello start.</span></span>
- <span data-ttu-id="6296c-172">Menti a módosításokat (**Ctrl-O**, **Enter**) és a kilépési hello editor (**Ctrl-X**).</span><span class="sxs-lookup"><span data-stu-id="6296c-172">Save your changes (**Ctrl-O**, **Enter**) and exit hello editor (**Ctrl-X**).</span></span>
- <span data-ttu-id="6296c-173">tooenable SPI, indítsa újra a hello málna Pi.</span><span class="sxs-lookup"><span data-stu-id="6296c-173">tooenable SPI, reboot hello Raspberry Pi.</span></span> <span data-ttu-id="6296c-174">Hello Terminálszolgáltatások újraindítás megszakad, újra Ha hello málna Pi újraindul a toosign van szüksége:</span><span class="sxs-lookup"><span data-stu-id="6296c-174">Rebooting disconnects hello terminal, you need toosign in again when hello Raspberry Pi restarts:</span></span>

  ```sh
  sudo reboot
  ```


[img-connection-diagram]: media/iot-suite-raspberry-pi-kit-prepare-pi/rpi2_remote_monitoring.png

[lnk-install-raspbian]: https://www.raspberrypi.org/learning/software-guide/quickstart/
[lnk-pi-wireless]: https://www.raspberrypi.org/documentation/configuration/wireless/README.md
[lnk-pi-ssh]: https://www.raspberrypi.org/documentation/remote-access/ssh/README.md
[lnk-ssh-windows]: https://www.raspberrypi.org/documentation/remote-access/ssh/windows.md
[lnk-ssh-linux]: https://www.raspberrypi.org/documentation/remote-access/ssh/unix.md
[lnk-starter-kits]: https://azure.microsoft.com/develop/iot/starter-kits/