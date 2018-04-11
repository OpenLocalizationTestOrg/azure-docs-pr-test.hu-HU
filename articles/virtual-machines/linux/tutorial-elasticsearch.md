---
title: Az ElasticSearch telepítése egy fejlesztési virtuális gépre Azure-ban
description: Oktatóanyag – Az Elastic Stack telepítése egy fejlesztési célú linuxos virtuális gépre az Azure-ban
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rloutlaw
manager: justhe
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: tutorial
ms.date: 10/11/2017
ms.author: routlaw
ms.openlocfilehash: 5b0b51504478cc0d501a89760ccd60808a69ccbd
ms.sourcegitcommit: 694e40a193980dea1e2f945471071f11030d5641
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/29/2018
---
# <a name="install-the-elastic-stack-on-an-azure-vm"></a><span data-ttu-id="b3cc7-103">Az Elastic Stack telepítése egy Azure-beli virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="b3cc7-103">Install the Elastic Stack on an Azure VM</span></span>

<span data-ttu-id="b3cc7-104">Ez a cikk ismerteti az [Elasticsearch](https://www.elastic.co/products/elasticsearch), a [Logstash](https://www.elastic.co/products/logstash) és a [Kibana](https://www.elastic.co/products/kibana) egy Ubuntu rendszerű virtuális gépre történő telepítését az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-104">This article walks you through how to deploy [Elasticsearch](https://www.elastic.co/products/elasticsearch), [Logstash](https://www.elastic.co/products/logstash), and [Kibana](https://www.elastic.co/products/kibana), on an Ubuntu VM in Azure.</span></span> <span data-ttu-id="b3cc7-105">Ha szeretné működés közben megtekinteni az Elastic Stacket, lehetősége van csatlakozni a Kibanához, és használhatja a mintául szolgáló naplózási adatokat.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-105">To see the Elastic Stack in action, you can optionally connect to Kibana  and work with some sample logging data.</span></span> 

<span data-ttu-id="b3cc7-106">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-106">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3cc7-107">Ubuntus virtuális gép létrehozása egy Azure-erőforráscsoportban</span><span class="sxs-lookup"><span data-stu-id="b3cc7-107">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b3cc7-108">Az Elasticsearch, a Logstash és a Kibana telepítése a virtuális gépre</span><span class="sxs-lookup"><span data-stu-id="b3cc7-108">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b3cc7-109">Mintaadatok elküldése az Elasticsearch számára a Logstash használatával</span><span class="sxs-lookup"><span data-stu-id="b3cc7-109">Send sample data to Elasticsearch with Logstash</span></span> 
> * <span data-ttu-id="b3cc7-110">Portok megnyitása és adatok használata a Kibana-konzolon</span><span class="sxs-lookup"><span data-stu-id="b3cc7-110">Open ports and work with data in the Kibana console</span></span>


 <span data-ttu-id="b3cc7-111">Az üzemelő példány alkalmas az Elastic Stackkel végzett alapszintű fejlesztésre.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-111">This deployment is suitable for basic development with the Elastic Stack.</span></span> <span data-ttu-id="b3cc7-112">További információt az Elastic Stackről – beleértve az éles környezetre vonatkozó javaslatokat – az [Elastic dokumentációjában](https://www.elastic.co/guide/index.html) és az [Azure Architecture Centerben](/azure/architecture/elasticsearch/) talál.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-112">For more on the Elastic Stack, including recommendations for a production environment, see the [Elastic documentation](https://www.elastic.co/guide/index.html) and the [Azure Architecture Center](/azure/architecture/elasticsearch/).</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="b3cc7-113">Ha a parancssori felület helyi telepítését és használatát választja, akkor ehhez az oktatóanyaghoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-113">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="b3cc7-114">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="b3cc7-115">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="b3cc7-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-a-resource-group"></a><span data-ttu-id="b3cc7-116">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="b3cc7-116">Create a resource group</span></span>

<span data-ttu-id="b3cc7-117">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-117">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="b3cc7-118">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-118">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="b3cc7-119">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-119">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-virtual-machine"></a><span data-ttu-id="b3cc7-120">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3cc7-120">Create a virtual machine</span></span>

<span data-ttu-id="b3cc7-121">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-121">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="b3cc7-122">Az alábbi példa egy *myVM* nevű virtuális gépet és SSH-kulcsokat hoz létre, ha azok még nem léteznek a kulcsok alapméretezett helyén.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-122">The following example creates a VM named *myVM* and creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="b3cc7-123">Ha konkrét kulcsokat szeretné használni, használja az `--ssh-key-value` beállítást.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-123">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="b3cc7-124">A virtuális gép létrehozása után az Azure CLI az alábbi példához hasonló információkat jelenít meg.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-124">When the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="b3cc7-125">Jegyezze fel a `publicIpAddress` értékét.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-125">Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="b3cc7-126">Ez a cím használható a virtuális gép eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-126">This address is used to access the VM.</span></span>

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/<subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "40.68.254.142",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="ssh-into-your-vm"></a><span data-ttu-id="b3cc7-127">Bejelentkezés a virtuális gépre SSH-val</span><span class="sxs-lookup"><span data-stu-id="b3cc7-127">SSH into your VM</span></span>

<span data-ttu-id="b3cc7-128">Ha még nem ismeri a virtuális gépéhez tartozó nyilvános IP-címet, futtassa az [az network public-ip list](/cli/azure/network/public-ip#list) parancsot:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-128">If you don't already know the public IP address of your VM, run the [az network public-ip list](/cli/azure/network/public-ip#list) command:</span></span>

```azurecli-interactive
az network public-ip list --resource-group myResourceGroup --query [].ipAddress
```

<span data-ttu-id="b3cc7-129">Használja az alábbi parancsot egy SSH-munkamenet létrehozásához a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-129">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="b3cc7-130">Helyettesítse be a virtuális gépe tényleges nyilvános IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-130">Substitute the correct public IP address of your virtual machine.</span></span> <span data-ttu-id="b3cc7-131">Ebben a példában az IP-cím a következő: *40.68.254.142*.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-131">In this example, the IP address is *40.68.254.142*.</span></span>

```bash
ssh azureuser@40.68.254.142
```

## <a name="install-the-elastic-stack"></a><span data-ttu-id="b3cc7-132">Az Elastic Stack telepítése</span><span class="sxs-lookup"><span data-stu-id="b3cc7-132">Install the Elastic Stack</span></span>

<span data-ttu-id="b3cc7-133">Importálja az Elasticsearch-aláírókulcsot, és frissítse az APT-források listáját, hogy szerepeljen benne az Elastic csomagtára:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-133">Import the Elasticsearch signing key and update your APT sources list to include the Elastic package repository:</span></span>

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/5.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-5.x.list
```

<span data-ttu-id="b3cc7-134">Telepítse a Java Virtualt a virtuális gépen, és konfigurálja a JAVA_HOME változót – ez az Elastic Stack-összetevők futtatásához szükséges.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-134">Install the Java Virtual on the VM and configure the JAVA_HOME variable-this is necessary for the Elastic Stack components to run.</span></span>

```bash
sudo apt update && sudo apt install openjdk-8-jre-headless
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```

<span data-ttu-id="b3cc7-135">Futtassa az alábbi parancsokat az ubuntus csomagforrások frissítéséhez, és az Elasticsearch, a Kibana és a Logstash telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-135">Run the following commands to update Ubuntu package sources and install Elasticsearch, Kibana, and Logstash.</span></span>

```bash
sudo apt update && sudo apt install elasticsearch kibana logstash   
```

> [!NOTE]
> <span data-ttu-id="b3cc7-136">A részletes telepítési utasításokat, beleértve a mappastruktúrát és a kezdeti konfigurációt az [Elastic dokumentációjában](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html) találja.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-136">Detailed installation instructions, including directory layouts and initial configuration, are maintained in [Elastic's documentation](https://www.elastic.co/guide/en/elastic-stack/current/installing-elastic-stack.html)</span></span>

## <a name="start-elasticsearch"></a><span data-ttu-id="b3cc7-137">Az Elasticsearch indítása</span><span class="sxs-lookup"><span data-stu-id="b3cc7-137">Start Elasticsearch</span></span> 

<span data-ttu-id="b3cc7-138">Az Elasticsearch a virtuális gépen való indításához használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-138">Start Elasticsearch on your VM with the following command:</span></span>

```bash
sudo systemctl start elasticsearch.service
```

<span data-ttu-id="b3cc7-139">Ez a parancs nem hoz létre kimenetet, ezért ellenőrizze ezzel a `curl`-paranccsal, hogy az Elasticsearch fut-e a virtuális gépen:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-139">This command produces no output, so verify that Elasticsearch is running on the VM with this `curl` command:</span></span>

```bash
curl -XGET 'localhost:9200/'
```

<span data-ttu-id="b3cc7-140">Ha az Elasticsearch fut, az alábbihoz hasonló kimenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-140">If Elasticsearch is running, you see output like the following:</span></span>

```json
{
  "name" : "w6Z4NwR",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "SDzCajBoSK2EkXmHvJVaDQ",
  "version" : {
    "number" : "5.6.3",
    "build_hash" : "1a2f265",
    "build_date" : "2017-10-06T20:33:39.012Z",
    "build_snapshot" : false,
    "lucene_version" : "6.6.1"
  },
  "tagline" : "You Know, for Search"
}
```

## <a name="start-logstash-and-add-data-to-elasticsearch"></a><span data-ttu-id="b3cc7-141">A Logstash indítása és adatok hozzáadása az Elasticsearchhöz</span><span class="sxs-lookup"><span data-stu-id="b3cc7-141">Start Logstash and add data to Elasticsearch</span></span>

<span data-ttu-id="b3cc7-142">Indítsa el a Logstasht a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-142">Start Logstash with the following command:</span></span>

```bash 
sudo systemctl start logstash.service
```

<span data-ttu-id="b3cc7-143">Tesztelje a Logstasht interaktív módban, hogy meggyőződhessen a helyes működéséről:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-143">Test Logstash in interactive mode to make sure it's working correctly:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -e 'input { stdin { } } output { stdout {} }'
```

<span data-ttu-id="b3cc7-144">Ez egy alapszintű logstash-[folyamat](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html), amely a standard bemenetet egy standard kimenetbe adja vissza.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-144">This is a basic logstash [pipeline](https://www.elastic.co/guide/en/logstash/5.6/pipeline.html) that echoes standard input to standard output.</span></span> 

```output
The stdin plugin is now waiting for input:
hello azure
2017-10-11T20:01:08.904Z myVM hello azure
```

<span data-ttu-id="b3cc7-145">Állítsa be a Logstasht úgy, hogy továbbítsa a kernelüzeneteket erről a virtuális gépről az Elasticsearchre.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-145">Set up Logstash to forward the kernel messages from this VM to Elasticsearch.</span></span> <span data-ttu-id="b3cc7-146">Hozzon létre egy új fájlt egy üres, `vm-syslog-logstash.conf` nevű könyvtárban, és illessze be az alábbi Logstash-konfigurációt:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-146">Create a new file in an empty directory called `vm-syslog-logstash.conf` and paste in the following Logstash configuration:</span></span>

```Logstash
input {
    stdin {
        type => "stdin-type"
    }

    file {
        type => "syslog"
        path => [ "/var/log/*.log", "/var/log/*/*.log", "/var/log/messages", "/var/log/syslog" ]
        start_position => "beginning"
    }
}

output {

    stdout {
        codec => rubydebug
    }
    elasticsearch {
        hosts  => "localhost:9200"
    }
}
```

<span data-ttu-id="b3cc7-147">Tesztelje ezt a konfigurációt, és küldje a rendszernaplóadatokat az Elasticsearchbe:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-147">Test this configuration and send the syslog data to Elasticsearch:</span></span>

```bash
sudo /usr/share/logstash/bin/logstash -f vm-syslog-logstash.conf
```

<span data-ttu-id="b3cc7-148">A rendszernapló-bejegyzések a terminálon úgy jelennek meg, ahogy a rendszer elküldte őket az Elasticsearchbe.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-148">You see the syslog entries in your terminal echoed as they are sent to Elasticsearch.</span></span> <span data-ttu-id="b3cc7-149">Az adatok elküldése után lépjen ki a Logstashből a `CTRL+C` használatával.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-149">Use `CTRL+C` to exit out of Logstash once you've sent some data.</span></span>

## <a name="start-kibana-and-visualize-the-data-in-elasticsearch"></a><span data-ttu-id="b3cc7-150">A Kibana indítása és az adatok megjelenítése az Elasticsearchben</span><span class="sxs-lookup"><span data-stu-id="b3cc7-150">Start Kibana and visualize the data in Elasticsearch</span></span>

<span data-ttu-id="b3cc7-151">Szerkessze az `/etc/kibana/kibana.yml` fájlt, és módosítsa a Kibana által figyelt IP-címet, hogy hozzá tudjon férni a böngészőből.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-151">Edit `/etc/kibana/kibana.yml` and change the IP address Kibana listens on so you can access it from your web browser.</span></span>

```bash
server.host:"0.0.0.0"
```

<span data-ttu-id="b3cc7-152">Indítsa el a Kibanát a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-152">Start Kibana with the following command:</span></span>

```bash
sudo systemctl start kibana.service
```

<span data-ttu-id="b3cc7-153">Nyissa meg az 5601-es portot az Azure CLI-ről, hogy távolról is hozzá lehessen férni a Kibana-konzolhoz:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-153">Open port 5601 from the Azure CLI to allow remote access to the Kibana console:</span></span>

```azurecli-interactive
az vm open-port --port 5601 --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="b3cc7-154">Nyissa meg a Kibana-konzolt, és a **Create** (Létrehozás) elemet választva hozzon létre egy alapértelmezett indexet az Elasticsearchbe korábban elküldött rendszernaplóadatok alapján.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-154">Open up the Kibana console and select **Create** to generate a default index based on the syslog data you sent to Elasticsearch earlier.</span></span> 

![Rendszernapló-események tallózása a Kibanában](media/elasticsearch-install/kibana-index.png)

<span data-ttu-id="b3cc7-156">A Kibana-konzolon a **Discover** (Felderítés) elemet választva kereshet és tallózhat a rendszernapló-események között, és szűrheti is őket.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-156">Select **Discover** on the Kibana console to search, browse, and filter through the syslog events.</span></span>

![Rendszernapló-események tallózása a Kibanában](media/elasticsearch-install/kibana-search-filter.png)

## <a name="next-steps"></a><span data-ttu-id="b3cc7-158">További lépések</span><span class="sxs-lookup"><span data-stu-id="b3cc7-158">Next steps</span></span>

<span data-ttu-id="b3cc7-159">Ebben az oktatóanyagban telepítette az Elastic Stacket egy fejlesztési célú virtuális gépre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b3cc7-159">In this tutorial, you deployed the Elastic Stack into a development VM in Azure.</span></span> <span data-ttu-id="b3cc7-160">Megismerte, hogyan végezheti el az alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="b3cc7-160">You learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b3cc7-161">Ubuntus virtuális gép létrehozása egy Azure-erőforráscsoportban</span><span class="sxs-lookup"><span data-stu-id="b3cc7-161">Create an Ubuntu VM in an Azure resource group</span></span>
> * <span data-ttu-id="b3cc7-162">Az Elasticsearch, a Logstash és a Kibana telepítése a virtuális gépre</span><span class="sxs-lookup"><span data-stu-id="b3cc7-162">Install Elasticsearch, Logstash, and Kibana on the VM</span></span>
> * <span data-ttu-id="b3cc7-163">Mintaadatok elküldése az Elasticsearch számára a Logstashből</span><span class="sxs-lookup"><span data-stu-id="b3cc7-163">Send sample data to Elasticsearch from Logstash</span></span> 
> * <span data-ttu-id="b3cc7-164">Portok megnyitása és adatok használata a Kibana-konzolon</span><span class="sxs-lookup"><span data-stu-id="b3cc7-164">Open ports and work with data in the Kibana console</span></span>