A következő olyan kiszolgálókat hello fürt Windows Server 2008 R2 vagy Windows Server 2012 rendszer esetén ellenőriznie kell, hogy hello gyorsjavítás [KB2854082](http://support.microsoft.com/kb/2854082) egyes hello a helyszíni kiszolgálók vagy Azure virtuális gépek hello fürt részét képező telepítve van. Bármely kiszolgáló vagy a virtuális Gépet, amely hello fürtben, de nem hello a rendelkezésre állási csoportban van lehet ez a gyorsjavítás.

Hello a távoli asztali munkamenetgazda az egyes fürtcsomópontok hello, töltse le [KB2854082](http://support.microsoft.com/kb/2854082) tooa helyi könyvtárba. Ezután telepítse hello gyorsjavítást a fürt minden csomópontján egymás után. Ha hello fürtszolgáltatás hello fürtcsomóponton jelenleg fut, hello kiszolgáló újraindul hello gyorsjavítás telepítési hello végén.

> [!WARNING]
> Hello Fürtszolgáltatás leállítása vagy újraindítása hello server hatással van a fürt és hello rendelkezésre állási csoport állapotának hello kvórum, és előfordulhat, hogy a fürt toogo offline állapotba. toomaintain hello magas rendelkezésre állás a fürt telepítésekor, győződjön meg arról, hogy:
> 
> * hello fürt nem optimális kvórum állapotát. 
> * Minden csomóponton hello gyorsjavítás telepítése előtt a fürt összes csomópontján online állapotban.
> * Hello fürt többi csomópontjára hello gyorsjavítás telepítése előtt engedélyezi hello gyorsjavítás telepítési toorun toocompletion egy csomóponton, beleértve a teljes mértékben a hello kiszolgáló újraindítása.
> 
> 

