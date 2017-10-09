Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben. 

Amikor a saját kép tooAzure vagy hello platform képek is rendszerindítás egyike, miért egy virtuális gép nem indítható állapotba lekérdezi számos oka lehet. E szolgáltatások engedélyezése meg tooeasily diagnosztizálhatja és a virtuális gépek helyreállítás rendszerindítási hibák esetén.

A Linux virtuális gépek megtekintéséhez a konzol napló, a portál hello hello kimeneti:

![Azure Portal](./media/virtual-machines-common-boot-diagnostics/screenshot1.png)
 
Azonban a Windows és Linux rendszerű virtuális gépek Azure is lehetővé teszi, hogy egy képernyőfelvétel a hello VM hello hipervizorról toosee:

![Hiba](./media/virtual-machines-common-boot-diagnostics/screenshot2.png)

Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban. Vegye figyelembe, a pillanatképek és a kimeneti too10 perc tooappear a tárfiókban lévő is eltarthat.

## <a name="common-boot-errors"></a>Gyakori rendszerindítási hibák

- [0xC000000E](https://support.microsoft.com/help/4010129)
- [0xC000000F](https://support.microsoft.com/help/4010130)
- [0xC0000011](https://support.microsoft.com/help/4010134)
- [0xC0000034](https://support.microsoft.com/help/4010140)
- [0xC0000098](https://support.microsoft.com/help/4010137)
- [0xC00000BA](https://support.microsoft.com/help/4010136)
- [0xC000014C](https://support.microsoft.com/help/4010141)
- [0xC0000221](https://support.microsoft.com/help/4010132)
- [0xC0000225](https://support.microsoft.com/help/4010138)
- [0xC0000359](https://support.microsoft.com/help/4010135)
- [0xC0000605](https://support.microsoft.com/help/4010131)
- [Nem található operációs rendszer](https://support.microsoft.com/help/4010142)
- [Rendszerindítási hiba vagy INACCESSIBLE_BOOT_DEVICE](https://support.microsoft.com/help/4010143)

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Diagnosztika engedélyezése egy új virtuális gépen
1. Ha egy új virtuális gép létrehozása a betekintő portálon hello, válassza ki a hello **Azure Resource Manager** hello telepítési modell legördülő listából:
 
    ![Resource Manager](./media/virtual-machines-common-boot-diagnostics/screenshot3.jpg)

2. A diagnosztikai fájlok hello figyelés beállítás tooselect hello tárfiók tooplace helyének megadása
 
    ![Virtuális gép létrehozása](./media/virtual-machines-common-boot-diagnostics/screenshot4.jpg)

3. Ha telepíti az Azure Resource Manager sablon alapján, keresse meg a virtuálisgép-erőforrás tooyour, és a hozzáfűző hello diagnosztikai profil szakasz. Ne felejtse el toouse hello "2015-06-15" API version fejlécnek.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. hello diagnosztikai profiljának lehetővé teszi tooselect hello tárfiók, amelybe tooput ezek a naplók.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

a rendszerindítási diagnosztika engedélyezve van, itt a tárházban kivételét minta virtuális gép toodeploy.

## <a name="update-an-existing-virtual-machine"></a>Meglévő virtuális gép frissítése ##

hello portálon keresztül tooenable a rendszerindítási diagnosztika, is frissítheti egy meglévő virtuális gép hello portálon keresztül. Válassza ki a rendszerindítási diagnosztika lehetőséget, majd mentse hello. Hello VM tootake léptetéséhez indítsa újra.

![Létező virtuális gép frissítése](./media/virtual-machines-common-boot-diagnostics/screenshot5.png)

