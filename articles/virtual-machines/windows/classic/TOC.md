# Áttekintés
## [Információ a virtuális gépekről](../../virtual-machines-windows-about.md)
## [Lemezek és virtuális merevlemezek](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
## [Virtuális hálózatok](../../../virtual-network/virtual-networks-overview.md)
## [Gyakori kérdések](faq.md)
## [Virtuális gépek, webhelyek és felhőszolgáltatások összehasonlítása](../../../app-service-web/choose-web-site-cloud-service-vm.md)
## [Tárolók](../../virtual-machines-windows-containers.md)

# Bevezetés
## [Hello portál virtuális gép létrehozása](tutorial.md)
## [Jelentkezzen be a virtuális gép tooa](connect-logon.md)
## [Az Azure PowerShell telepítése](/powershell/azure/overview)
## [Az Azure parancssori felület telepítése](../../../cli-install-nodejs.md)

# Útmutató

## A Storage használata
### [Adatlemez csatolása](attach-disk.md)
### [Adatlemez leválasztása](detach-disk.md)
### [A „D:” meghajtó használata adatlemezként](../../virtual-machines-windows-change-drive-letter.md)

## Network (Hálózat)
### [Hogyan tooset végpontok mentése](setup-endpoints.md)
### [Virtuális gépek csatlakoztatása virtuális hálózathoz vagy felhőszolgáltatáshoz](connect-vms.md)
### [A hagyományos Vneteket tooResource Manager Vnetek csatlakozás](../../../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
### [Terheléselosztó létrehozása](../../../load-balancer/load-balancer-get-started-internet-classic-portal.md)
### [NSG-k kezelése az Azure PowerShell-lel](../../../virtual-network/virtual-networks-create-nsg-classic-ps.md)

## Üzembe helyezés
### [Egyéni virtuális gép létrehozása](createportal.md)
### [Virtuális gép létrehozása és konfigurálása az Azure PowerShell-lel](create-powershell.md)
### [Windows rendszerű virtuális gép rögzítése](capture-image.md)
### [Virtuális merevlemez létrehozása és feltöltése a PowerShell-lel](createupload-vhd.md)
### [Az Azure-beli virtuális gépek üzembe helyezésének automatizálása a Chef szolgáltatással](../../virtual-machines-windows-chef-automation.md)
### [Virtuális gépek létrehozása és kezelése a Visual Studióban](manage-visual-studio.md)
### [Webalkalmazáshoz tartozó virtuális gép létrehozása a Visual Studióban](web-app-visual-studio.md)
### [Számításigényes feladat futtatása Java környezetben](java-run-compute-intensive-task.md)
### [Django Hello World webalkalmazás](python-django-web-app.md)

## Konfigurálás
### [Jelszó vagy hello távoli asztal szolgáltatás alaphelyzetbe állítása](../../virtual-machines-windows-reset-rdp.md)
### [A Symantec Endpoint Protection telepítése és konfigurálása](install-symantec.md)
### [A Trend Micro Deep Security telepítése és konfigurálása szolgáltatásként](install-trend.md)
### [Rendelkezésre állási csoport konfigurálása](configure-availability.md)
### [A Windows virtuális gépek hello klasszikus üzembe helyezési modellel létrehozott átméretezése](resize-vm.md)
### [Karbantartás](planned-maintenance-schedule.md)

## Kezelés
### [Klasszikus tooResource Manager áttelepítése](../../virtual-machines-windows-migration-classic-resource-manager-deep-dive.md)
### [Virtuális gépek kezelése az Azure PowerShell-lel](manage-psh.md)
### [Virtuálisgép-ügynök hello és bővítmények](agents-and-extensions.md)
### [Virtuálisgép-bővítmények kezelése](manage-extensions.md)
### [Egyéni parancsfájl-bővítmény virtuális gépek számára](extensions-customscript.md)
### [Egyéni adatok betöltése Azure virtuális gépbe](inject-custom-data.md)

## Felkészülés
### [További információk a rendszerképekről](about-images.md)
### [A virtuális gépek mérete](../../virtual-machines-windows-sizes.md)
### [Az Azure-beli virtuális gépek tervezett karbantartása](../../virtual-machines-windows-planned-maintenance.md)
### [Az Azure infrastruktúra-szolgáltatások megvalósítási irányelvei](../../virtual-machines-windows-infrastructure-subscription-accounts-guidelines.md)

## Számítási feladatok kezelése
### [Nagy teljesítményű feldolgozás (HPC)](../../virtual-machines-windows-hpcpack-cluster-options.md)
#### [Erőforrások automatikus méretezése](hpcpack-cluster-node-autogrowshrink.md)
#### [Számítási csomópontok kezelése](hpcpack-cluster-node-manage.md)
#### [Fürt létrehozása](hpcpack-cluster-powershell-script.md)
#### [A fürt toorun MPI alkalmazások beállítása](hpcpack-rdma-cluster.md)
#### [Excel és SOA típusú számítási feladatok futtatása](../../virtual-machines-windows-excel-cluster-hpcpack.md)
#### [Hozzon létre egy Piactéri lemezképhez hello átjárócsomópont](../../virtual-machines-windows-hpcpack-cluster-headnode.md)
#### [A helyszíni tooAzure feladatok elküldéséhez](../../virtual-machines-windows-hpcpack-cluster-submit-jobs.md)
### [MongoDB](install-mongodb.md)
### [MySQL](mysql-2008r2.md)
### [Oracle](../../workloads/oracle/oracle-considerations.md)
### [SAP](sap-get-started.md)
### [SQL Server](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
### [Tomcat](java-run-tomcat-app-server.md)

## Hibaelhárítás
### [Távoli asztali kapcsolatok](../../virtual-machines-windows-troubleshoot-rdp-connection.md)
####[Távoli asztali kapcsolatok problémáinak részletes hibaelhárítási lépései](../../virtual-machines-windows-detailed-troubleshoot-rdp.md)
### [Access tooan alkalmazás](../../virtual-machines-windows-troubleshoot-app-connection.md)
### [Klasszikus üzembe helyezési hibák új virtuális gép létrehozásakor](troubleshoot-deployment-new-vm.md)
### [Klasszikus üzembe helyezési hibák meglévő virtuális gép újraindításakor vagy átméretezésekor](virtual-machines-windows-classic-restart-resize-error-troubleshooting.md)
### [RDP-jelszó visszaállítása](reset-rdp.md)
### [Csatlakoztassa a virtuális gép virtuális merevlemez tootroubleshooting](troubleshoot-recovery-disks-portal.md)

# Referencia
## [PowerShell](/powershell/azure/overview)
## [Azure CLI](/cli/azure/vm)
## [Java](/java/api)
## [.NET](/dotnet/api/microsoft.azure.management.compute)
## [Resource Manager-sablonok készítése](../../../resource-group-authoring-templates.md)
## [Közösségi sablonok](https://azure.microsoft.com/documentation/templates)
## [Számítási REST](/rest/api/compute)
## [Hálózati REST](/rest/api)
## [Tárolási REST](/rest/api/storageservices)

# Erőforrások
## [Azure-ütemterv](https://azure.microsoft.com/roadmap/?category=compute)
## [Díjszabás](https://azure.microsoft.com/pricing/details/virtual-machines/#Windows)
## [Díjkalkulátor](https://azure.microsoft.com/pricing/calculator/)
## [Régiónkénti rendelkezésre állás](https://azure.microsoft.com/regions/services/)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-virtual-machine)
## [Videók](https://azure.microsoft.com/documentation/videos/index/?services=virtual-machines)
