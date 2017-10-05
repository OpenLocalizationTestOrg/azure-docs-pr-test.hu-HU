---
title: "Távolítsa el a kiszolgálókat, és tiltsa le a védelmet |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan a Site Recovery-tároló kiszolgálók regisztrációját, és tiltsa le a védelmet a virtuális gépek és fizikai kiszolgálók."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a>Kiszolgálók eltávolítása és a védelem letiltása

Az üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégia hozzájárul az Azure Site Recovery szolgáltatásban. A szolgáltatás koordinálja a replikációt, a feladatátvételi és a helyreállítási virtuális gépek és fizikai kiszolgálók. A gépeket az Azure-ba, vagy egy másodlagos helyszíni adatközpontba replikálhatja. A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.

Ez a cikk ismerteti az Azure portálon Recovery Services-tároló kiszolgálók regisztrációját, és tiltsa le a védelmet a Site Recovery által védett gépek.

Megjegyzéseit vagy kérdéseit a cikk alján, vagy az [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr) teheti fel.

## <a name="unregister-a-connected-configuration-server"></a>A csatlakoztatott konfigurációs kiszolgáló regisztrációját

Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók replikálása az Azure-ba, az alábbiak szerint tudja törölni a tárolóból egy csatlakoztatott konfigurációs kiszolgáló:

1. Tiltsa le a gép védelmét. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.
2. Szüntesse meg a szabályzatoknak. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **replikációs házirendek**, kattintson duplán a tartozó házirend. Kattintson a jobb gombbal a kiszolgáló > **Disassociate**.
3. Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal a kiszolgáló > **törlése**.
4. A konfigurációs kiszolgáló törlése.
5. Manuálisan távolítsa el a fő célkiszolgálón futó mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a konfigurációs kiszolgálón futó).
6. Távolítsa el a további folyamat kiszolgálókat.
7. Távolítsa el a konfigurációs kiszolgálót.
8. A konfigurációs kiszolgálón, távolítsa el a MySQL Site Recovery által telepített példányát.
9. A konfigurációs kiszolgáló beállításjegyzékében a kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>A frissíthető konfigurációs kiszolgáló regisztrációját

Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók replikálása az Azure-ba, az alábbiak szerint tudja törölni a tárolóból egy frissíthető konfigurációs kiszolgáló:

1. Tiltsa le a gép védelmét. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**. Válassza ki **gép kezelésének leállítása**.
2. Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal a kiszolgáló > **törlése**.
3. A konfigurációs kiszolgáló törlése.
4. Manuálisan távolítsa el a fő célkiszolgálón futó mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a konfigurációs kiszolgálón futó).
5. Távolítsa el a további folyamat kiszolgálókat.
6. Távolítsa el a konfigurációs kiszolgálót.
7. A konfigurációs kiszolgálón, távolítsa el a MySQL Site Recovery által telepített példányát.
8. A konfigurációs kiszolgáló beállításjegyzékében a kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Egy csatlakoztatott VMM-kiszolgáló regisztrációjának törlése

Ajánlott eljárásként azt javasoljuk, hogy a VMM-kiszolgáló regisztrációjának törlése Azure való csatlakozáskor. Ez biztosítja, hogy a VMM-kiszolgáló (és más VMM-kiszolgálókon és párosított) beállításainak megtisztítva megfelelően. Frissíthető kiszolgáló csak akkor törlése, ha probléma van a végleges megfeleléssel összefüggő. Ha a VMM-kiszolgáló nincs csatlakoztatva, szüksége lesz manuálisan a beállításokat egy parancsfájlt futtathat.

1. Állítsa le a felhőket a VMM-kiszolgálón el szeretné távolítani a virtuális gépek replikálása.
2. Törölje a törölni kívánt VMM-kiszolgálón felhők által használt hálózati leképezések. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés > **törlése**.
3. Felhőből a VMM-kiszolgálón szeretné eltávolítani a replikációs házirend társítását.  A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a tartozó házirend. Kattintson a jobb gombbal a felhő > **Disassociate**.
4. Törölje a VMM-kiszolgáló vagy az aktív VMM-csomópont. A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal a kiszolgáló > **törlése**.
5. Távolítsa el manuálisan a VMM-kiszolgálón a szolgáltató. Ha rendelkezik fürttel, távolítsa el az összes csomópontot.
6. Ha az Azure-bA replikál, manuálisan távolítsa el a Microsoft a Recovery Services agent Hyper-V gazdagépek a törölt felhőkben.



### <a name="unregister-an-unconnected-vmm-server"></a>Az frissíthető a VMM-kiszolgáló regisztrációjának törlése

1. Állítsa le a felhőket a VMM-kiszolgálón el szeretné távolítani a virtuális gépek replikálása.
2. Törölje a VMM-kiszolgálón, amely a törölni kívánt felhők által használt hálózati leképezések. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés > **törlése**.
3. Vegye figyelembe a VMM-kiszolgáló Azonosítóját.
4. Felhőből a VMM-kiszolgálón szeretné eltávolítani a replikációs házirend társítását.  A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a tartozó házirend. Kattintson a jobb gombbal a felhő > **Disassociate**.
5. Törölje a VMM-kiszolgáló vagy az aktív csomópontra. A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal a kiszolgáló > **törlése**.
6. Töltse le és futtassa a [karbantartási parancsprogramot](http://aka.ms/asr-cleanup-script-vmm) a VMM-kiszolgálón. Nyissa meg a PowerShell használata a **Futtatás rendszergazdaként** kapcsoló, a végrehajtási házirend az alapértelmezett (LocalMachine) hatókör módosítására. A parancsfájl adja meg az Azonosítót, a VMM-kiszolgáló el szeretné távolítani. A parancsfájl a regisztrációs és felhőpárosítási adatait a kiszolgálóról távolítja el.
5. Futtassa a karbantartási parancsprogramot bármely más VMM-kiszolgálókon, amelyek tartalmazzák a felhők eltávolítani kívánt VMM-kiszolgálón van párosítva-felhő.
6. Futtassa a karbantartási parancsprogramot a bármely más passzív VMM-fürtcsomóponton, hogy a szolgáltató telepítve.
7. Távolítsa el manuálisan a VMM-kiszolgálón a szolgáltató. Ha rendelkezik fürttel, távolítsa el az összes csomópontot.
8. Ha Ön replikálása Azure-ba, eltávolíthatja a Microsoft Recovery Services Agent ügynököt a Hyper-V gazdagépekről, a törölt felhőkben.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>A Hyper-V gazdagépet a Hyper-V hely regisztrációjának törlése

A VMM által nem felügyelt Hyper-V-gazdagépek egy Hyper-V helyre gyűjti az adatokat. Távolítsa el a gazdagép egy Hyper-V hely az alábbiak szerint:

1. Tiltsa le a replikációt a gazdagépen található Hyper-V virtuális gépek esetén.
2. Szüntesse meg a Hyper-V hely házirendek. A **Site Recovery-infrastruktúra** > **a Hyper-V helyek** >  **replikációs házirendek**, kattintson duplán a tartozó házirend. Kattintson a jobb gombbal a hely > **Disassociate**.
3. Hyper-V-gazdagépek törlése. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V-gazdagépek**, kattintson a jobb gombbal a kiszolgáló > **törlése**.
4. Hyper-V hely törlése után minden gazdagép el lettek távolítva belőle. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V helyek**, kattintson a jobb gombbal a hely > **törlése**.
5. Futtassa az alábbi parancsfájlt minden Hyper-V állomáson eltávolított. A parancsfájl a szükségtelenné vált beállításait a kiszolgálón, és megszünteti azt a tárolóból.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a>Tiltsa le a védelmet a VMware virtuális gép vagy fizikai kiszolgálón

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.
2. A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:
    - **(Ajánlott) a gép védelmének letiltása**. Ez a beállítás használatával állítsa le a gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek. Ez a beállítás a következő körülmények között csak akkor jelenik meg:
        - **Már átméretezte a virtuális gép kötet**– Ha azt szeretné, hogy a kötet a virtuális gép állapota kritikus állapotba kerül. Válassza ezt a beállítást, letiltja a védelem során az Azure-ban a helyreállítási pontok megőrzése. Ha engedélyezi a gép védelmét, az átméretezett kötet adatait Azure használatával lesz áthelyezve.
        - **Nemrég futtatását feladatátvevő**– egy feladatátvételi teszt a környezet futtatása után válassza ezt a beállítást, a helyszíni gépeket újra védelmének megkezdéséhez. Letiltja a virtuális gépeken, és ezután engedélyeznie kell azokat újra védelmét. Letiltja a gépet, amelynek ez a beállítás nincs hatással a replika virtuális gép az Azure-ban. Ne távolítsa el a mobilitási szolgáltatást a számítógépről.
    - **Gép kezelésének leállítása**. Ha ezt a beállítást, a gép csak törlődni fognak a tárolóból. A helyszíni védelmi beállítások a gép nem érinti. Távolítsa el a számítógép beállításait, és távolítsa el a gépet az Azure-előfizetés, kell tisztítsa meg a beállításokat a mobilitási szolgáltatás eltávolításával.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Tiltsa le a védelmet a Hyper-V virtuális gépek VMM-felhőben

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.
2. A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:

    - **(Ajánlott) a gép védelmének letiltása**. Ez a beállítás használatával állítsa le a gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek.
    - **Gép kezelésének leállítása**. Ha ezt a beállítást, a gép csak törlődni fognak a tárolóból. A helyszíni védelmi beállítások a gép nem érinti. Távolítsa el a számítógép beállításait, és távolítsa el a gépet az Azure-előfizetés, meg kell törölnie a beállítások manuálisan, az alábbi utasításokat. Vegye figyelembe, hogy törölje a virtuális gépet és annak merevlemezei válassza ki, ha azok fogja eltávolítani a célhelyről.

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a>Védelmi beállítások - replikációt, hogy a VMM másodlagos hely törlése

Ha a kiválasztott **gép kezelésének leállítása** és Ön replikálása egy másodlagos helyre, és futtassa ezt a parancsfájlt a beállításokat az elsődleges virtuális gép az elsődleges kiszolgálón. A VMM-konzolon kattintson a PowerShell gombra a VMM PowerShell-konzol megnyitásához. SQLVM1 cserélje le a virtuális gép nevét.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. A másodlagos VMM-kiszolgálón futtassa ezt a parancsfájlt a másodlagos virtuális gép beállításait:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. A másodlagos VMM-kiszolgálón frissítse a virtuális gépek a Hyper-V gazdagép-kiszolgálón, hogy a másodlagos virtuális gép újra a VMM-konzol lekérdezi észlelte.
4. A fenti lépések kapcsolja ki a replikációs beállításokat a VMM-kiszolgálón. Ha le kívánja állítani a virtuális gép replikálását, futtassa a következő parancsfájlt bizony az elsődleges és másodlagos virtuális gépeket. SQLVM1 cserélje le a virtuális gép nevét.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a>Védelmi beállításokat - replikáció az Azure-bA

1. Ha a kiválasztott **gép kezelésének leállítása** és replikálja az Azure-ba, a parancsfájl futtatásával a forrás VMM-kiszolgálón, a PowerShell használatával a VMM-konzolról.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. A fenti lépések törölje a replikációs beállításokat a VMM-kiszolgálón. Állítsa le a replikációt a virtuális gép a Hyper-V kiszolgálón futó, futtassa a parancsfájlt. SQLVM1 cserélje le a virtuális gép, és a host01.contoso.com nevű, a Hyper-V gazdakiszolgáló nevét.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Tiltsa le a védelmet a Hyper-V virtuális gépek a Hyper-V hely

Ez az eljárás használható, ha az Azure Hyper-V virtuális gépek a VMM-kiszolgáló nem replikál.

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal a gép > **törlése**.
2. A **eltávolítása gép**, a következő lehetőségek közül választhat:

   - **(Ajánlott) a gép védelmének letiltása**. Ez a beállítás használatával állítsa le a gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek.
   - **Gép kezelésének leállítása**. Ha ezt a beállítást a gép csak törlődni fognak a tárolóból. A helyszíni védelmi beállítások a gép nem érinti. Távolítsa el a számítógép beállításait, és távolítsa el a virtuális gépet az Azure-előfizetés, tisztítása a beállításokat manuálisan kell. Ha szeretné, törölje a virtuális gépet és annak merevlemezei, akkor azok törlődnek a célhelyről.
3. Ha a kiválasztott **gép kezelésének leállítása**, futtassa ezt a parancsfájlt a forráskiszolgálón a Hyper-V gazdagép, eltávolítja a virtuális gép replikálása. SQLVM1 cserélje le a virtuális gép nevét.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
