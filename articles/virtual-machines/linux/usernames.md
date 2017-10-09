---
title: "Linux felhasználónevek aaaSelecting |} Microsoft Docs"
description: "Ismerje meg, hogyan tooselect felhasználó nevét, a Linux virtuális gép az Azure-ban."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 33b50c97-92f1-46c9-a623-e37f67459c5c
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: c65e2ac46f40bb8c9d74cccbaf248a070c0fa6cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="selecting-user-names-for-linux-on-azure"></a><span data-ttu-id="fc844-103">Felhasználónevek kiválasztása Azure-ban futtatott Linux esetén</span><span class="sxs-lookup"><span data-stu-id="fc844-103">Selecting User Names for Linux on Azure</span></span>
[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="fc844-104">Amikor egy Linux virtuális gépet az Azure később használható toolog hello VM a nem rendszergazda felhasználó hello nevét kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="fc844-104">When you provision a Linux virtual machine on Azure you must specify hello name of a non-root user that you can later use toolog into hello VM.</span></span> <span data-ttu-id="fc844-105">Úgy is dönthet, hello hello új felhasználó nevét, vagy ha a beállítás a klasszikus Azure portálon hello elfogadhatja hello alapértelmezett neve "azureuser".</span><span class="sxs-lookup"><span data-stu-id="fc844-105">You may choose hello name of hello new user, or if provisioning via hello Azure classic portal you can accept hello default name "azureuser".</span></span>

<span data-ttu-id="fc844-106">A legtöbb esetben ez a felhasználó nem található a hello alapjául szolgáló lemezképhez, és hello kiépítési folyamat során jön létre.</span><span class="sxs-lookup"><span data-stu-id="fc844-106">In most cases this user won't exist on hello base image and will be created during hello provisioning process.</span></span> <span data-ttu-id="fc844-107">Ha alapszintű hello Virtuálisgép-lemezkép hello létezik, akkor hello Azure Linux ügynök egyszerűen konfigurál hello jelszó vagy SSH-kulcs az adott felhasználó hello hello virtuális gép létrehozásakor a megadott adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="fc844-107">If hello user exists on hello base VM image, then hello Azure Linux agent simply configures hello password and/or SSH key for that user based on hello information you specified when creating hello VM.</span></span>

<span data-ttu-id="fc844-108">**Azonban**, Linux, amely nem használható felhasználónevek álló készletet határoz meg.</span><span class="sxs-lookup"><span data-stu-id="fc844-108">**However**, Linux defines a set of user names that should not be used.</span></span> <span data-ttu-id="fc844-109">kiépítési folyamat lesz hello **sikertelen** Ha tooprovision Linux virtuális gép egy meglévő rendszer felhasználó, amelyet a 0 – 99 UID rendelkező felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="fc844-109">hello provisioning process will **fail** if you try tooprovision a Linux VM using an existing system user, which is defined as a user with UID 0-99.</span></span> <span data-ttu-id="fc844-110">Tipikus példája hello `root` felhasználó, amelynek UID 0.</span><span class="sxs-lookup"><span data-stu-id="fc844-110">A typical example is hello `root` user, which has UID 0.</span></span>

* <span data-ttu-id="fc844-111">További információ: [Linux Standard Base - felhasználói azonosító tartományok](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span><span class="sxs-lookup"><span data-stu-id="fc844-111">See also: [Linux Standard Base - User ID Ranges](http://refspecs.linuxfoundation.org/LSB_4.1.0/LSB-Core-generic/LSB-Core-generic/uidrange.html)</span></span>

<span data-ttu-id="fc844-112">hello CentOS és Ubuntu, hogy azt ne használja az Azure-on Linux virtuális gépek kiépítése közös beépített rendszer felhasználók listája látható.</span><span class="sxs-lookup"><span data-stu-id="fc844-112">hello following is a list of common built-in system users for CentOS and Ubuntu that you should avoid using when provisioning a Linux virtual machine on Azure.</span></span> <span data-ttu-id="fc844-113">A lista létrehozási csak egy példa, toohello dokumentáció a részletek a telepítési tooensure úgy dönt, nem ütközik egy meglévő felhasználóval rendszer hello felhasználónévhez.</span><span class="sxs-lookup"><span data-stu-id="fc844-113">This list is just an example, please refer toohello documentation for your distribution tooensure that hello username you choose does not conflict with an existing system user.</span></span>

## <a name="centos"></a><span data-ttu-id="fc844-114">CentOS</span><span class="sxs-lookup"><span data-stu-id="fc844-114">CentOS</span></span>
* <span data-ttu-id="fc844-115">abrt</span><span class="sxs-lookup"><span data-stu-id="fc844-115">abrt</span></span>
* <span data-ttu-id="fc844-116">ADM</span><span class="sxs-lookup"><span data-stu-id="fc844-116">adm</span></span>
* <span data-ttu-id="fc844-117">hang</span><span class="sxs-lookup"><span data-stu-id="fc844-117">audio</span></span>
* <span data-ttu-id="fc844-118">bin</span><span class="sxs-lookup"><span data-stu-id="fc844-118">bin</span></span>
* <span data-ttu-id="fc844-119">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="fc844-119">cdrom</span></span>
* <span data-ttu-id="fc844-120">cgred</span><span class="sxs-lookup"><span data-stu-id="fc844-120">cgred</span></span>
* <span data-ttu-id="fc844-121">démon</span><span class="sxs-lookup"><span data-stu-id="fc844-121">daemon</span></span>
* <span data-ttu-id="fc844-122">dbus</span><span class="sxs-lookup"><span data-stu-id="fc844-122">dbus</span></span>
* <span data-ttu-id="fc844-123">kitárcsázáshoz</span><span class="sxs-lookup"><span data-stu-id="fc844-123">dialout</span></span>
* <span data-ttu-id="fc844-124">DIP</span><span class="sxs-lookup"><span data-stu-id="fc844-124">dip</span></span>
* <span data-ttu-id="fc844-125">lemez</span><span class="sxs-lookup"><span data-stu-id="fc844-125">disk</span></span>
* <span data-ttu-id="fc844-126">hajlékonylemez</span><span class="sxs-lookup"><span data-stu-id="fc844-126">floppy</span></span>
* <span data-ttu-id="fc844-127">FTP</span><span class="sxs-lookup"><span data-stu-id="fc844-127">ftp</span></span>
* <span data-ttu-id="fc844-128">FTP</span><span class="sxs-lookup"><span data-stu-id="fc844-128">ftp</span></span>
* <span data-ttu-id="fc844-129">játékok</span><span class="sxs-lookup"><span data-stu-id="fc844-129">games</span></span>
* <span data-ttu-id="fc844-130">Gopher</span><span class="sxs-lookup"><span data-stu-id="fc844-130">gopher</span></span>
* <span data-ttu-id="fc844-131">haldaemon</span><span class="sxs-lookup"><span data-stu-id="fc844-131">haldaemon</span></span>
* <span data-ttu-id="fc844-132">halt</span><span class="sxs-lookup"><span data-stu-id="fc844-132">halt</span></span>
* <span data-ttu-id="fc844-133">kmem</span><span class="sxs-lookup"><span data-stu-id="fc844-133">kmem</span></span>
* <span data-ttu-id="fc844-134">zárolás</span><span class="sxs-lookup"><span data-stu-id="fc844-134">lock</span></span>
* <span data-ttu-id="fc844-135">LP</span><span class="sxs-lookup"><span data-stu-id="fc844-135">lp</span></span>
* <span data-ttu-id="fc844-136">mail</span><span class="sxs-lookup"><span data-stu-id="fc844-136">mail</span></span>
* <span data-ttu-id="fc844-137">Man</span><span class="sxs-lookup"><span data-stu-id="fc844-137">man</span></span>
* <span data-ttu-id="fc844-138">Hiba</span><span class="sxs-lookup"><span data-stu-id="fc844-138">mem</span></span>
* <span data-ttu-id="fc844-139">nfsnobody</span><span class="sxs-lookup"><span data-stu-id="fc844-139">nfsnobody</span></span>
* <span data-ttu-id="fc844-140">senki sem</span><span class="sxs-lookup"><span data-stu-id="fc844-140">nobody</span></span>
* <span data-ttu-id="fc844-141">NTP</span><span class="sxs-lookup"><span data-stu-id="fc844-141">ntp</span></span>
* <span data-ttu-id="fc844-142">Operátor</span><span class="sxs-lookup"><span data-stu-id="fc844-142">operator</span></span>
* <span data-ttu-id="fc844-143">oprofile</span><span class="sxs-lookup"><span data-stu-id="fc844-143">oprofile</span></span>
* <span data-ttu-id="fc844-144">postdrop</span><span class="sxs-lookup"><span data-stu-id="fc844-144">postdrop</span></span>
* <span data-ttu-id="fc844-145">utótag</span><span class="sxs-lookup"><span data-stu-id="fc844-145">postfix</span></span>
* <span data-ttu-id="fc844-146">qpidd</span><span class="sxs-lookup"><span data-stu-id="fc844-146">qpidd</span></span>
* <span data-ttu-id="fc844-147">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="fc844-147">root</span></span>
* <span data-ttu-id="fc844-148">RPC</span><span class="sxs-lookup"><span data-stu-id="fc844-148">rpc</span></span>
* <span data-ttu-id="fc844-149">rpcuser</span><span class="sxs-lookup"><span data-stu-id="fc844-149">rpcuser</span></span>
* <span data-ttu-id="fc844-150">saslauth</span><span class="sxs-lookup"><span data-stu-id="fc844-150">saslauth</span></span>
* <span data-ttu-id="fc844-151">Leállítás</span><span class="sxs-lookup"><span data-stu-id="fc844-151">shutdown</span></span>
* <span data-ttu-id="fc844-152">slocate</span><span class="sxs-lookup"><span data-stu-id="fc844-152">slocate</span></span>
* <span data-ttu-id="fc844-153">sshd</span><span class="sxs-lookup"><span data-stu-id="fc844-153">sshd</span></span>
* <span data-ttu-id="fc844-154">stapdev</span><span class="sxs-lookup"><span data-stu-id="fc844-154">stapdev</span></span>
* <span data-ttu-id="fc844-155">stapusr</span><span class="sxs-lookup"><span data-stu-id="fc844-155">stapusr</span></span>
* <span data-ttu-id="fc844-156">Szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="fc844-156">sync</span></span>
* <span data-ttu-id="fc844-157">sys</span><span class="sxs-lookup"><span data-stu-id="fc844-157">sys</span></span>
* <span data-ttu-id="fc844-158">szalag</span><span class="sxs-lookup"><span data-stu-id="fc844-158">tape</span></span>
* <span data-ttu-id="fc844-159">Teszt</span><span class="sxs-lookup"><span data-stu-id="fc844-159">test</span></span>
* <span data-ttu-id="fc844-160">tcpdump parancsot</span><span class="sxs-lookup"><span data-stu-id="fc844-160">tcpdump</span></span>
* <span data-ttu-id="fc844-161">TTY</span><span class="sxs-lookup"><span data-stu-id="fc844-161">tty</span></span>
* <span data-ttu-id="fc844-162">felhasználók</span><span class="sxs-lookup"><span data-stu-id="fc844-162">users</span></span>
* <span data-ttu-id="fc844-163">utempter</span><span class="sxs-lookup"><span data-stu-id="fc844-163">utempter</span></span>
* <span data-ttu-id="fc844-164">utmp</span><span class="sxs-lookup"><span data-stu-id="fc844-164">utmp</span></span>
* <span data-ttu-id="fc844-165">uucp</span><span class="sxs-lookup"><span data-stu-id="fc844-165">uucp</span></span>
* <span data-ttu-id="fc844-166">vcsa</span><span class="sxs-lookup"><span data-stu-id="fc844-166">vcsa</span></span>
* <span data-ttu-id="fc844-167">Videó</span><span class="sxs-lookup"><span data-stu-id="fc844-167">video</span></span>
* <span data-ttu-id="fc844-168">kerék</span><span class="sxs-lookup"><span data-stu-id="fc844-168">wheel</span></span>

## <a name="ubuntu"></a><span data-ttu-id="fc844-169">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="fc844-169">Ubuntu</span></span>
* <span data-ttu-id="fc844-170">ADM</span><span class="sxs-lookup"><span data-stu-id="fc844-170">adm</span></span>
* <span data-ttu-id="fc844-171">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="fc844-171">admin</span></span>
* <span data-ttu-id="fc844-172">hang</span><span class="sxs-lookup"><span data-stu-id="fc844-172">audio</span></span>
* <span data-ttu-id="fc844-173">biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="fc844-173">backup</span></span>
* <span data-ttu-id="fc844-174">bin</span><span class="sxs-lookup"><span data-stu-id="fc844-174">bin</span></span>
* <span data-ttu-id="fc844-175">CD-ROM</span><span class="sxs-lookup"><span data-stu-id="fc844-175">cdrom</span></span>
* <span data-ttu-id="fc844-176">crontab</span><span class="sxs-lookup"><span data-stu-id="fc844-176">crontab</span></span>
* <span data-ttu-id="fc844-177">démon</span><span class="sxs-lookup"><span data-stu-id="fc844-177">daemon</span></span>
* <span data-ttu-id="fc844-178">kitárcsázáshoz</span><span class="sxs-lookup"><span data-stu-id="fc844-178">dialout</span></span>
* <span data-ttu-id="fc844-179">DIP</span><span class="sxs-lookup"><span data-stu-id="fc844-179">dip</span></span>
* <span data-ttu-id="fc844-180">lemez</span><span class="sxs-lookup"><span data-stu-id="fc844-180">disk</span></span>
* <span data-ttu-id="fc844-181">Faxkiszolgáló</span><span class="sxs-lookup"><span data-stu-id="fc844-181">fax</span></span>
* <span data-ttu-id="fc844-182">hajlékonylemez</span><span class="sxs-lookup"><span data-stu-id="fc844-182">floppy</span></span>
* <span data-ttu-id="fc844-183">biztosító</span><span class="sxs-lookup"><span data-stu-id="fc844-183">fuse</span></span>
* <span data-ttu-id="fc844-184">játékok</span><span class="sxs-lookup"><span data-stu-id="fc844-184">games</span></span>
* <span data-ttu-id="fc844-185">gnats</span><span class="sxs-lookup"><span data-stu-id="fc844-185">gnats</span></span>
* <span data-ttu-id="fc844-186">IRC</span><span class="sxs-lookup"><span data-stu-id="fc844-186">irc</span></span>
* <span data-ttu-id="fc844-187">kmem</span><span class="sxs-lookup"><span data-stu-id="fc844-187">kmem</span></span>
* <span data-ttu-id="fc844-188">fekvő</span><span class="sxs-lookup"><span data-stu-id="fc844-188">landscape</span></span>
* <span data-ttu-id="fc844-189">libuuid</span><span class="sxs-lookup"><span data-stu-id="fc844-189">libuuid</span></span>
* <span data-ttu-id="fc844-190">lista</span><span class="sxs-lookup"><span data-stu-id="fc844-190">list</span></span>
* <span data-ttu-id="fc844-191">LP</span><span class="sxs-lookup"><span data-stu-id="fc844-191">lp</span></span>
* <span data-ttu-id="fc844-192">mail</span><span class="sxs-lookup"><span data-stu-id="fc844-192">mail</span></span>
* <span data-ttu-id="fc844-193">Man</span><span class="sxs-lookup"><span data-stu-id="fc844-193">man</span></span>
* <span data-ttu-id="fc844-194">MessageBus</span><span class="sxs-lookup"><span data-stu-id="fc844-194">messagebus</span></span>
* <span data-ttu-id="fc844-195">mlocate</span><span class="sxs-lookup"><span data-stu-id="fc844-195">mlocate</span></span>
* <span data-ttu-id="fc844-196">netdev</span><span class="sxs-lookup"><span data-stu-id="fc844-196">netdev</span></span>
* <span data-ttu-id="fc844-197">Hírek</span><span class="sxs-lookup"><span data-stu-id="fc844-197">news</span></span>
* <span data-ttu-id="fc844-198">senki sem</span><span class="sxs-lookup"><span data-stu-id="fc844-198">nobody</span></span>
* <span data-ttu-id="fc844-199">nogroup</span><span class="sxs-lookup"><span data-stu-id="fc844-199">nogroup</span></span>
* <span data-ttu-id="fc844-200">Operátor</span><span class="sxs-lookup"><span data-stu-id="fc844-200">operator</span></span>
* <span data-ttu-id="fc844-201">plugdev</span><span class="sxs-lookup"><span data-stu-id="fc844-201">plugdev</span></span>
* <span data-ttu-id="fc844-202">Proxy</span><span class="sxs-lookup"><span data-stu-id="fc844-202">proxy</span></span>
* <span data-ttu-id="fc844-203">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="fc844-203">root</span></span>
* <span data-ttu-id="fc844-204">SASL</span><span class="sxs-lookup"><span data-stu-id="fc844-204">sasl</span></span>
* <span data-ttu-id="fc844-205">árnyékmásolat</span><span class="sxs-lookup"><span data-stu-id="fc844-205">shadow</span></span>
* <span data-ttu-id="fc844-206">src</span><span class="sxs-lookup"><span data-stu-id="fc844-206">src</span></span>
* <span data-ttu-id="fc844-207">ssh</span><span class="sxs-lookup"><span data-stu-id="fc844-207">ssh</span></span>
* <span data-ttu-id="fc844-208">sshd</span><span class="sxs-lookup"><span data-stu-id="fc844-208">sshd</span></span>
* <span data-ttu-id="fc844-209">személyzet</span><span class="sxs-lookup"><span data-stu-id="fc844-209">staff</span></span>
* <span data-ttu-id="fc844-210">sudo</span><span class="sxs-lookup"><span data-stu-id="fc844-210">sudo</span></span>
* <span data-ttu-id="fc844-211">Szinkronizálás</span><span class="sxs-lookup"><span data-stu-id="fc844-211">sync</span></span>
* <span data-ttu-id="fc844-212">sys</span><span class="sxs-lookup"><span data-stu-id="fc844-212">sys</span></span>
* <span data-ttu-id="fc844-213">syslog</span><span class="sxs-lookup"><span data-stu-id="fc844-213">syslog</span></span>
* <span data-ttu-id="fc844-214">szalag</span><span class="sxs-lookup"><span data-stu-id="fc844-214">tape</span></span>
* <span data-ttu-id="fc844-215">TTY</span><span class="sxs-lookup"><span data-stu-id="fc844-215">tty</span></span>
* <span data-ttu-id="fc844-216">felhasználók</span><span class="sxs-lookup"><span data-stu-id="fc844-216">users</span></span>
* <span data-ttu-id="fc844-217">utmp</span><span class="sxs-lookup"><span data-stu-id="fc844-217">utmp</span></span>
* <span data-ttu-id="fc844-218">uucp</span><span class="sxs-lookup"><span data-stu-id="fc844-218">uucp</span></span>
* <span data-ttu-id="fc844-219">Videó</span><span class="sxs-lookup"><span data-stu-id="fc844-219">video</span></span>
* <span data-ttu-id="fc844-220">hangalámondás</span><span class="sxs-lookup"><span data-stu-id="fc844-220">voice</span></span>
* <span data-ttu-id="fc844-221">whoopsie</span><span class="sxs-lookup"><span data-stu-id="fc844-221">whoopsie</span></span>
* <span data-ttu-id="fc844-222">www-adatok</span><span class="sxs-lookup"><span data-stu-id="fc844-222">www-data</span></span>

