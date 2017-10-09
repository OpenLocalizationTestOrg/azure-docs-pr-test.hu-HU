


Ez a témakör ismerteti, hogyan:

* Szúrjon be egy Azure virtuális gép (VM) adatokat, amikor folyamatban van.
* Beolvasni a Windows és Linux.
* Egyes rendszerek toodetect elérhető speciális eszközöket használja, és automatikusan kezelni az egyéni adatokat.

> [!NOTE]
> Ez a cikk ismerteti, hogyan egyéni adatok beszúrhatja egy virtuális Géphez létre hello Azure szolgáltatásfelügyeleti API használatával. Hogyan toouse hello Azure erőforrás-kezelési API, lásd: toosee [hello példa sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-customdata).
> 
> 

## <a name="injecting-custom-data-into-your-azure-virtual-machine"></a>Egyéni adatok beszúrva közzététele az Azure virtuális gépen
Ez a funkció jelenleg csak a hello támogatott [Azure parancssori felület](https://github.com/Azure/azure-xplat-cli). Itt létrehozhatunk egy `custom-data.txt` fájlt, amely tartalmazza az adatokat, majd szúrjon, amely a virtuális gép toohello kiépítése során. Bár használhatja hello beállításokat hello `azure vm create` parancs hello következő egy nagyon egyszerű módszert mutatja be:

```
    azure vm create <vmname> <vmimage> <username> <password> \  
    --location "West US" --ssh 22 \  
    --custom-data ./custom-data.txt  
```


## <a name="using-custom-data-in-hello-virtual-machine"></a>Egyéni adatok használatának hello virtuális gép
* Ha az Azure virtuális gép egy Windows-alapú virtuális Gépet, akkor egyéni adatfájl hello túl mentett`%SYSTEMDRIVE%\AzureData\CustomData.bin`. Bár a helyi számítógép toohello hello base64-kódolású tootransfer volt új virtuális Gépet, akkor a rendszer automatikusan dekódolni, és megnyitható vagy azonnal.
  
  > [!NOTE]
  > Ha hello fájl létezik, a rendszer felülírja. hello biztonsági hello címtár értéke túl**System: Full Control** és **rendszergazdák: Full Control**.
  > 
  > 
* Esetén az Azure virtuális gép a Linux-alapú virtuális gépek hello egyéni adatok fájl is található, a következő hello attól függően, hogy a distro helyezi. hello történhet base64-kódolású, ezért először esetleg toodecode hello adatok:
  
  * `/var/lib/waagent/ovf-env.xml`
  * `/var/lib/waagent/CustomData`
  * `/var/lib/cloud/instance/user-data.txt` 

## <a name="cloud-init-on-azure"></a>Azure cloud inicializálás
Ha az Azure virtuális gép Ubuntu vagy CoreOS lemezképből, majd használhatja CustomData toosend egy felhő-config toocloud inicializálás. Vagy ha az egyéni fájlját egy parancsfájlt, majd felhő inicializálás egyszerűen végrehajtható.

### <a name="ubuntu-cloud-images"></a>Ubuntu felhő lemezképek
A legtöbb Azure Linux-lemezképekben volna szerkesztése "/ etc/waagent.conf" tooconfigure hello ideiglenes lemezes és a lapozófájl-kapacitás erőforrásfájl. Lásd: [Azure Linux ügynök felhasználói útmutató](../articles/virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) további információt.

Azonban a hello Ubuntu felhő képek, és kell használnia felhő inicializálás tooconfigure hello erőforrás (Ez azt jelenti, hogy hello "elmúló") lemez swap partíció. Hello lap következő hello Ubuntu wiki további részletekért lásd: [AzureSwapPartitions](https://wiki.ubuntu.com/AzureSwapPartitions).

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps-using-cloud-init"></a>További lépések: Felhő inicializálás használatával
További információkért lásd: hello [Ubuntu felhő inicializálás dokumentációja](https://help.ubuntu.com/community/CloudInit).

<!--Link references-->
[Adja hozzá a szerepkör Service Management REST API-referencia](http://msdn.microsoft.com/library/azure/jj157186.aspx)

[Azure parancssori felület](https://github.com/Azure/azure-xplat-cli)

