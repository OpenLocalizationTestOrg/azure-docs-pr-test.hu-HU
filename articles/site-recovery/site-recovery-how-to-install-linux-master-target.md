---
title: "a Linux-fő célkiszolgáló feladatátvétel az Azure tooon helyszíni aaaHow tooinstall |} Microsoft Docs"
description: "A Linux virtuális gép újbóli védelméhez, előtt kell egy Linux fő célkiszolgáló. Megtudhatja, hogyan tooinstall egyet."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: gauravd
editor: 
ms.assetid: 44813a48-c680-4581-a92e-cecc57cc3b1e
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 08/11/2017
ms.author: ruturajd
ms.openlocfilehash: d7c55d115712b9862414979f89efb1f177c5f0dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-a-linux-master-target-server"></a><span data-ttu-id="75331-104">A Linux fő célkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="75331-104">Install a Linux master target server</span></span>
<span data-ttu-id="75331-105">Miután a virtuális gépeket nem sikerül, hátsó hello virtuális gépek toohello helyszíni hely sikertelen lehet.</span><span class="sxs-lookup"><span data-stu-id="75331-105">After you fail over your virtual machines, you can fail back hello virtual machines toohello on-premises site.</span></span> <span data-ttu-id="75331-106">a toofail vissza kell tooreprotect hello virtuális gép Azure toohello a helyszíni helyről.</span><span class="sxs-lookup"><span data-stu-id="75331-106">toofail back, you need tooreprotect hello virtual machine from Azure toohello on-premises site.</span></span> <span data-ttu-id="75331-107">Ez a folyamat egy a helyszíni fő célkiszolgáló tooreceive hello forgalom kell.</span><span class="sxs-lookup"><span data-stu-id="75331-107">For this process, you need an on-premises master target server tooreceive hello traffic.</span></span> 

<span data-ttu-id="75331-108">Ha a védett virtuális gépet egy Windows rendszerű virtuális gép, majd meg kell olyan Windows fő célkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="75331-108">If your protected virtual machine is a Windows virtual machine, then you need a Windows master target.</span></span> <span data-ttu-id="75331-109">A Linux virtuális gép van szüksége a Linuxos fő célkiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="75331-109">For a Linux virtual machine, you need a Linux master target.</span></span> <span data-ttu-id="75331-110">Olvasási hello alábbi lépéseit toolearn hogyan toocreate és a telepítése egy Linux fő cél.</span><span class="sxs-lookup"><span data-stu-id="75331-110">Read hello following steps toolearn how toocreate and install a Linux master target.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75331-111">A fő célkiszolgáló hello 9.10.0 kiadástól kezdve, hello legújabb fő célkiszolgáló csak telepíthető egy Ubuntu 16.04 kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="75331-111">Starting with release of hello 9.10.0 master target server, hello latest master target server can be only installed on an Ubuntu 16.04 server.</span></span> <span data-ttu-id="75331-112">Új telepítések CentOS6.6 kiszolgálók nincsenek engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="75331-112">New installations aren't allowed on  CentOS6.6 servers.</span></span> <span data-ttu-id="75331-113">Azonban tovább tooupgrade a régi fő célkiszolgálók hello 9.10.0 verziójával.</span><span class="sxs-lookup"><span data-stu-id="75331-113">However, you can continue tooupgrade your old master target servers by using hello 9.10.0 version.</span></span>

## <a name="overview"></a><span data-ttu-id="75331-114">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="75331-114">Overview</span></span>
<span data-ttu-id="75331-115">Ez a cikk ismerteti hogyan tooinstall egy Linux fő cél.</span><span class="sxs-lookup"><span data-stu-id="75331-115">This article provides instructions for how tooinstall a Linux master target.</span></span>

<span data-ttu-id="75331-116">Megjegyzéseit vagy kérdéseit a cikk vagy hello hello végén utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="75331-116">Post comments or questions at hello end of this article or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="75331-117">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="75331-117">Prerequisites</span></span>

* <span data-ttu-id="75331-118">toochoose hello gazdagép mely toodeploy-hello fő célkiszolgáló, határozza meg, ha a feladat-visszavétel hello toobe tooan meglévő helyszíni virtuális gép vagy tooa új virtuális gép lesz.</span><span class="sxs-lookup"><span data-stu-id="75331-118">toochoose hello host on which toodeploy hello master target, determine if hello failback is going toobe tooan existing on-premises virtual machine or tooa new virtual machine.</span></span> 
    * <span data-ttu-id="75331-119">Egy meglévő virtuális gép hello hello fő célkiszolgáló kell rendelkeznie hozzáférés toohello adattárolókhoz hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="75331-119">For an existing virtual machine, hello host of hello master target should have access toohello data stores of hello virtual machine.</span></span>
    * <span data-ttu-id="75331-120">Ha hello a helyszíni virtuális gép nem létezik, azonos gazdagép hello fő célként hello hello feladat-visszavételt a virtuális gép létrejön.</span><span class="sxs-lookup"><span data-stu-id="75331-120">If hello on-premises virtual machine does not exist, hello failback virtual machine is created on hello same host as hello master target.</span></span> <span data-ttu-id="75331-121">Választhat ESXi-állomáson tooinstall hello fő célkiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="75331-121">You can choose any ESXi host tooinstall hello master target.</span></span>
* <span data-ttu-id="75331-122">hello fő célkiszolgálón, amely hello folyamatkiszolgáló és hello konfigurációs kiszolgáló képes kommunikálni a hálózaton kell lennie.</span><span class="sxs-lookup"><span data-stu-id="75331-122">hello master target should be on a network that can communicate with hello process server and hello configuration server.</span></span>
* <span data-ttu-id="75331-123">hello hello fő célkiszolgáló kell lennie egyenlő tooor rendszernél korábbi hello folyamatkiszolgáló és a konfigurációs kiszolgáló hello hello verzióit.</span><span class="sxs-lookup"><span data-stu-id="75331-123">hello version of hello master target must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="75331-124">Például ha hello hello konfigurációs kiszolgáló verziószáma 9.4, hello fő célkiszolgáló hello verziója lehet 9.4 vagy 9.3, de nem 9,5.</span><span class="sxs-lookup"><span data-stu-id="75331-124">For example, if hello version of hello configuration server is 9.4, hello version of hello master target can be 9.4 or 9.3 but not 9.5.</span></span>
* <span data-ttu-id="75331-125">hello fő célkiszolgáló csak akkor lehet VMware virtuális gépek nem egy fizikai kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="75331-125">hello master target can only be a VMware virtual machine and not a physical server.</span></span>

## <a name="create-hello-master-target-according-toohello-sizing-guidelines"></a><span data-ttu-id="75331-126">Hello fő célkiszolgáló toohello méretezési irányelvek szerint létrehozása</span><span class="sxs-lookup"><span data-stu-id="75331-126">Create hello master target according toohello sizing guidelines</span></span>

<span data-ttu-id="75331-127">Hozza létre a fő célkiszolgáló hello méretezési útmutató következő hello megfelelően:</span><span class="sxs-lookup"><span data-stu-id="75331-127">Create hello master target in accordance with hello following sizing guidelines:</span></span>
- <span data-ttu-id="75331-128">**RAM**: legalább 6 GB</span><span class="sxs-lookup"><span data-stu-id="75331-128">**RAM**: 6 GB or more</span></span>
- <span data-ttu-id="75331-129">**Az operációs rendszer lemezméret**: 100 GB vagy több (tooinstall CentOS6.6)</span><span class="sxs-lookup"><span data-stu-id="75331-129">**OS disk size**: 100 GB or more (tooinstall CentOS6.6)</span></span>
- <span data-ttu-id="75331-130">**További lemezmérete adatmegőrzési meghajtó**: 1 TB-os</span><span class="sxs-lookup"><span data-stu-id="75331-130">**Additional disk size for retention drive**: 1 TB</span></span>
- <span data-ttu-id="75331-131">**A Processzormagok**: 4 mag, vagy több</span><span class="sxs-lookup"><span data-stu-id="75331-131">**CPU cores**: 4 cores or more</span></span>

<span data-ttu-id="75331-132">hello következő támogatott Ubuntu kernelek támogatottak.</span><span class="sxs-lookup"><span data-stu-id="75331-132">hello following supported Ubuntu kernels are supported.</span></span>


|<span data-ttu-id="75331-133">Kernel-sorozat</span><span class="sxs-lookup"><span data-stu-id="75331-133">Kernel Series</span></span>  |<span data-ttu-id="75331-134">Túl támogatja</span><span class="sxs-lookup"><span data-stu-id="75331-134">Support up too</span></span> |
|---------|---------|
|<span data-ttu-id="75331-135">4.4</span><span class="sxs-lookup"><span data-stu-id="75331-135">4.4</span></span>      |<span data-ttu-id="75331-136">4.4.0-81-Generic</span><span class="sxs-lookup"><span data-stu-id="75331-136">4.4.0-81-generic</span></span>         |
|<span data-ttu-id="75331-137">4.8</span><span class="sxs-lookup"><span data-stu-id="75331-137">4.8</span></span>      |<span data-ttu-id="75331-138">4.8.0-56-Generic</span><span class="sxs-lookup"><span data-stu-id="75331-138">4.8.0-56-generic</span></span>         |
|<span data-ttu-id="75331-139">4.10</span><span class="sxs-lookup"><span data-stu-id="75331-139">4.10</span></span>     |<span data-ttu-id="75331-140">4.10.0-24-Generic</span><span class="sxs-lookup"><span data-stu-id="75331-140">4.10.0-24-generic</span></span>        |


## <a name="deploy-hello-master-target-server"></a><span data-ttu-id="75331-141">Hello fő célkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="75331-141">Deploy hello master target server</span></span>

### <a name="install-ubuntu-16042-minimal"></a><span data-ttu-id="75331-142">Ubuntu 16.04.2 telepítése minimális</span><span class="sxs-lookup"><span data-stu-id="75331-142">Install Ubuntu 16.04.2 Minimal</span></span>

<span data-ttu-id="75331-143">A következő hello lépéseket tooinstall hello Ubuntu 16.04.2 64 bites operációs rendszer hello igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="75331-143">Take hello following hello steps tooinstall hello Ubuntu 16.04.2 64-bit operating system.</span></span>

<span data-ttu-id="75331-144">**1. lépés:** toohello Ugrás [letöltése hivatkozásra](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) hello legközelebbi tükör válassza az Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet töltse le, amely.</span><span class="sxs-lookup"><span data-stu-id="75331-144">**Step 1:** Go toohello [download link](https://www.ubuntu.com/download/server/thank-you?version=16.04.2&architecture=amd64) and choose hello closest mirror from which download an Ubuntu 16.04.2 minimal 64-bit ISO.</span></span>

<span data-ttu-id="75331-145">Ubuntu 16.04.2 minimális 64 bites ISO-Lemezképet ne hello DVD-meghajtó, és indítsa el a hello rendszer.</span><span class="sxs-lookup"><span data-stu-id="75331-145">Keep an Ubuntu 16.04.2 minimal 64-bit ISO in hello DVD drive and start hello system.</span></span>

<span data-ttu-id="75331-146">**2. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-146">**Step 2:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Válasszon nyelvet](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image1.png)

<span data-ttu-id="75331-148">**3. lépés:** válasszon **Ubuntu Server telepítése**, majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-148">**Step 3:** Select **Install Ubuntu Server**, and then select **Enter**.</span></span>

![Válassza ki a telepítés Ubuntu Server](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image2.png)

<span data-ttu-id="75331-150">**4. lépés:** válasszon **angol** a kívánt nyelvet, és válassza ki, **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-150">**Step 4:** Select **English** as your preferred language, and then select **Enter**.</span></span>

![Jelölje ki a kívánt nyelvet angol nyelven](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image3.png)

<span data-ttu-id="75331-152">**5. lépés:** válassza ki a megfelelő lehetőséget a hello hello **időzóna** beállítások listáját, és válassza **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-152">**Step 5:** Select hello appropriate option from hello **Time Zone** options list, and then select **Enter**.</span></span>

![Válassza ki a megfelelő időzóna hello](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image4.png)

<span data-ttu-id="75331-154">**6. lépés:** válasszon **nem** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-154">**Step 6:** Select **No** (hello default option), and then select **Enter**.</span></span>


![Hello billentyűzet konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image5.png)

<span data-ttu-id="75331-156">**7. lépés:** válasszon **angol (amerikai)** , hello hello billentyűzet származási országot, és adja **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-156">**Step 7:** Select **English (US)** as hello country of origin for hello keyboard, and then select **Enter**.</span></span>

![USA hello származási kiválasztása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image6.png)

<span data-ttu-id="75331-158">**8. lépés:** válasszon **angol (amerikai)** hello billentyűzetkiosztás, és válassza ki, **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-158">**Step 8:** Select **English (US)** as hello keyboard layout, and then select **Enter**.</span></span>

![Jelölje ki angol hello billentyűzetkiosztás módosítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image7.png)

<span data-ttu-id="75331-160">**9. lépés:** hello állomásnév megadása a kiszolgáló hello **állomásnév** mezőbe, majd válassza ki **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="75331-160">**Step 9:** Enter hello hostname for your server in hello **Hostname** box, and then select **Continue**.</span></span>

![Adja meg a kiszolgáló hello állomásnév](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image8.png)

<span data-ttu-id="75331-162">**10. lépés:** toocreate egy felhasználói fiókot adjon meg hello felhasználónevet, és válassza **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="75331-162">**Step 10:** toocreate a user account, enter hello user name, and then select **Continue**.</span></span>

![Felhasználói fiók létrehozása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image9.png)

<span data-ttu-id="75331-164">**11. lépés:** hello jelszó hello új felhasználói fiókhoz, majd válassza ki **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="75331-164">**Step 11:** Enter hello password for hello new user account, and then select **Continue**.</span></span>

![Adja meg a hello jelszó](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image10.png)

<span data-ttu-id="75331-166">**12. lépés:** hello új felhasználó hello jelszót, majd válassza ki **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="75331-166">**Step 12:** Confirm hello password for hello new user, and then select **Continue**.</span></span>

![Hello jelszó megerősítése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image11.png)

<span data-ttu-id="75331-168">**13. lépés:** válasszon **nem** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-168">**Step 13:** Select **No** (hello default option), and then select **Enter**.</span></span>

![Felhasználók és a jelszavak beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image12.png)

<span data-ttu-id="75331-170">**14. lépés:** Ha hello időzóna megjelenített helyes, válassza ki a **Igen** (hello az alapértelmezett beállítás), majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-170">**Step 14:** If hello time zone that's displayed is correct, select **Yes** (hello default option), and then select **Enter**.</span></span>

<span data-ttu-id="75331-171">tooreconfigure az időzónát, jelölje be **nem**.</span><span class="sxs-lookup"><span data-stu-id="75331-171">tooreconfigure your time zone, select **No**.</span></span>

![Hello óra konfigurálása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image13.png)

<span data-ttu-id="75331-173">**15. lépés:** hello metódus beállítások particionálás, válassza ki **interaktív - használja a teljes lemez**, majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-173">**Step 15:** From hello partitioning method options, select **Guided - use entire disk**, and then select **Enter**.</span></span>

![Hello particionálás módszer kiválasztása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image14.png)

<span data-ttu-id="75331-175">**16. lépés:** válassza ki a megfelelő lemez a hello hello **válassza ki a lemez toopartition** beállítások, és válassza **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-175">**Step 16:** Select hello appropriate disk from hello **Select disk toopartition** options, and then select **Enter**.</span></span>


![Válassza ki a hello lemez](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image15.png)

<span data-ttu-id="75331-177">**17. lépés:** válasszon **Igen** toowrite hello módosítások toodisk, és válassza **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-177">**Step 17:** Select **Yes** toowrite hello changes toodisk, and then select **Enter**.</span></span>

![Hello módosítások toodisk írása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image16.png)

<span data-ttu-id="75331-179">**18. lépés:** hello alapértelmezett beállítást, jelölje be **Folytatás**, majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-179">**Step 18:** Select hello default option, select **Continue**, and then select **Enter**.</span></span>

![Válassza ki a hello alapértelmezett beállítás](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image17.png)

<span data-ttu-id="75331-181">**19. lépést:** hello megfelelő beállítást a rendszer a frissítések kezelése, majd válassza ki és **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-181">**Step 19:** Select hello appropriate option for managing upgrades on your system, and then select **Enter**.</span></span>

![Válassza ki, hogyan toomanage frissíti](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image18.png)

> [!WARNING]
> <span data-ttu-id="75331-183">Mivel hello Azure Site Recovery fő célkiszolgáló hello Ubuntu olyan speciális verziója szükséges, meg kell tooensure adott hello kernel frissítések le vannak tiltva hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="75331-183">Because hello Azure Site Recovery master target server requires a very specific version of hello Ubuntu, you need tooensure that hello kernel upgrades are disabled for hello virtual machine.</span></span> <span data-ttu-id="75331-184">Ha engedélyezve vannak, a rendszeres megjelenjenek hello fő server toomalfunction miatt.</span><span class="sxs-lookup"><span data-stu-id="75331-184">If they are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span> <span data-ttu-id="75331-185">Győződjön meg arról, hogy hello **automatikus frissítések** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="75331-185">Make sure you select hello **No automatic updates** option.</span></span>


<span data-ttu-id="75331-186">**20. lépés:** válassza ki az alapértelmezett beállításokat.</span><span class="sxs-lookup"><span data-stu-id="75331-186">**Step 20:** Select default options.</span></span> <span data-ttu-id="75331-187">Ha openSSH az SSH csatlakozni, jelölje be a hello **OpenSSH server** lehetőséget, majd válassza ki **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="75331-187">If you want openSSH for SSH connect, select hello **OpenSSH server** option, and then select **Continue**.</span></span>

![Válassza ki a szoftver](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image19.png)

<span data-ttu-id="75331-189">**21. lépés:** válasszon **Igen**, majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-189">**Step 21:** Select **Yes**, and then select **Enter**.</span></span>

![Telepítés céljaként hello LÁRVAJÁRAT rendszertöltő](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image20.png)

<span data-ttu-id="75331-191">**22. lépés:** válassza ki a megfelelő eszköz hello rendszerindító betöltő telepítéshez hello (lehetőleg **/dev/sda**), majd válassza ki **Enter**.</span><span class="sxs-lookup"><span data-stu-id="75331-191">**Step 22:** Select hello appropriate device for hello boot loader installation (preferably **/dev/sda**), and then select **Enter**.</span></span>

![Jelöljön ki egy eszközt a rendszerindító betöltő telepítéshez](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image21.png)

<span data-ttu-id="75331-193">**23. lépés:** kiválasztása **Folytatás**, majd válassza ki **Enter** toofinish hello telepítési.</span><span class="sxs-lookup"><span data-stu-id="75331-193">**Step 23:** Select **Continue**, and then select **Enter** toofinish hello installation.</span></span>

![Hello telepítés befejezése](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image22.png)

<span data-ttu-id="75331-195">Miután hello telepítés befejeződött, jelentkezzen be toohello VM hello új felhasználói hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="75331-195">After hello installation has finished, sign in toohello VM with hello new user credentials.</span></span> <span data-ttu-id="75331-196">(Lásd túl**lépés 10** további információt.)</span><span class="sxs-lookup"><span data-stu-id="75331-196">(Refer too**Step 10** for more information.)</span></span>

<span data-ttu-id="75331-197">Lépéseket hello a következő képernyőkép tooset hello legfelső szintű hello ismertetett felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="75331-197">Take hello steps that are described in hello following screenshot tooset hello ROOT user password.</span></span> <span data-ttu-id="75331-198">Legfelső szintű felhasználóként, majd jelentkezzen be.</span><span class="sxs-lookup"><span data-stu-id="75331-198">Then sign in as ROOT user.</span></span>

![Hello legfelső szintű felhasználói jelszó beállítása](./media/site-recovery-how-to-install-linux-master-target/ubuntu/image23.png)


### <a name="prepare-hello-machine-for-configuration-as-a-master-target-server"></a><span data-ttu-id="75331-200">Konfiguráció hello gép előkészítése a fő célkiszolgáló-kiszolgálóként</span><span class="sxs-lookup"><span data-stu-id="75331-200">Prepare hello machine for configuration as a master target server</span></span>
<span data-ttu-id="75331-201">A következő előkészíteni a hello gép konfigurációjához, mivel a fő célkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="75331-201">Next, prepare hello machine for configuration as a master target server.</span></span>

<span data-ttu-id="75331-202">minden SCSI merevlemez egy Linux virtuális gép tooget hello azonosító engedélyezése hello **lemez. EnableUUID = TRUE** paraméter.</span><span class="sxs-lookup"><span data-stu-id="75331-202">tooget hello ID for each SCSI hard disk in a Linux virtual machine, enable hello **disk.EnableUUID = TRUE** parameter.</span></span>

<span data-ttu-id="75331-203">Ez a paraméter, hajtsa végre a megfelelő hello a következő lépések tooenable:</span><span class="sxs-lookup"><span data-stu-id="75331-203">tooenable this parameter, take hello following steps:</span></span>

1. <span data-ttu-id="75331-204">Állítsa le a virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="75331-204">Shut down your virtual machine.</span></span>

2. <span data-ttu-id="75331-205">Kattintson a jobb gombbal a hello bejegyzés hello virtuális gép hello bal oldali ablaktáblán, majd válassza ki **beállításainak szerkesztése**.</span><span class="sxs-lookup"><span data-stu-id="75331-205">Right-click hello entry for hello virtual machine in hello left pane, and then select **Edit Settings**.</span></span>

3. <span data-ttu-id="75331-206">Jelölje be hello **beállítások** fülre.</span><span class="sxs-lookup"><span data-stu-id="75331-206">Select hello **Options** tab.</span></span>

4. <span data-ttu-id="75331-207">Hello bal oldali ablaktáblában jelöljön ki **speciális** > **általános**, majd válassza ki a hello **konfigurációs paraméterek** hello képernyő jobb alsó részén hello gombjára.</span><span class="sxs-lookup"><span data-stu-id="75331-207">In hello left pane, select **Advanced** > **General**, and then select hello **Configuration Parameters** button on hello lower-right part of hello screen.</span></span>

    ![Beállítások lap](./media/site-recovery-how-to-install-linux-master-target/media/image20.png)

    <span data-ttu-id="75331-209">Hello **konfigurációs paraméterek** , nincs lehetőség hello gép futása közben.</span><span class="sxs-lookup"><span data-stu-id="75331-209">hello **Configuration Parameters** option is not available when hello machine is running.</span></span> <span data-ttu-id="75331-210">toomake ezen a lapon aktív, hello virtuális gép leállítása.</span><span class="sxs-lookup"><span data-stu-id="75331-210">toomake this tab active, shut down hello virtual machine.</span></span>

5. <span data-ttu-id="75331-211">Tekintse meg, hogy a sor **lemez. EnableUUID** már létezik.</span><span class="sxs-lookup"><span data-stu-id="75331-211">See whether a row with **disk.EnableUUID** already exists.</span></span>

    - <span data-ttu-id="75331-212">Ha hello érték létezik-e, és értéke túl**hamis**, módosítsa hello túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="75331-212">If hello value exists and is set too**False**, change hello value too**True**.</span></span> <span data-ttu-id="75331-213">(hello értékei nem kis-és nagybetűket.)</span><span class="sxs-lookup"><span data-stu-id="75331-213">(hello values are not case-sensitive.)</span></span>

    - <span data-ttu-id="75331-214">Ha hello érték létezik-e, és értéke túl**igaz**, jelölje be **Mégse**.</span><span class="sxs-lookup"><span data-stu-id="75331-214">If hello value exists and is set too**True**, select **Cancel**.</span></span>

    - <span data-ttu-id="75331-215">Ha hello érték nem létezik, válassza ki a **Hozzáadás sor**.</span><span class="sxs-lookup"><span data-stu-id="75331-215">If hello value does not exist, select **Add Row**.</span></span>

    - <span data-ttu-id="75331-216">Adja hozzá a hello neve oszlopban **lemez. EnableUUID**, és utána állítsa be hello érték túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="75331-216">In hello name column, add **disk.EnableUUID**, and then set hello value too**TRUE**.</span></span>

    ![A rendszer ellenőrzi a e lemezen. EnableUUID már létezik.](./media/site-recovery-how-to-install-linux-master-target/media/image21.png)

#### <a name="disable-kernel-upgrades"></a><span data-ttu-id="75331-218">Tiltsa le a kernel frissítések</span><span class="sxs-lookup"><span data-stu-id="75331-218">Disable kernel upgrades</span></span>

<span data-ttu-id="75331-219">Az Azure Site Recovery fő célkiszolgáló hello Ubuntu olyan speciális verziója szükséges, győződjön meg arról, hogy hello kernel frissítések hello virtuális géphez van-e tiltva.</span><span class="sxs-lookup"><span data-stu-id="75331-219">Azure Site Recovery master target server requires a very specific version of hello Ubuntu, ensure that hello kernel upgrades are disabled for hello virtual machine.</span></span>

<span data-ttu-id="75331-220">Ha engedélyezve van a rendszermag-frissítések, a rendszeres megjelenjenek hello fő server toomalfunction miatt.</span><span class="sxs-lookup"><span data-stu-id="75331-220">If kernel upgrades are enabled, then any regular upgrades cause hello master target server toomalfunction.</span></span>

#### <a name="download-and-install-additional-packages"></a><span data-ttu-id="75331-221">Töltse le és telepítse a további csomagokat</span><span class="sxs-lookup"><span data-stu-id="75331-221">Download and install additional packages</span></span>

> [!NOTE]
> <span data-ttu-id="75331-222">Győződjön meg arról, hogy az Internet kapcsolat toodownload, és további csomagok telepítése.</span><span class="sxs-lookup"><span data-stu-id="75331-222">Make sure that you have Internet connectivity toodownload and install additional packages.</span></span> <span data-ttu-id="75331-223">Ha nem rendelkezik internetkapcsolattal, szükség van-e toomanually keresse meg a RPM csomagok és a telepítést.</span><span class="sxs-lookup"><span data-stu-id="75331-223">If you don't have Internet connectivity, you need toomanually find these RPM packages and install them.</span></span>

```
apt-get install -y multipath-tools lsscsi python-pyasn1 lvm2 kpartx
```

### <a name="get-hello-installer-for-setup"></a><span data-ttu-id="75331-224">A telepítő hello telepítő beolvasása</span><span class="sxs-lookup"><span data-stu-id="75331-224">Get hello installer for setup</span></span>

<span data-ttu-id="75331-225">Ha a fő célkiszolgáló rendelkezik internetkapcsolattal, a következő lépéseket toodownload hello telepítő hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="75331-225">If your master target has Internet connectivity, you can use hello following steps toodownload hello installer.</span></span> <span data-ttu-id="75331-226">Ellenkező esetben másolja hello telepítő hello folyamat kiszolgálóról, majd telepítés.</span><span class="sxs-lookup"><span data-stu-id="75331-226">Otherwise, you can copy hello installer from hello process server and then install it.</span></span>

#### <a name="download-hello-master-target-installation-packages"></a><span data-ttu-id="75331-227">Töltse le a hello fő telepítőcsomagok</span><span class="sxs-lookup"><span data-stu-id="75331-227">Download hello master target installation packages</span></span>

<span data-ttu-id="75331-228">[Töltse le a hello legújabb Linux fő célkiszolgáló telepítése bits](https://aka.ms/latestlinuxmobsvc).</span><span class="sxs-lookup"><span data-stu-id="75331-228">[Download hello latest Linux master target installation bits](https://aka.ms/latestlinuxmobsvc).</span></span>

<span data-ttu-id="75331-229">toodownload, Linux, típus használatával:</span><span class="sxs-lookup"><span data-stu-id="75331-229">toodownload it by using Linux, type:</span></span>

```
wget https://aka.ms/latestlinuxmobsvc -O latestlinuxmobsvc.tar.gz
```

<span data-ttu-id="75331-230">Győződjön meg arról, hogy töltse le és csomagolja ki a hello installer a kezdőkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="75331-230">Make sure that you download and unzip hello installer in your home directory.</span></span> <span data-ttu-id="75331-231">Ha túl csomagolja ki**/usr/helyi**, majd hello telepítése nem sikerül.</span><span class="sxs-lookup"><span data-stu-id="75331-231">If you unzip too**/usr/Local**, then hello installation  fails.</span></span>


#### <a name="access-hello-installer-from-hello-process-server"></a><span data-ttu-id="75331-232">Hozzáférés hello telepítő hello folyamat kiszolgálóról</span><span class="sxs-lookup"><span data-stu-id="75331-232">Access hello installer from hello process server</span></span>

1. <span data-ttu-id="75331-233">Hello folyamatkiszolgáló, nyissa meg túl**C:\Program Files (x86) \Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span><span class="sxs-lookup"><span data-stu-id="75331-233">On hello process server, go too**C:\Program Files (x86)\Microsoft Azure Site Recovery\home\svsystems\pushinstallsvc\repository**.</span></span>

2. <span data-ttu-id="75331-234">Hello szükséges installer-fájl másolása hello folyamatkiszolgáló, és mentse a fájt **latestlinuxmobsvc.tar.gz** a kezdőkönyvtárban.</span><span class="sxs-lookup"><span data-stu-id="75331-234">Copy hello required installer file from hello process server, and save it as **latestlinuxmobsvc.tar.gz** in your home directory.</span></span>


### <a name="apply-custom-configuration-changes"></a><span data-ttu-id="75331-235">Egyéni konfigurációs módosítások alkalmazása</span><span class="sxs-lookup"><span data-stu-id="75331-235">Apply custom configuration changes</span></span>

<span data-ttu-id="75331-236">tooapply egyéni konfigurációs módosításokat, a lépéseket követve hello használata:</span><span class="sxs-lookup"><span data-stu-id="75331-236">tooapply custom configuration changes, use hello following steps:</span></span>


1. <span data-ttu-id="75331-237">Futtassa a következő parancs toountar hello bináris hello.</span><span class="sxs-lookup"><span data-stu-id="75331-237">Run hello following command toountar hello binary.</span></span>
    ```
    tar -zxvf latestlinuxmobsvc.tar.gz
    ```
    ![Képernyőfelvétel a hello parancs toorun](./media/site-recovery-how-to-install-linux-master-target/image16.png)

2. <span data-ttu-id="75331-239">Futtassa a következő parancs toogive engedély hello.</span><span class="sxs-lookup"><span data-stu-id="75331-239">Run hello following command toogive permission.</span></span>
    ```
    chmod 755 ./ApplyCustomChanges.sh
    ```

3. <span data-ttu-id="75331-240">Futtassa a következő parancsfájl toorun hello hello.</span><span class="sxs-lookup"><span data-stu-id="75331-240">Run hello following command toorun hello script.</span></span>
    ```
    ./ApplyCustomChanges.sh
    ```
> [!NOTE]
> <span data-ttu-id="75331-241">Csak egyszer hello parancsfájl hello kiszolgálón fut.</span><span class="sxs-lookup"><span data-stu-id="75331-241">Run hello script only once on hello server.</span></span> <span data-ttu-id="75331-242">Hello kiszolgáló leállítása.</span><span class="sxs-lookup"><span data-stu-id="75331-242">Shut down hello server.</span></span> <span data-ttu-id="75331-243">Indítsa újra hello server lemez hozzáadása után hello a következő szakaszban leírtak szerint.</span><span class="sxs-lookup"><span data-stu-id="75331-243">Then restart hello server after you add a disk, as described in hello next section.</span></span>

### <a name="add-a-retention-disk-toohello-linux-master-target-virtual-machine"></a><span data-ttu-id="75331-244">Adatmegőrzési lemez toohello Linux fő célkiszolgáló virtuális gépek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="75331-244">Add a retention disk toohello Linux master target virtual machine</span></span>

<span data-ttu-id="75331-245">A következő lépéseket toocreate adatmegőrzési lemez hello használata:</span><span class="sxs-lookup"><span data-stu-id="75331-245">Use hello following steps toocreate a retention disk:</span></span>

1. <span data-ttu-id="75331-246">Csatlakoztassa az új 1 TB méretű lemez toohello Linux fő célkiszolgáló virtuális gép, és indítsa el a hello gép.</span><span class="sxs-lookup"><span data-stu-id="75331-246">Attach a new 1-TB disk toohello Linux master target virtual machine, and then start hello machine.</span></span>

2. <span data-ttu-id="75331-247">Használjon hello **többutas -inden** parancsazonosító toolearn hello többutas hello adatmegőrzési lemez.</span><span class="sxs-lookup"><span data-stu-id="75331-247">Use hello **multipath -ll** command toolearn hello multipath ID of hello retention disk.</span></span>

    ```
    multipath -ll
    ```
    ![hello adatmegőrzési lemez hello többutas azonosítója](./media/site-recovery-how-to-install-linux-master-target/media/image22.png)

3. <span data-ttu-id="75331-249">Formázza hello meghajtót, és ezután hozzon létre egy fájlrendszer hello új meghajtón.</span><span class="sxs-lookup"><span data-stu-id="75331-249">Format hello drive, and then create a file system on hello new drive.</span></span>

    ```
    mkfs.ext4 /dev/mapper/<Retention disk's multipath id>
    ```
    ![Fájlrendszer hello meghajtón létrehozása](./media/site-recovery-how-to-install-linux-master-target/media/image23.png)

4. <span data-ttu-id="75331-251">Miután létrehozta a hello fájlrendszer, csatlakoztassa a hello megőrzési lemezt.</span><span class="sxs-lookup"><span data-stu-id="75331-251">After you create hello file system, mount hello retention disk.</span></span>
    ```
    mkdir /mnt/retention
    mount /dev/mapper/<Retention disk's multipath id> /mnt/retention
    ```
    ![Csatlakoztatását a hello adatmegőrzési lemez](./media/site-recovery-how-to-install-linux-master-target/media/image24.png)

5. <span data-ttu-id="75331-253">Hozzon létre hello **fstab** bejegyzés toomount hello adatmegőrzési meghajtó hello rendszer minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="75331-253">Create hello **fstab** entry toomount hello retention drive every time hello system starts.</span></span>
    ```
    vi /etc/fstab
    ```
    <span data-ttu-id="75331-254">Válassza ki **beszúrása** toobegin hello fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="75331-254">Select **Insert** toobegin editing hello file.</span></span> <span data-ttu-id="75331-255">Hozzon létre egy új sort, és helyezze be a következő szöveg hello.</span><span class="sxs-lookup"><span data-stu-id="75331-255">Create a new line, and then insert hello following text.</span></span> <span data-ttu-id="75331-256">Hello többutas Lemezazonosítót kiemelt hello többutas azonosító hello előző parancs alapján szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="75331-256">Edit hello disk multipath ID based on hello highlighted multipath ID from hello previous command.</span></span>

    <span data-ttu-id="75331-257"> **/dev/eseményleképező/ <Retention disks multipath id> /mnt/megőrzési ext4 rw 0 0**</span><span class="sxs-lookup"><span data-stu-id="75331-257">**/dev/mapper/<Retention disks multipath id> /mnt/retention ext4 rw 0 0**</span></span>

    <span data-ttu-id="75331-258">Válassza ki **Esc**, majd írja be **: wq** (írása, és lépjen ki a) tooclose hello szerkesztő ablakot.</span><span class="sxs-lookup"><span data-stu-id="75331-258">Select **Esc**, and then type **:wq** (write and quit) tooclose hello editor window.</span></span>

### <a name="install-hello-master-target"></a><span data-ttu-id="75331-259">Hello fő célkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="75331-259">Install hello master target</span></span>

> [!IMPORTANT]
> <span data-ttu-id="75331-260">hello hello fő célkiszolgáló kell lennie egyenlő tooor rendszernél korábbi hello folyamatkiszolgáló és a konfigurációs kiszolgáló hello hello verzióit.</span><span class="sxs-lookup"><span data-stu-id="75331-260">hello version of hello master target server must be equal tooor earlier than hello versions of hello process server and hello configuration server.</span></span> <span data-ttu-id="75331-261">Ha ez a feltétel nem teljesül, a védelem-újrabeállítási sikeres, de a replikálás mindaddig sikertelen.</span><span class="sxs-lookup"><span data-stu-id="75331-261">If this condition is not met, reprotect succeeds, but replication fails.</span></span>


> [!NOTE]
> <span data-ttu-id="75331-262">Hello fő célkiszolgáló telepítése előtt ellenőrizze, hogy hello **/etc/hosts** hello virtuálisgép-fájlt tartalmaz bejegyzéseit, amelyek leképezése hello helyi állomásnevével toohello IP-címek társított összes hálózati adapter.</span><span class="sxs-lookup"><span data-stu-id="75331-262">Before you install hello master target server, check that hello **/etc/hosts** file on hello virtual machine contains entries that map hello local hostname toohello IP addresses that are associated with all network adapters.</span></span>

1. <span data-ttu-id="75331-263">Másolja a hello jelszót **C:\ProgramData\Microsoft Azure hely Recovery\private\connection.passphrase** hello konfigurációs kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="75331-263">Copy hello passphrase from **C:\ProgramData\Microsoft Azure Site Recovery\private\connection.passphrase** on hello configuration server.</span></span> <span data-ttu-id="75331-264">Majd mentse a fájt **passphrase.txt** hello futtatásával helyi könyvtárába hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="75331-264">Then save it as **passphrase.txt** in hello same local directory by running hello following command:</span></span>

    ```
    echo <passphrase> >passphrase.txt
    ```
    <span data-ttu-id="75331-265">Példa:</span><span class="sxs-lookup"><span data-stu-id="75331-265">Example:</span></span> 
    
    ```
    echo itUx70I47uxDuUVY >passphrase.txt
    ```

2. <span data-ttu-id="75331-266">Vegye figyelembe a hello konfigurációs kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="75331-266">Note hello configuration server's IP address.</span></span> <span data-ttu-id="75331-267">Esetleg szükség lenne rá hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="75331-267">You need it in hello next step.</span></span>

3. <span data-ttu-id="75331-268">Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello és hello kiszolgáló regisztrálása hello konfigurációs kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="75331-268">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```

    <span data-ttu-id="75331-269">Példa:</span><span class="sxs-lookup"><span data-stu-id="75331-269">Example:</span></span> 
    
    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

    <span data-ttu-id="75331-270">Várjon, amíg hello parancsfájl befejeződik.</span><span class="sxs-lookup"><span data-stu-id="75331-270">Wait until hello script finishes.</span></span> <span data-ttu-id="75331-271">Ha hello fő célkiszolgáló regisztrálása sikeres volt, a fő célkiszolgáló hello szerepel-e a hello **Site Recovery-infrastruktúra** hello portál lapján.</span><span class="sxs-lookup"><span data-stu-id="75331-271">If hello master target registers sucessfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


#### <a name="install-hello-master-target-by-using-interactive-installation"></a><span data-ttu-id="75331-272">Interaktív telepítés használatával hello fő célkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="75331-272">Install hello master target by using interactive installation</span></span>

1. <span data-ttu-id="75331-273">Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello.</span><span class="sxs-lookup"><span data-stu-id="75331-273">Run hello following command tooinstall hello master target.</span></span> <span data-ttu-id="75331-274">Hello ügynök szerepkör, válasszon **fő célkiszolgáló**.</span><span class="sxs-lookup"><span data-stu-id="75331-274">For hello agent role, choose **Master Target**.</span></span>

    ```
    ./install
    ```

2. <span data-ttu-id="75331-275">Válasszon telepítési hello alapértelmezett elérési utat, majd válassza ki **Enter** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="75331-275">Choose hello default location for installation, and then select **Enter** toocontinue.</span></span>

    ![Fő célkiszolgáló telepítése az alapértelmezett hely kiválasztása](./media/site-recovery-how-to-install-linux-master-target/image17.png)

<span data-ttu-id="75331-277">Hello telepítés befejeződését követően hello konfigurációs kiszolgáló regisztrálása a hello parancssor használatával.</span><span class="sxs-lookup"><span data-stu-id="75331-277">After hello installation has finished, register hello configuration server by using hello command line.</span></span>

1. <span data-ttu-id="75331-278">Vegye figyelembe a hello hello konfigurációs kiszolgáló IP-címét.</span><span class="sxs-lookup"><span data-stu-id="75331-278">Note hello IP address of hello configuration server.</span></span> <span data-ttu-id="75331-279">Esetleg szükség lenne rá hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="75331-279">You need it in hello next step.</span></span>

2. <span data-ttu-id="75331-280">Futtassa a következő parancs tooinstall hello fő célkiszolgáló hello és hello kiszolgáló regisztrálása hello konfigurációs kiszolgálóval.</span><span class="sxs-lookup"><span data-stu-id="75331-280">Run hello following command tooinstall hello master target server and register hello server with hello configuration server.</span></span>

    ```
    ./install -q -d /usr/local/ASR -r MT -v VmWare
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i <ConfigurationServer IP Address> -P passphrase.txt
    ```
    <span data-ttu-id="75331-281">Példa:</span><span class="sxs-lookup"><span data-stu-id="75331-281">Example:</span></span> 

    ```
    /usr/local/ASR/Vx/bin/UnifiedAgentConfigurator.sh -i 104.40.75.37 -P passphrase.txt
    ```

   <span data-ttu-id="75331-282">Várjon, amíg hello parancsfájl befejeződik.</span><span class="sxs-lookup"><span data-stu-id="75331-282">Wait until hello script finishes.</span></span> <span data-ttu-id="75331-283">Ha hello fő célkiszolgáló sikeresen regisztrált, a fő célkiszolgáló hello szerepel-e a hello **Site Recovery-infrastruktúra** hello portál lapján.</span><span class="sxs-lookup"><span data-stu-id="75331-283">If hello master target is registered succesfully, hello master target is listed on hello **Site Recovery Infrastructure** page of hello portal.</span></span>


### <a name="upgrade-hello-master-target"></a><span data-ttu-id="75331-284">A fő célkiszolgáló hello frissítése</span><span class="sxs-lookup"><span data-stu-id="75331-284">Upgrade hello master target</span></span>

<span data-ttu-id="75331-285">Hello telepítő futtatásához.</span><span class="sxs-lookup"><span data-stu-id="75331-285">Run hello installer.</span></span> <span data-ttu-id="75331-286">Automatikusan észleli, hogy hello ügynök telepítve van a hello fő célkiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="75331-286">It automatically detects that hello agent is installed on hello master target.</span></span> <span data-ttu-id="75331-287">Jelölje be tooupgrade **Y**.  Hello telepítő befejezése után ellenőrizze a hello hello fő célkiszolgáló hello a következő parancs használatával telepített verzióját.</span><span class="sxs-lookup"><span data-stu-id="75331-287">tooupgrade, select **Y**.  After hello setup has been completed, check hello version of hello master target installed by using hello following command.</span></span>

    ```
    cat /usr/local/.vx_version
    ```

<span data-ttu-id="75331-288">Láthatja, hogy hello **verzió** mező ad hello fő célkiszolgáló hello verziószámát.</span><span class="sxs-lookup"><span data-stu-id="75331-288">You can see that hello **Version** field gives hello version number of hello master target.</span></span>

### <a name="install-vmware-tools-on-hello-master-target-server"></a><span data-ttu-id="75331-289">A VMware-eszközök hello fő célkiszolgáló telepítése</span><span class="sxs-lookup"><span data-stu-id="75331-289">Install VMware tools on hello master target server</span></span>

<span data-ttu-id="75331-290">Tooinstall VMware-eszközök hello fő célkiszolgáló kell, hogy azt felderíthesse hello adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="75331-290">You need tooinstall VMware tools on hello master target so that it can discover hello data stores.</span></span> <span data-ttu-id="75331-291">Hello eszközök telepítése nem történik meg, ha hello védelem-újrabeállítási képernyőn nem szerepel a listában a hello adattárolókhoz.</span><span class="sxs-lookup"><span data-stu-id="75331-291">If hello tools are not installed, hello reprotect screen isn't listed in hello data stores.</span></span> <span data-ttu-id="75331-292">Hello VMware-eszközök telepítését, miután toorestart kell.</span><span class="sxs-lookup"><span data-stu-id="75331-292">After installation of hello VMware tools, you need toorestart.</span></span>

## <a name="next-steps"></a><span data-ttu-id="75331-293">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="75331-293">Next steps</span></span>
<span data-ttu-id="75331-294">Hello telepítése és regisztrálása a hello fő célkiszolgáló rendelkezik finsihed, miután hello fő célkiszolgáló hello megjelenő láthatja **fő célkiszolgáló** szakasz **Site Recovery-infrastruktúra**, hello alatt konfigurációs kiszolgáló áttekintése.</span><span class="sxs-lookup"><span data-stu-id="75331-294">After hello installation and registration of hello master target has finsihed, you can see hello master target appear on hello **Master Target** section in **Site Recovery Infrastructure**, under hello configuration server overview.</span></span>

<span data-ttu-id="75331-295">Most már folytathatja az [ismételt védelem](site-recovery-how-to-reprotect.md), feladat-visszavétel követ.</span><span class="sxs-lookup"><span data-stu-id="75331-295">You can now proceed with [reprotection](site-recovery-how-to-reprotect.md), followed by failback.</span></span>

## <a name="common-issues"></a><span data-ttu-id="75331-296">Gyakori problémák</span><span class="sxs-lookup"><span data-stu-id="75331-296">Common issues</span></span>

* <span data-ttu-id="75331-297">Győződjön meg arról, hogy ne kapcsolja be az összes felügyeleti összetevőkön, például az olyan fő célkiszolgálót, a Storage vmotion szolgáltatások használata.</span><span class="sxs-lookup"><span data-stu-id="75331-297">Make sure you do not turn on Storage vMotion on any management components such as a master target.</span></span> <span data-ttu-id="75331-298">Ha hello fő célkiszolgáló sikeres védelem-újrabeállítási után helyezi át, nem lehet leválasztani hello virtuálisgép-lemezeket (VMDKs).</span><span class="sxs-lookup"><span data-stu-id="75331-298">If hello master target moves after a successful reprotect, hello virtual machine disks (VMDKs) cannot be detached.</span></span> <span data-ttu-id="75331-299">Ebben az esetben a feladat-visszavétel sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="75331-299">In this case, failback fails.</span></span>

* <span data-ttu-id="75331-300">hello fő célkiszolgáló nem kell meglévő pillanatképeket hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="75331-300">hello master target should not have any snapshots on hello virtual machine.</span></span> <span data-ttu-id="75331-301">Ha a pillanatképek vannak, a feladat-visszavétel sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="75331-301">If there are snapshots, failback fails.</span></span>

* <span data-ttu-id="75331-302">Miatt toosome egyéni hálózati konfigurációját egyes ügyfelek hello hálózati illesztő a rendszerindítás során le van tiltva, és hello fő célkiszolgáló ügynöke nem lehet inicializálni.</span><span class="sxs-lookup"><span data-stu-id="75331-302">Due toosome custom NIC configurations at some customers, hello network interface is disabled during startup, and hello master target agent cannot initialize.</span></span> <span data-ttu-id="75331-303">Győződjön meg arról, hogy hello következő tulajdonságai megfelelően vannak beállítva.</span><span class="sxs-lookup"><span data-stu-id="75331-303">Make sure that hello following properties are correctly set.</span></span> <span data-ttu-id="75331-304">Ellenőrizze, hogy ezeket a tulajdonságokat a hello Ethernet kártya fájl /etc/sysconfig/network-scripts/ifcfg-eth *.</span><span class="sxs-lookup"><span data-stu-id="75331-304">Check these properties in hello Ethernet card file's /etc/sysconfig/network-scripts/ifcfg-eth*.</span></span>
    * <span data-ttu-id="75331-305">BOOTPROTO = dhcp</span><span class="sxs-lookup"><span data-stu-id="75331-305">BOOTPROTO=dhcp</span></span>
    * <span data-ttu-id="75331-306">ONBOOT = Igen</span><span class="sxs-lookup"><span data-stu-id="75331-306">ONBOOT=yes</span></span>
