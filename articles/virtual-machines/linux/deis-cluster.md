---
title: "a 3-csomópont Deis aaaDeploy fürt |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toocreate 3-csomópont Deis fürt Azure-ban Azure Resource Manager-sablonok"
services: virtual-machines-linux
documentationcenter: 
author: HaishiBai
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 5eb67eb7-95d4-461d-8eac-44925224ba5f
ms.service: virtual-machines-linux
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 06/24/2015
ms.author: hbai
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a4c0fb8cbb849264e64b433540157c9afecd184e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-configure-a-3-node-deis-cluster-in-azure"></a>Telepítse és konfigurálja a 3-csomópont Deis fürt az Azure-ban
Ez a cikk végigvezeti a kiépítés egy [Deis](http://deis.io/) fürt az Azure-on. Magában foglalja a hello szükséges tanúsítványok toodeploying létrehozásán és skálázásán minta lépéseket hello **nyissa meg** alkalmazás hello újonnan kiosztott fürtön.

hello alábbi ábrán látható hello telepített hello rendszer architektúrájával. A rendszergazda kezeli a hello fürt használt eszközök, mint Deis **deis** és **deisctl**. Egy Azure terheléselosztó, amely továbbítja az hello kapcsolatok tooone hello tag hello fürtön található csomópontok keresztül létesít kapcsolatokat. hello ügyfelek hozzáférési keresztül, valamint a hello terheléselosztó alkalmazások telepítését. Ebben az esetben hello terheléselosztó továbbítja hello forgalom tooa Deis útválasztó háló, amely további routs forgalom toocorresponding Docker tárolók hello fürtön található.

  ![Telepített Desis fürt architektúra ábrája](./media/deis-cluster/architecture-overview.png)

A sorrend toorun lépések hello keresztül szüksége lesz:

* Aktív Azure-előfizetés. Ha még nincs fiókja, a szabad eljáráshoz beszerezheti [Azure.com webhelyre](https://azure.microsoft.com/).
* Munkahelyi vagy iskolai azonosító toouse Azure erőforráscsoport-sablonok. Ha egy személyes fiók és jelentkezzen be Microsoft-azonosítóval rendelkezik, akkor túl[hozzon létre egy azonosító különbözik a személyes](../windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Vagy – attól függően, hogy az ügyfél operációs rendszer – hello [Azure PowerShell](/powershell/azureps-cmdlets-docs) vagy hello [Azure parancssori felület Mac, Linux és Windows](../../cli-install-nodejs.md).
* [OpenSSL](https://www.openssl.org/). OpenSSL használt toogenerate hello szükséges tanúsítványok.
* Például egy Git ügyfél [Git bash eszközt](https://git-scm.com/).
* tootest hello mintaalkalmazást, biztosítani kell a DNS-kiszolgáló. A DNS-kiszolgálók vagy helyettesítő A rekordok támogató szolgáltatások is használhatja.
* Egy számítógép toorun Deis ügyféleszközök elől. A helyi számítógép vagy virtuális gép is használhatja. Ezekkel az eszközökkel futtathatja szinte bármilyen a Linux-disztribúció, de hello következő utasítások használják Ubuntu.

## <a name="provision-hello-cluster"></a>Kiépítés hello fürt
Ebben a részben azt ismertetjük egy [Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md) sablon adattárból hello nyílt forráskódú [azure-gyors üzembe helyezés-sablonok](https://github.com/Azure/azure-quickstart-templates). Először a hello sablon lesz lemásolásához. Ezután létre fog hozni egy új SSH-kulcspárral hitelesítéshez. És ezt követően konfigurálja egy új, fürt azonosítója. És végezetül fogja használni a hello héjparancsfájlt vagy hello PowerShell parancsfájl tooprovision hello fürt.

1. Klónozás hello tárház: [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
   
        git clone https://github.com/Azure/azure-quickstart-templates
2. Nyissa meg toohello sablon mappája:
   
        cd azure-quickstart-templates\deis-cluster-coreos
3. Hozzon létre egy új SSH-kulcspárral ssh-keygen használata:
   
        ssh-keygen -t rsa -b 4096 -c "[your_email@domain.com]"
4. Létrehoz egy tanúsítványt, titkos kulcs fent hello használata:
   
        openssl req -x509 -days 365 -new -key [your private key file] -out [cert file toobe generated]
5. Nyissa meg túl[https://discovery.etcd.io/new](https://discovery.etcd.io/new) toogenerate új fürt tokent, az alábbihoz hasonló következőhöz:
   
        https://discovery.etcd.io/6a28e078895c5ec737174db2419bb2f3
   <br />
   Minden egyes CoreOS fürt toohave egyedi jogkivonatot adott szabad szolgáltatásból kell. Ellenőrizze a [CoreOS dokumentáció](https://coreos.com/docs/cluster-management/setup/cluster-discovery/) további részleteket.
6. Hello módosítása **felhő-config.yaml** tooreplace hello meglévő fájl **felderítési** token az új jogkivonatot hello:
   
        #cloud-config
        ---
        coreos:
          etcd:
            # generate a new token for each unique cluster from https://discovery.etcd.io/new
            # uncomment hello following line and replace it with your discovery URL
            discovery: https://discovery.etcd.io/3973057f670770a7628f917d58c2208a
        ...
7. Módosítsa a hello **azuredeploy-parameters.json** fájl: létrehozott a 4. lépésben egy szövegszerkesztőben megnyitott hello tanúsítványt. Másolja az összes szöveg közötti `----BEGIN CERTIFICATE-----` és `-----END CERTIFICATE-----` történő hello **sshKeyData** paraméter (lesz szüksége tooremove összes soremelés karakter).
8. Módosítsa a hello **newStorageAccountName** paraméter. Ez az hello tárfiók a virtuális gép operációs rendszere lemezek. A fióknév globálisan egyedi toobe rendelkezik.
9. Módosítsa a hello **publicDomainName** paraméter. Ez lesz hello load balancer nyilvános IP-cím társított hello DNS-név részét. hello végső FQDN lesz hello formátuma *[Ez a paraméter értékének]*. *[régió]* . cloudapp.azure.com. Például ha deishbai32 hello nevet ad meg, és hello erőforráscsoport üzembe helyezett toohello USA nyugati régiója régiót, majd a végső hello tooyour terheléselosztó FQDN lesz deishbai32.westus.cloudapp.azure.com.
10. Hello paraméter fájl mentéséhez. És az Azure PowerShell hello fürt azt követően:
    
        .\deploy-deis.ps1 -ResourceGroupName [resource group name] -ResourceGroupLocation "West US" -TemplateFile
        .\azuredeploy.json -ParametersFile .\azuredeploy-parameters.json -CloudInitFile .\cloud-config.yaml
    
    vagy az Azure parancssori felület:
    
        ./deploy-deis.sh -n "[resource group name]" -l "West US" -f ./azuredeploy.json -e ./azuredeploy-parameters.json
        -c ./cloud-config.yaml  
11. Hello erőforráscsoport üzembe helyezése után megtekintheti a hello csoportban lévő összes hello erőforrást a klasszikus Azure portálon. Ahogy az alábbi képernyőfelvételen látható hello hello erőforráscsoport három virtuális gépek virtuális hálózat, amely tartományhoz csatlakoztatott toohello azonos rendelkezésre állási csoportot. hello csoport is tartalmaz egy terheléselosztó, amely van egy társított nyilvános IP-címe.
    
    ![hello kiosztott erőforráscsoport a klasszikus Azure portálon](./media/deis-cluster/resource-group.png)

## <a name="install-hello-client"></a>Hello ügyfél telepítése
Szüksége **deisctl** toocontrol a fürt Deis. Bár deisctl automatikusan megtörténik a hello fürt összes csomópontjának, továbbra is egy célszerű toouse deisctl egy külön felügyeleti számítógépen. Továbbá mert minden csomópont csak privát IP-címek vannak beállítva, szüksége lesz toouse SSH tunneling hello terheléselosztó, amely van egy nyilvános IP-címe, tooconnect toohello csomópont gépek keresztül. hello az alábbiakban hello külön Ubuntu fizikai vagy virtuális gépen deisctl beállításának lépésein.

1. Telepítés deisctl:mkdir deis
   
        cd deis
        curl -sSL http://deis.io/deisctl/install.sh | sh -s 1.6.1
        sudo ln -fs $PWD/deisctl /usr/local/bin/deisctl
2. Adja hozzá a titkos kulcs toossh ügynök:
   
        eval `ssh-agent -s`
        ssh-add [path toohello private key file, see step 1 in hello previous section]
3. Deisctl konfigurálása:
   
        export DEISCTL_TUNNEL=[public ip of hello load balancer]:2223

hello sablon meghatározza a bejövő NAT-szabályok, amelyek kapcsolódnak 2223 tooinstance 1, 2224 tooinstance 2 és 2225 tooinstance 3. Hello deisctl eszközzel redundanciát biztosít. Ezek a szabályok a klasszikus Azure portálon ellenőrizheti:

![A hello terheléselosztó NAT-szabályok](./media/deis-cluster/nat-rules.png)

> [!NOTE]
> Hello sablon jelenleg csak 3-csomópontot tartalmazó fürt támogatja. Ez az Azure Resource Manager sablon NAT-szabály definíciót, amely nem támogatja a hurok szintaxis korlátozása miatt.
> 
> 

## <a name="install-and-start-hello-deis-platform"></a>Telepítse és indítsa el a hello Deis platform
Most deisctl tooinstall használja, és indítsa el a hello Deis platform:

    deisctl config platform set domain=[some domain]
    deisctl config platform set sshPrivateKey=[path toohello private key file]
    deisctl install platform
    deisctl start platform

> [!NOTE]
> Kezdő hello platform eltart egy ideig (akár 10 percet). Különösen hello jelentéskészítő szolgáltatás indítása a hosszú időt vehet igénybe. És egyes esetekben néhány záma toosucceed tart: Ha hello művelet toohang úgy tűnik, próbáljon meg `ctrl+c` toobreak végrehajtása hello parancsot, és próbálkozzon újra.
> 
> 

Használhat `deisctl list` tooverify, ha az összes szolgáltatás fut:

    deisctl list
    UNIT                            MACHINE                 LOAD    ACTIVE          SUB
    deis-builder.service            ebe3005e.../10.0.0.6    loaded  active          running
    deis-controller.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-database.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logger.service             9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-logspout.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-logspout.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-publisher.service          8d658d5a.../10.0.0.4    loaded  active          running
    deis-publisher.service          9c79bbdd.../10.0.0.5    loaded  active          running
    deis-publisher.service          ebe3005e.../10.0.0.6    loaded  active          running
    deis-registry@1.service         ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@1.service           ebe3005e.../10.0.0.6    loaded  active          running
    deis-router@2.service           8d658d5a.../10.0.0.4    loaded  active          running
    deis-router@3.service           9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-daemon.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-daemon.service       ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-gateway@1.service    9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-metadata.service     9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-metadata.service     ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-monitor.service      8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-monitor.service      9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-monitor.service      ebe3005e.../10.0.0.6    loaded  active          running
    deis-store-volume.service       8d658d5a.../10.0.0.4    loaded  active          running
    deis-store-volume.service       9c79bbdd.../10.0.0.5    loaded  active          running
    deis-store-volume.service       ebe3005e.../10.0.0.6    loaded  active          running

Gratulálunk! Most már futó Deis clsuter Azure! A következő helyezzünk üzembe egy minta Ugrás alkalmazás toosee hello fürt működés közben.

## <a name="deploy-and-scale-a-hello-world-application"></a>Üzembe helyezését és méretezését egy Hello World alkalmazásról
hello következő lépések bemutatják, hogyan toodeploy a "Hello World" Ugrás alkalmazás toohello fürt. a lépések hello [dokumentáció Deis](http://docs.deis.io/en/latest/using_deis/using-dockerfiles/#using-dockerfiles).

1. A hello útválasztási háló toowork megfelelően, szüksége lesz toohave helyettesítő A rekord a tartomány toohello nyilvános IP-cím hello terheléselosztó mutat. hello alábbi képernyőfelvételen látható egy minta tartományregisztrációs hello A rekord GoDaddy:
   
    ![Godaddy A rekord](./media/deis-cluster/go-daddy.png)
   
   <p />
2. Telepítés deis:
   
        mkdir deis
        cd deis
        curl -sSL http://deis.io/deis-cli/install.sh | sh
        ln -fs $PWD/deis /usr/local/bin/deis
3. Hozzon létre egy új SSH-kulcsot, és adja hozzá a nyilvános kulcs tooGitHub hello (természetesen is felhasználhatja a meglévő kulcsok). új SSH-kulcspárral, toocreate használja:
   
        cd ~/.ssh
        ssh-keygen (press [Enter]s toouse default file names and empty passcode)
4. Adja hozzá a id_rsa.pub, vagy az Ön által választott, tooGitHub hello a nyilvános kulcsot. Ehhez a hello hozzáadása SSH kulcs gombra kattintva az SSH-kulcsok konfigurációs képernyőjén:
   
   ![GitHub-kulcs](./media/deis-cluster/github-key.png)
   
   <p />
5. Új felhasználó regisztrálása:
   
        deis register http://deis.[your domain]
   <p />
6. Hello SSH-kulcs hozzáadása:
   
        deis keys:add [path tooyour SSH public key]
   <p />      
7. Hozzon létre egy alkalmazást.
   
        git clone https://github.com/deis/helloworld.git
        cd helloworld
        deis create
        git push deis master
   <p />
8.hello git push Docker képek toobe létrehozott és telepített, ami eltarthat néhány percig, akkor indul el. A felhasználói élmény, a alkalmanként lépés 10 (Pushing tooprivate lemezképtárba) lefagyhat. Ha ez történik, le is hello folyamat eltávolítása hello használó "deis alkalmazások: semmisítse meg – a <application name> ` tooremove hello application and try again. You can use `deis apps:list" toofind hello nevét az alkalmazás. Ha minden megfelelően működik, akkor kell kinéznie: hello hasonló parancs végrehajtásának hello végén
   
        -----> Launching...
               done, lambda-underdog:v2 deployed tooDeis
               http://lambda-underdog.artitrack.com
               toolearn more, use `deis help` or visit http://deis.io
        toossh://git@deis.artitrack.com:2222/lambda-underdog.git
         * [new branch]      master -> master
   <p />
9. Győződjön meg arról, hogy működik-e az alkalmazás hello:
   
        curl -S http://[your application name].[your domain]
   A következőnek kell megjelennie:
   
        Welcome tooDeis!
        See hello documentation at http://docs.deis.io/ for more information.
        (you can use geis apps:list tooget hello name of your application).
   <p />
10. Hello too3 alkalmazáspéldányok skálázni:
    
        deis scale cmd=3
    <p />
11. Másik lehetőségként használhatja az alkalmazás adatok tooexamine részletek deis. hello következő kimenetek tartoznak, amely az alkalmazás központi telepítése:
    
        deis info
        === lambda-underdog Application
        {
          "updated": "2015-05-22T06:14:10UTC",
          "uuid": "10c74ee7-b7ff-4786-967a-7e65af7eabc3",
          "created": "2015-05-22T06:07:55UTC",
          "url": "lambda-underdog.artitrack.com",
          "owner": "haishi",
          "id": "lambda-underdog",
          "structure": {
            "cmd": 3
          }
        }
    
        === lambda-underdog Processes
        --- cmd:
        cmd.1 up (v2)
        cmd.2 up (v2)
        cmd.3 up (v2)
    
        === lambda-underdog Domains
        No domains

## <a name="next-steps"></a>Következő lépések
Ez a cikk telefonon keresztül minden hello lépéseket tooprovision egy új Deis fürt Azure-ban Azure Resource Manager-sablonok. hello sablon tooling eszköz kapcsolatok, valamint a telepített alkalmazások terheléselosztását redundanciát támogatja. hello sablon megakadályozza, hogy tag csomópontján, amely értékes nyilvános IP-erőforrások menti, és lehetővé teszi a biztonságosabb környezet toohost nyilvános IP-címek használatával. toolearn több, tekintse meg a következő cikkek hello:

[Azure Resource Manager áttekintése][resource-group-overview]  
[Hogyan toouse hello Azure parancssori felület][azure-command-line-tools]  
[Using Azure PowerShell használata az Azure Resource Manager eszközzel][powershell-azure-resource-manager]  

[azure-command-line-tools]: ../../cli-install-nodejs.md
[resource-group-overview]: ../../azure-resource-manager/resource-group-overview.md
[powershell-azure-resource-manager]: ../../azure-resource-manager/powershell-azure-resource-manager.md
