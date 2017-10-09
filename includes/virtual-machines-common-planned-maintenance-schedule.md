

## <a name="multi-and-single-instance-vms"></a>Többszörös és egypéldányos virtuális gépek
Sok ügyfél futó Azure száma akkor fontos, hogy azok ütemezhető a virtuális gépek tervezett karbantartás miatt toohello állásidő--változni mintegy 15 percre leáll--, amely akkor fordul elő, karbantartás során. Rendelkezésre állási készletek toohelp vezérlő is használhatja, amikor kiosztott virtuális gépek tervezett karbantartás.

Nincsenek Azure-on futó virtuális gépek két lehetséges konfigurációkat. Virtuális gépek vagy többpéldányos vagy egypéldányos kell konfigurálni. Ha virtuális gépek rendelkezésre állási csoportba, majd vannak konfigurálva több példányt. Vegye figyelembe, még akkor is egyetlen, virtuális gépek is telepíthető egy rendelkezésre állási csoportot, hogy több példányt kezelje őket. Ha a virtuális gépek nincsenek egy rendelkezésre állási csoportot, majd vannak konfigurálva, Egypéldányos.  További részletek a rendelkezésre állási csoportok: [kezelése hello rendelkezésre állása a Windows virtuális gépek](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) vagy [a Linux virtuális gépek rendelkezésre állásának kezelése hello](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Tervezett karbantartás frissíti toosingle-példányt, és többpéldányos virtuális gépek külön-külön történik. A virtuális gépek toobe egypéldányos újrakonfigurálása (ha többpéldányos) vagy toobe többpéldányos (ha egypéldányos) szabályozhatja, ha a virtuális Gépeik kap hello tervezett karbantartás. Lásd: [a tervezett karbantartások Azure Linux virtuális gépek](../articles/virtual-machines/linux/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) vagy [a tervezett karbantartásról, a Azure Windows virtuális gépek](../articles/virtual-machines/windows/planned-maintenance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) Azure virtuális gépek tervezett karbantartása leírását.

## <a name="for-multi-instance-configuration"></a>Többpéldányos konfiguráció
Kiválaszthatja a tervezett karbantartások hatással van a virtuális gépek üzembe helyezett egy rendelkezésre állási csoport konfigurációs feloldhatja a virtuális gépek rendelkezésre állási készletek hello idő.

1. Az e-mail elküldésekor történik tooyou hét nappal előtt hello tervezett karbantartás tooyour virtuális gépek többpéldányos konfigurációban. hello előfizetési azonosítók, és hatással hello többpéldányos virtuális gépek nevek szerepelnek hello üdvözlő e-mail törzsét.
2. E hét napban, dönthet úgy hello idő saját példányait az adott régióban eltávolításával a többpéldányos virtuális gépek a rendelkezésre állási csoport frissítése. Ez a változás az újraindítás, mint a virtuális gép hello mozgatása egy fizikai gazdagép, konfigurációs okok karbantartási, tooanother fizikai gazdagép nem karbantartási szánt szánt.
3. A rendelkezésre állási hello Azure-portálon csoport hello VM eltávolíthat.

   1. Jelölje ki hello VM tooremove hello portal hello rendelkezésre állási csoportban.  

   2. A **beállítások**, kattintson a **rendelkezésre állási csoport**.

      ![Rendelkezésre állási csoport kiválasztása](./media/virtual-machines-planned-maintenance-schedule/availabilitysetselection.png)

   3. Hello rendelkezésre állási csoport legördülő menüre, válassza ki a "Nem részei egy rendelkezésre állási csoportnak."

      ![Távolítsa el a készletből](./media/virtual-machines-planned-maintenance-schedule/availabilitysetwarning.png)

   4. Hello lap tetején kattintson **mentése**. Kattintson a **Igen** tooacknowledge, hogy ez a művelet újraindítja a virtuális gép hello.

   >[!TIP]
   >Hello VM toomulti-példány később újrakonfigurálhatja felsorolt hello rendelkezésre állási csoportok kiválasztásával.

4. Virtuális gépek rendelkezésre állási készletek távolítva áthelyezett tooSingle-példány állomások és nem frissülnek a hello tervezett karbantartás tooAvailability konfigurációk beállítása során.
5. Hello frissítés tooAvailability beállítása virtuális gépek befejezése után (tooschedule hello eredeti e-mailben foglaltaknak megfelelően), adja hozzá hello virtuális gépek a rendelkezésre állási készletek programba. Rendelkezésre állási csoport tagjaivá újrakonfigurálja hello virtuális gépek több példányt, és a újraindítását eredményezi. Általában ha minden többpéldányos frissítés hello teljes Azure-alapú környezetben között megadta, Egypéldányos karbantartási követi.

A virtuális gép eltávolítása a rendelkezésre állási csoport is elérhető Azure PowerShell használatával:

```
Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Remove-AzureAvailabilitySet | Update-AzureVM
```

## <a name="for-single-instance-configuration"></a>Egypéldányos konfiguráció
Kiválaszthatja a tervezett karbantartások hatással van, virtuális gépek egypéldányos konfigurációban adja hozzá a virtuális gépeken a rendelkezésre állási készletek hello idő.

Lépésről lépésre

1. Az e-mail elküldésekor történik tooyou hét nappal előtt hello tervezett karbantartás tooVMs egypéldányos konfigurációban. hello előfizetési azonosítók, és hatással hello egypéldányos virtuális gépek nevek szerepelnek hello üdvözlő e-mail törzsét.
2. E hét napban, dönthet úgy hello idő a példányát adja hozzá az Egypéldányos virtuális gépek tooan rendelkezésre állási újraindítások beállítása ugyanabban a régióban. Ez a változás az újraindítás, mint a virtuális gép hello mozgatása egy fizikai gazdagép, konfigurációs okok karbantartási, tooanother fizikai gazdagép nem karbantartási szánt szánt.
3. Kövesse az utasításokat itt tooadd meglévő virtuális gépek rendelkezésre állási készletek hello Azure-portál és az Azure PowerShell használatával történő. (Lásd: hello Azure PowerShell-példa, amely ezeket a lépéseket.)
4. Miután a virtuális gépek több példányt voltak, tartoznak, a hello tervezett karbantartás tooSingle-példány virtuális gépeket.
5. Hello egypéldányos VM frissítése után (hello eredeti e-mailben tooschedule szerint), térhet vissza hello virtuális gépek toosingle-példány: hello virtuális gépek eltávolítása a rendelkezésre állási készletek.

A virtuális gép tooan hozzáadása a rendelkezésre állási csoportban is elérhető Azure PowerShell használatával:

    Get-AzureVM -ServiceName "<VmCloudServiceName>" -Name "<VmName>" | Set-AzureAvailabilitySet -AvailabilitySetName "<AvSetName>" | Update-AzureVM

<!--Anchors-->



<!--Link references-->
[Virtual Machines Manage Availability]: virtual-machines-windows-tutorial.md
[Understand planned versus unplanned maintenance]: virtual-machines-manage-availability.md#Understand-planned-versus-unplanned-maintenance/
