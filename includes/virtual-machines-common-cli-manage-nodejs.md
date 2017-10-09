Használata előtt a Resource Manager parancsok és sablonok toodeploy Azure hello Azure CLI és munkaterhelés erőforráscsoportok, erőforrások szüksége lesz egy fiókra a Azure-ban. Ha nem rendelkezik fiókkal, [itt feliratkozhat az ingyenes Azure-próbaidőszakra](https://azure.microsoft.com/pricing/free-trial/).

Ha még nem telepítette a hello Azure CLI és a csatlakoztatott tooyour előfizetéssel, lásd: [telepítés hello Azure CLI](../articles/cli-install-nodejs.md) hello mód beállítása túl`arm` a `azure config mode arm`, és tooAzure hello `azure login` parancsot.

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- Az Azure CLI 10 – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](../articles/virtual-machines/linux/cli-manage.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="basic-azure-resource-manager-commands-in-azure-cli"></a>Alapszintű Azure Resource Manager-parancsok az Azure CLI-ben
Ez a cikk foglalkozik alapvető parancsok fog szeretné, hogy az Azure CLI toomanage toouse és az erőforrások (elsősorban a virtuális gépek) kommunikálni az Azure-előfizetésben.  Segítségért adott parancssori kapcsolók és a lehetőségek részletes, használhat hello online utasítás segítséget és beállítások beírásával `azure <command> <subcommand> --help` vagy `azure help <command> <subcommand>`.

> [!NOTE]
> Ezek a példák nem tartalmazzák a sablonalapú műveleteket, amelyek általában ajánlottak a virtuális gépek a Resource Managerben történő üzembe helyezéséhez. További információ: [használata hello Azure CLI-t az Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md) és [központi telepítése és virtuális gépek Azure Resource Manager-sablonok segítségével kezel, és az Azure parancssori felület hello](../articles/virtual-machines/linux/create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

| Tevékenység | Resource Manager |
| --- | --- | --- |
| Hozzon létre hello legalapvetőbb VM |`azure vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password>`<br/><br/>(Hello beszerzése `image-urn` a hello `azure vm image list` parancsot. Példákért tekintse meg [ezt a cikket](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).) |
| Linux rendszerű virtuális gép létrehozása |`azure  vm create [options] <resource-group> <name> <location> -y "Linux"` |
| Windows rendszerű virtuális gép létrehozása |`azure  vm create [options] <resource-group> <name> <location> -y "Windows"` |
| Virtuális gépek felsorolása |`azure  vm list [options]` |
| Virtuális gép adatainak lekérése |`azure  vm show [options] <resource_group> <name>` |
| Virtuális gép elindítása |`azure vm start [options] <resource_group> <name>` |
| Virtuális gép leállítása |`azure vm stop [options] <resource_group> <name>` |
| Virtuális gép felszabadítása |`azure vm deallocate [options] <resource-group> <name>` |
| Virtuális gép újraindítása |`azure vm restart [options] <resource_group> <name>` |
| Virtuális gép törlése |`azure vm delete [options] <resource_group> <name>` |
| Virtuális gép rögzítése |`azure vm capture [options] <resource_group> <name>` |
| Virtuális gép létrehozása felhasználói rendszerképből |`azure  vm create [options] –q <image-name> <resource-group> <name> <location> <os-type>` |
| Virtuális gép létrehozása specializált lemezből |`azue  vm create [options] –d <os-disk-vhd> <resource-group> <name> <location> <os-type>` |
| Adja hozzá a adatok lemez tooa méretű VM |`azure  vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]` |
| Adatlemez eltávolítása egy virtuális gépből |`azure  vm disk detach [options] <resource-group> <vm-name> <lun>` |
| Egy általános bővítmény tooa VM hozzáadása |`azure  vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>` |
| Adja hozzá a virtuális gép hozzáférés bővítmény tooa VM |`azure vm reset-access [options] <resource-group> <name>` |
| Adja hozzá a Docker bővítmény tooa méretű VM |`azure  vm docker create [options] <resource-group> <name> <location> <os-type>` |
| Virtuálisgép-bővítmény eltávolítása |`azure  vm extension set [options] –u <resource-group> <vm-name> <name> <publisher-name> <version>` |
| A virtuálisgép-erőforrások használatának megtekintése |`azure vm list-usage [options] <location>` |
| Minden elérhető virtuálisgép-méret megtekintése |`azure vm sizes [options]` |

## <a name="next-steps"></a>Következő lépések
* Alapszintű VM-felügyeleti túllépnek hello parancssori felület parancsait további példákért lásd [Using hello Azure CLI-t az Azure Resource Manager](../articles/virtual-machines/azure-cli-arm-commands.md).
