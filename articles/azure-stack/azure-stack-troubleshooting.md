---
title: "A Microsoft Azure verem hibaelhárítása |} Microsoft Docs"
description: "Az Azure verem hibaelhárítás."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: helaw
ms.openlocfilehash: 0a8e871a3a44cb14503832d2f3a096712f8112a7
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 11/13/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>A Microsoft Azure verem hibaelhárítása

*A következőkre vonatkozik: Azure szoftverfejlesztői készletet*

A dokumentum Azure verem általános hibaelhárítási információkat tartalmaz. 

Az Azure verem műszaki szoftverfejlesztői készlet érhető el egy kiértékelési környezet, mert nincs hivatalos támogatás a Microsoft ügyfél-támogatási szolgálathoz.  Ha nem dokumentált problémát tapasztal, ellenőrizze, hogy a [Azure verem MSDN fórumon](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) további segítséget vagy arról információkat.  

Ebben a szakaszban ismertetett hibaelhárításával kapcsolatos ajánlások több forrásból származnak, és előfordulhat, hogy, vagy nem lehet feloldani az adott problémát. Kódminták kapnak, és nem lehet garantálni a kívánt eredmény elérése érdekében. Ez a szakasz az gyakori módosításokat és frissítések, a termék fejlesztései valósíthatók meg.

## <a name="deployment"></a>Környezet
### <a name="deployment-failure"></a>Központi telepítési problémái
Ha a telepítés során hibát észlel, futtassa újra a beállítást, a telepítési parancsfájl segítségével indítsa újra a sikertelen lépés a központi telepítést.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>A telepítés végén a PowerShell-munkamenet még meg nyitva, és nem jelenik meg a kimenetet
Ez a viselkedés esetén valószínűleg csak egy PowerShell-parancsablakban alapértelmezett viselkedése eredményét van kijelölve. A development kit telepítési ténylegesen sikeresen befejeződött, de a parancsfájl az ablak kiválasztásakor szünetel. Ez a helyzet a word keresi, "select" címsorában a parancssori ablakban ellenőrizheti.  Az ESC billentyű kikapcsolni azt, és az üzenet megjelenjenek-e után azt.

## <a name="virtual-machines"></a>Virtual machines (Virtuális gépek)
### <a name="default-image-and-gallery-item"></a>Alapértelmezett kép és a gyűjtemény elem
Virtuális gépek Azure-készletben telepítése előtt először adja hozzá egy Windows Server kép és a galériabeli elemet.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>A saját Azure verem állomás az újraindítás után néhány virtuális gép esetleg nem indul el automatikusan.
Miután a gazdagép újraindul, azt tapasztalhatja, Azure verem szolgáltatások nem érhetők el azonnal.  Ennek az az oka az Azure a verem [infrastruktúra virtuális gépek](azure-stack-architecture.md#virtual-machine-roles) és RPs eltarthat egy kis bit konzisztenciájának ellenőrzése, de végül indul el automatikusan.

Előfordulhat, hogy a bérlői virtuális gépek nem indul el automatikusan a rendszer az Azure verem development kit gazdagép újraindítása után is.  Ez egy ismert probléma, és online állapotba kerüljön néhány manuális szükséges:

1.  Indítsa el az Azure verem development kit állomás **Feladatátvevőfürt-kezelő** a Start menüből.
2.  Válassza ki a fürtöt **S-Cluster.azurestack.local**.
3.  Válassza ki **szerepkörök**.
4.  Bérlői virtuális gépek megjelennek a *mentett* állapotát.  Miután minden infrastruktúra virtuális gépek futnak, kattintson a jobb gombbal a bérlői virtuális gépek, és válassza ki **Start** folytatni a virtuális Gépet.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Tudom néhány virtuális gépnél, hogy törölték, de továbbra is tekintse meg a VHD-fájlok a lemezen. Ez a viselkedés várható?
Igen, ez az elvárt viselkedés. Az ezzel a módszerrel úgy lett kialakítva, mert:

* Ha töröl egy virtuális Gépet, a VHD-k nem törlődnek. Lemezek külön erőforrások az erőforráscsoportban.
* A storage-fiók törlésekor a lekérdezi a törlés azonnal Azure Resource Manageren keresztül (portál, PowerShell) látható, de a lemezek tartalmazhat továbbra is őrizze meg tárolási szemétgyűjtés futtatásáig.

Ha azt látja, hogy "árva" VHD-k, fontos tudni, hogy ha a mappa a tárfiókon törölt részét képezik. A tárfiók nem lett törölve, akkor szokásos azok továbbra is létezik.

További az adatmegőrzési küszöbértékének és az igény visszaigénylését konfigurálásával kapcsolatos [storage-fiókok kezelése](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Storage
### <a name="storage-reclamation"></a>Tárolási visszaigénylését
Megjelennek a portálon regenerált kapacitást akár a 14 órát is igénybe vehet. Terület-visszanyerést attól függ, hogy számos tényező befolyásolja, többek között a használat százalékos belső tároló fájlok blokk blob-tárolóban. Ezért attól függően, hogy mennyi adat törlődik, nincs garancia a szemétgyűjtő futtatásakor kell visszaigényelt terület mennyisége.

## <a name="windows-azure-pack-connector"></a>Windows Azure Pack-összekötő
* Ha Azure verem szoftverfejlesztői készlet telepítése után megváltoztatja a azurestackadmin fiók jelszavát, több felhőalapú mód már nem konfigurálhatja. Ezért hogy nem lehet kapcsolódni a Windows Azure Pack célkörnyezet.
* Miután beállította a többszörös felhő mód:
    * A felhasználó csak azok a beállítások alaphelyzetbe állítása után az irányítópulton látható. (A felhasználói portálon, kattintson a portál beállítások (fogaskerék ikonra a jobb felső sarokban) ikonra. A **alapértelmezett beállításainak visszaállítása**, kattintson a **alkalmaz**.)
    * Az irányítópult címek nem jelenhet meg. Ha a probléma akkor fordul elő, manuálisan kell hozzáadni azokat vissza.
    * Egyes csempék nem lehetséges, hogy megfelelően megjelenítése, amikor először hozzáadja őket az irányítópult megnyitásához. A probléma megoldásához frissítse a böngészőt.



