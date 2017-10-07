---
title: "aaaRemove kiszolgálók és a tiltsa le a védelmet |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toounregister kiszolgálót a Site Recovery tároló és a virtuális gépek és fizikai kiszolgálók toodisable védelem."
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
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a>Kiszolgálók eltávolítása és a védelem letiltása

hello Azure Site Recovery szolgáltatás hozzájárul tooyour üzleti folytonossági és vészhelyreállítási (BCDR) helyreállítási stratégiát. hello szolgáltatás koordinálja a replikációt, a feladatátvételi és a helyreállítási virtuális gépek és fizikai kiszolgálók. Gépek replikált tooAzure vagy tooa másodlagos helyszíni adatközpontba lehet. A szolgáltatás gyors áttekintéséhez olvassa el az [Azure Site Recovery szolgáltatás mibenlétével foglalkozó](site-recovery-overview.md) cikket.

Ez a cikk ismerteti, hogyan toounregister kiszolgálókat a Recovery Services hello Azure-portálon a tároló, és hogyan Site Recovery által védett gépek toodisable védelmét.

Ez a cikk vagy a hello hello alsó megjegyzések vagy kérdések utáni [Azure Recovery Services fórumon](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="unregister-a-connected-configuration-server"></a>A csatlakoztatott konfigurációs kiszolgáló regisztrációját

Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooAzure replikálja, az alábbiak szerint tudja törölni a tárolóból egy csatlakoztatott konfigurációs kiszolgáló:

1. Tiltsa le a gép védelmét. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.
2. Szüntesse meg a szabályzatoknak. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **replikációs házirendek**, kattintson duplán a hello tartozó házirend. Kattintson a jobb gombbal hello konfigurációs kiszolgáló > **Disassociate**.
3. Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal hello kiszolgáló > **Törlése**.
4. Hello konfigurációs kiszolgáló törlése.
5. Manuálisan távolítsa el a fő célkiszolgáló hello futó hello mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a hello konfigurációs kiszolgálón futó).
6. Távolítsa el a további folyamat kiszolgálókat.
7. Távolítsa el a hello konfigurációs kiszolgálót.
8. Hello konfigurációs kiszolgálón távolítsa el a Site Recovery által telepített MySQL hello példányát.
9. Hello beállításjegyzék hello konfigurációs kiszolgáló hello kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-unconnected-configuration-server"></a>A frissíthető konfigurációs kiszolgáló regisztrációját

Ha VMware virtuális gépek vagy windowsos/Linuxos fizikai kiszolgálók tooAzure replikálja, az alábbiak szerint tudja törölni a tárolóból egy frissíthető konfigurációs kiszolgáló:

1. Tiltsa le a gép védelmét. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**. Válassza ki **hello gép kezelésének leállítása**.
2. Távolítsa el a további helyszíni folyamat és fő célkiszolgálóra. A **Site Recovery-infrastruktúra** > **a VMWare és fizikai gépek** > **konfigurációs kiszolgálók**, kattintson a jobb gombbal hello kiszolgáló > **Törlése**.
3. Hello konfigurációs kiszolgáló törlése.
4. Manuálisan távolítsa el a fő célkiszolgáló hello futó hello mobilitási szolgáltatás (Ez lesz az vagy egy különálló kiszolgáló vagy a hello konfigurációs kiszolgálón futó).
5. Távolítsa el a további folyamat kiszolgálókat.
6. Távolítsa el a hello konfigurációs kiszolgálót.
7. Hello konfigurációs kiszolgálón távolítsa el a Site Recovery által telepített MySQL hello példányát.
8. Hello beállításjegyzék hello konfigurációs kiszolgáló hello kulcs törlése ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.

## <a name="unregister-a-connected-vmm-server"></a>Egy csatlakoztatott VMM-kiszolgáló regisztrációjának törlése

Ajánlott eljárásként azt javasoljuk hello VMM-kiszolgáló regisztrációjának törlése, ha tooAzure van csatlakoztatva. Ez biztosítja, hogy hello VMM-kiszolgáló (és más VMM-kiszolgálókon és párosított) beállításainak megtisztítva megfelelően. Frissíthető kiszolgáló csak akkor törlése, ha probléma van a végleges megfeleléssel összefüggő. Ha hello VMM-kiszolgáló nincs csatlakoztatva, szüksége lesz toomanually futtatni egy parancsfájlt tooclean beállításokat.

1. Állítsa le a virtuális gépek VMM-kiszolgálónak tooremove hello felhőkben replikálása.
2. Törölje a felhő hello toodelete kívánt VMM-kiszolgáló által használt hálózati leképezések. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés hello >  **Törlés**.
3. Szüntesse meg a VMM-kiszolgálónak tooremove hello felhőből replikációs házirendek.  A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend. Kattintson a jobb gombbal a hello felhő > **Disassociate**.
4. Hello VMM-kiszolgáló vagy az aktív VMM-csomópont törlése. A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal hello server >  **Törlés**.
5. Távolítsa el manuálisan a VMM-kiszolgáló hello szolgáltató hello. Ha rendelkezik fürttel, távolítsa el az összes csomópontot.
6. TooAzure replikál, hello Microsoft Recovery Services Agent ügynököt manuálisan távolítsa el a Hyper-V gazdagépekről törölt hello felhőkben.



### <a name="unregister-an-unconnected-vmm-server"></a>Az frissíthető a VMM-kiszolgáló regisztrációjának törlése

1. Állítsa le a virtuális gépek VMM-kiszolgálónak tooremove hello felhőkben replikálása.
2. A felhők, amelyet az toodelete hello VMM-kiszolgáló által használt hálózati leképezéseket törölhet. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hálózatleképezés**, kattintson a jobb gombbal a hálózatra való leképezés hello >  **Törlés**.
3. Vegye figyelembe a VMM-kiszolgáló hello hello azonosítója.
4. Szüntesse meg a VMM-kiszolgálónak tooremove hello felhőből replikációs házirendek.  A **Site Recovery-infrastruktúra** > **System Center VMM** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend. Kattintson a jobb gombbal a hello felhő > **Disassociate**.
5. Törölje a hello VMM-kiszolgáló vagy az aktív csomópontra. A **Site Recovery-infrastruktúra** > **System Center VMM** > **VMM-kiszolgálókon**, kattintson a jobb gombbal hello server >  **Törlés**.
6. Töltse le és futtassa a hello [karbantartási parancsprogramot](http://aka.ms/asr-cleanup-script-vmm) hello VMM-kiszolgálón. Nyissa meg a Powershellt a hello **Futtatás rendszergazdaként** beállítás, toochange hello végrehajtási házirend hello alapértelmezett (LocalMachine) hatókör. Hello parancsfájlban adja meg a hello Azonosítóját a VMM-kiszolgálónak tooremove hello. hello parancsfájl regisztrációs és felhőpárosítási információk hello kiszolgálóról távolítja el.
5. Bármely más kiszolgálókon a VMM-felhő van párosítva a VMM-kiszolgálónak tooremove hello felhők tartalmazó hello karbantartási parancsprogramot fut.
6. Futtassa a hello karbantartási parancsprogramot a bármely más passzív VMM-fürtcsomóponton, amely hello szolgáltató telepítve van.
7. Távolítsa el manuálisan a VMM-kiszolgáló hello szolgáltató hello. Ha rendelkezik fürttel, távolítsa el az összes csomópontot.
8. Ha tooAzure replikálása, akkor eltávolíthatja hello Microsoft Recovery Services Agent ügynököt Hyper-V gazdagépek törölt hello felhőkben.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>A Hyper-V gazdagépet a Hyper-V hely regisztrációjának törlése

A VMM által nem felügyelt Hyper-V-gazdagépek egy Hyper-V helyre gyűjti az adatokat. Távolítsa el a gazdagép egy Hyper-V hely az alábbiak szerint:

1. Tiltsa le a hello gazdagépen található Hyper-V virtuális gépek replikációját.
2. Házirendek hello Hyper-V hely társítását. A **Site Recovery-infrastruktúra** > **a Hyper-V helyek** >  **replikációs házirendek**, kattintson duplán a kapcsolódó hello házirend. Kattintson a jobb gombbal hello hely > **Disassociate**.
3. Hyper-V-gazdagépek törlése. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V-gazdagépek**, kattintson a jobb gombbal hello server >  **Törlés**.
4. Hello Hyper-V hely törlése után minden gazdagép el lettek távolítva belőle. A **Site Recovery-infrastruktúra** > **System Center VMM** > **Hyper-V helyek**, kattintson a jobb gombbal hello hely >  **Törlés**.
5. Futtassa a parancsfájlt minden Hyper-V állomáson, amelyről eltávolította a következő hello. hello parancsfájl hello futtató kiszolgáló beállításain a szükségtelenné vált, és megszünteti azt a hello tárolójából.


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
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
                "Stopping hello Azure Site Recovery service..."
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

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
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

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.
2. A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:
    - **Tiltsa le a védelmet (ajánlott) hello gép**. Használja ezt a beállítást toostop hello gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek. A következő körülmények között hello ezt a lehetőséget csak akkor jelenik meg:
        - **Hello méretű kötet már átméretezte**– során egy kötet hello virtuális gép kritikus állapotba kerül. Válassza ezt a beállítást toodisables védelmének biztosítását, ugyanakkor az Azure-ban a helyreállítási pontok megőrzése. Ha engedélyezi a hello gép védelmét, hello átméretezték, ezért hello kötet fognak átvitt tooAzure.
        - **Nemrég futtatását feladatátvevő**– futtatását követően egy feladatátvételi tootest a környezetében, válassza ki a beállítás toostart újra védelme a helyszíni gépeket. Letiltja a virtuális gépeken, és akkor szüksége tooenable védelmi őket újra. Ez a beállítás letiltása hello gép nincs hatással a hello replika virtuális gép az Azure-ban. Ne távolítsa el a mobilitási szolgáltatás hello hello gépről.
    - **Hello gép kezelésének leállítása**. Ha ezt a beállítást, hello gép csak eltávolítja a hello tárolójából. A helyszíni hello gép védelmi beállításai nem érinti. tooremove beállításait hello gépen, és tooremove hello gép hello Azure-előfizetéssel, kell tooclean hello beállítások hello mobilitási szolgáltatás eltávolításával.

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a>Tiltsa le a védelmet a Hyper-V virtuális gépek VMM-felhőben

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.
2. A **eltávolítása gép**, válassza ki az alábbi lehetőségek egyikét:

    - **Tiltsa le a védelmet (ajánlott) hello gép**. Használja ezt a beállítást toostop hello gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek.
    - **Hello gép kezelésének leállítása**. Ha ezt a beállítást, hello gép csak eltávolítja a hello tárolójából. A helyszíni hello gép védelmi beállításai nem érinti. tooremove beállítások hello gépen, és tooremove hello gép hello Azure-előfizetéssel, kell tooclean hello-beállítások mentése manuálisan, a hello utasításokat az alábbi. Vegye figyelembe, hogy ha toodelete hello virtuális gép és annak merevlemezei választja, akkor fog törlődni hello célhelyet.

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a>Védelmi beállítások - replikáció tooa VMM másodlagos hely törlése

Ha a kiválasztott **hello gép kezelésének leállítása** és replikálása tooa másodlagos helyet, a parancsfájl futtatása a hello elsődleges kiszolgáló tooclean hello elsődleges virtuális gép hello beállításokat. Hello VMM-konzolon kattintson a hello PowerShell gomb tooopen hello VMM PowerShell-konzolban. Cserélje le a virtuális gép nevének hello SQLVM1.

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. Hello másodlagos VMM-kiszolgálón futtassa a parancsfájl tooclean hello másodlagos virtuális gép hello beállításokat:

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. VMM-kiszolgálón hello másodlagos hello virtuális gépek hello Hyper-V gazdakiszolgálón, frissítése, hogy hello másodlagos virtuális gép lekérdezi észlelt újra hello VMM-konzolon.
4. hello a fenti lépéseket hello a replikációs beállításokat a VMM-kiszolgáló hello törlődnek. Ha azt szeretné, hogy hello virtuális gép, futtassa a következő parancsfájl bizony hello toostop replikálása hello az elsődleges és másodlagos virtuális gépeket. Cserélje le a virtuális gép nevének hello SQLVM1.

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a>Védelmi beállításokat - replikáció tooAzure

1. Ha a kiválasztott **hello gép kezelésének leállítása** és tooAzure replikálni, futtassa ezt a parancsfájlt a forráskiszolgálón hello VMM PowerShell használatával hello VMM-konzolról.
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. hello a fenti lépéseket hello a replikációs beállításokat a VMM-kiszolgáló hello törölje. hello Hyper-V gazdakiszolgálón futó hello virtuális gép replikálása toostop futtassa ezt a parancsfájlt. Cserélje le SQLVM1 hello nevét, valamint a virtuális gép, host01.contoso.com hello nevű hello Hyper-V gazdagép-kiszolgálón.

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a>Tiltsa le a védelmet a Hyper-V virtuális gépek a Hyper-V hely

Ez az eljárás használható, ha a Hyper-V virtuális gépek tooAzure nélkül egy VMM-kiszolgáló replikál.

1. A **védett elemek** > **replikált elemek**, kattintson a jobb gombbal hello gép > **törlése**.
2. A **eltávolítása gép**, kiválaszthatja az alábbi beállítások hello:

   - **Tiltsa le a védelmet (ajánlott) hello gép**. Használja ezt a beállítást toostop hello gép replikálása. Webhely-helyreállítási beállításai automatikusan törlődnek.
   - **Hello gép kezelésének leállítása**. Ha ezt a beállítást hello gép csak törlődni fog a hello tárolójából. A helyszíni hello gép védelmi beállításai nem érinti. tooremove beállítások hello gépen, és tooremove hello a virtuális gép hello Azure-előfizetéssel, kell tooclean hello-beállítások mentése manuálisan. Ha ki kell választania toodelete hello virtuális gép és annak merevlemezei, akkor azok törlődnek a hello célhelyet.
3. Ha a kiválasztott **hello gép kezelésének leállítása**, futtassa a parancsfájl hello forrás Hyper-V gazdakiszolgálón, tooremove hello virtuális gép replikálása. Cserélje le a virtuális gép nevének hello SQLVM1.

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
