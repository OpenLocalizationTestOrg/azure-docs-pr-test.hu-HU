---
title: "Az Azure Event Hubs rögzítése forgatókönyv |} Microsoft Docs"
description: "Az Event Hubs rögzítése funkció használata bemutatása az Azure Python SDK-t használó mintaalkalmazás."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/28/2017
ms.author: darosa;sethm
ms.openlocfilehash: a764a116755c20f60e92e553bd7c896425272b85
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="c2936-103">Event Hubs rögzítése forgatókönyv: Python</span><span class="sxs-lookup"><span data-stu-id="c2936-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="c2936-104">Event Hubs rögzítése rendszerben, amely lehetővé teszi, hogy automatikusan a streamelési adatok az eseményközpont számára egy Azure Blob storage-fiókot az Ön által választott az Event hubs szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c2936-104">Event Hubs Capture is a feature of Event Hubs that enables you to automatically deliver the streaming data in your event hub to an Azure Blob storage account of your choice.</span></span> <span data-ttu-id="c2936-105">Ez a funkció megkönnyíti, hogy a valós idejű streamelési adatok kötegfeldolgozási végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="c2936-105">This capability makes it easy to perform batch processing on real-time streaming data.</span></span> <span data-ttu-id="c2936-106">A cikkből megtudhatja, hogyan használható az Event Hubs rögzítése a Python.</span><span class="sxs-lookup"><span data-stu-id="c2936-106">This article describes how to use Event Hubs Capture with Python.</span></span> <span data-ttu-id="c2936-107">További információ az Event Hubs rögzítése: a [a cikk áttekintése](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c2936-107">For more information about Event Hubs Capture, see the [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="c2936-108">Ebben a példában a [Azure Python SDK](https://azure.microsoft.com/develop/python/) a rögzítése funkció bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="c2936-108">This sample uses the [Azure Python SDK](https://azure.microsoft.com/develop/python/) to demonstrate the Capture feature.</span></span> <span data-ttu-id="c2936-109">A sender.py program elküldi szimulált környezeti telemetriai Event Hubs JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="c2936-109">The sender.py program sends simulated environmental telemetry to Event Hubs in JSON format.</span></span> <span data-ttu-id="c2936-110">Az event hubs blob-tároló kötegekben telepítse ezeket az adatokat írni az rögzítése funkció használatára van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="c2936-110">The event hub is configured to use the Capture feature to write this data to blob storage in batches.</span></span> <span data-ttu-id="c2936-111">A capturereader.py app majd beolvassa a blobok eszközönként egy hozzáfűző fájlt hoz létre, majd írja az adatokat .csv fájlokba.</span><span class="sxs-lookup"><span data-stu-id="c2936-111">The capturereader.py app then reads these blobs and creates an append file per device, then writes the data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="c2936-112">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="c2936-112">What will be accomplished</span></span>

1. <span data-ttu-id="c2936-113">Hozzon létre egy Azure Blob Storage-fiókot és egy blob tároló belül, az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c2936-113">Create an Azure Blob Storage account and a blob container within it, using the Azure portal.</span></span>
2. <span data-ttu-id="c2936-114">Hozzon létre egy Eseményközpontba névtér, az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c2936-114">Create an Event Hub namespace, using the Azure portal.</span></span>
3. <span data-ttu-id="c2936-115">Létrehoz egy eseményközpontot a rögzítési szolgáltatás engedélyezve van, az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="c2936-115">Create an event hub with the Capture feature enabled, using the Azure portal.</span></span>
4. <span data-ttu-id="c2936-116">Adatok küldése az event hubs egy Python-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c2936-116">Send data to the event hub with a Python script.</span></span>
5. <span data-ttu-id="c2936-117">Olvassa be a fájlok rögzítését, illetve dolgozza fel őket másik Python-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="c2936-117">Read the files from the capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c2936-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c2936-118">Prerequisites</span></span>

- <span data-ttu-id="c2936-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="c2936-119">Python 2.7.x</span></span>
- <span data-ttu-id="c2936-120">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="c2936-120">An Azure subscription</span></span>
- <span data-ttu-id="c2936-121">Az aktív [Event Hubs névtér és az event hub.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="c2936-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="c2936-122">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2936-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="c2936-123">Jelentkezzen be az [Azure Portalra][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="c2936-123">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="c2936-124">A portál bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **tárolási**, és kattintson a **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="c2936-124">In the left navigation pane of the portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="c2936-125">Töltse ki a mezőket a storage-fiók panelen, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c2936-125">Complete the fields in the storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="c2936-126">Miután meggyőződött arról a **központi telepítések sikeres** üzenet, kattintson a nevére, az új tárfiók és a a **Essentials** panelen kattintson a **Blobok**.</span><span class="sxs-lookup"><span data-stu-id="c2936-126">After you see the **Deployments Succeeded** message, click the name of the new storage account and in the **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="c2936-127">Ha a **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** tetején.</span><span class="sxs-lookup"><span data-stu-id="c2936-127">When the **Blob service** blade opens, click **+ Container** at the top.</span></span> <span data-ttu-id="c2936-128">A tároló neve **rögzítése**, majd zárja be a **Blob szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="c2936-128">Name the container **capture**, then close the **Blob service** blade.</span></span>
5. <span data-ttu-id="c2936-129">Kattintson a **hívóbetűk** a bal oldali panelen, és másolja a tárfiók nevét és értékét a **key1**.</span><span class="sxs-lookup"><span data-stu-id="c2936-129">Click **Access keys** in the left blade and copy the name of the storage account and the value of **key1**.</span></span> <span data-ttu-id="c2936-130">Ezeket az értékeket a Jegyzettömb vagy más ideiglenes helyre mentse.</span><span class="sxs-lookup"><span data-stu-id="c2936-130">Save these values to Notepad or some other temporary location.</span></span>

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a><span data-ttu-id="c2936-131">A Python-parancsfájl események küldése az eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2936-131">Create a Python script to send events to your event hub</span></span>
1. <span data-ttu-id="c2936-132">Nyissa meg a kedvenc Python-szerkesztőt, például a [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="c2936-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="c2936-133">Hozzon létre nevű parancsfájl **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="c2936-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="c2936-134">Ez a parancsfájl 200 események küldése az eseményközpont.</span><span class="sxs-lookup"><span data-stu-id="c2936-134">This script sends 200 events to your event hub.</span></span> <span data-ttu-id="c2936-135">Elküldött JSON egyszerű környezeti értékek.</span><span class="sxs-lookup"><span data-stu-id="c2936-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="c2936-136">Az alábbi kód beillesztése sender.py:</span><span class="sxs-lookup"><span data-stu-id="c2936-136">Paste the following code into sender.py:</span></span>
   
  ```python
  import uuid
  import datetime
  import random
  import json
  from azure.servicebus import ServiceBusService
   
  sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
  devices = []
  for x in range(0, 10):
      devices.append(str(uuid.uuid4()))
   
  for y in range(0,20):
      for dev in devices:
          reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
          s = json.dumps(reading)
          sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
      print y
  ```
4. <span data-ttu-id="c2936-137">A névtér nevét, a kulcs értéke és az eseményközpont neveként az Event Hubs névtér létrehozása után beszerzett az előzőekben látható kód frissítése.</span><span class="sxs-lookup"><span data-stu-id="c2936-137">Update the preceding code to use your namespace name, key value, and event hub name that you obtained when you created the Event Hubs namespace.</span></span>

## <a name="create-a-python-script-to-read-your-capture-files"></a><span data-ttu-id="c2936-138">A rögzítési fájljainak olvasása a Python-parancsfájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="c2936-138">Create a Python script to read your Capture files</span></span>

1. <span data-ttu-id="c2936-139">Töltse ki a panelen, majd kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="c2936-139">Fill out the blade and click **Create**.</span></span>
2. <span data-ttu-id="c2936-140">Hozzon létre nevű parancsfájl **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="c2936-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="c2936-141">Ez a parancsfájl beolvassa a rögzített fájlok, és létrehoz egy fájlt eszközönként csak az eszköznek adatokat írni.</span><span class="sxs-lookup"><span data-stu-id="c2936-141">This script reads the captured files and creates a file per device to write the data only for that device.</span></span>
3. <span data-ttu-id="c2936-142">Az alábbi kód beillesztése capturereader.py:</span><span class="sxs-lookup"><span data-stu-id="c2936-142">Paste the following code into capturereader.py:</span></span>
   
  ```python
  import os
  import string
  import json
  import avro.schema
  from avro.datafile import DataFileReader, DataFileWriter
  from avro.io import DatumReader, DatumWriter
  from azure.storage.blob import BlockBlobService
   
  def processBlob(filename):
      reader = DataFileReader(open(filename, 'rb'), DatumReader())
      dict = {}
      for reading in reader:
          parsed_json = json.loads(reading["Body"])
          if not 'id' in parsed_json:
              return
          if not dict.has_key(parsed_json['id']):
              list = []
              dict[parsed_json['id']] = list
          else:
              list = dict[parsed_json['id']]
              list.append(parsed_json)
      reader.close()
      for device in dict.keys():
          deviceFile = open(device + '.csv', "a")
          for r in dict[device]:
              deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
  def startProcessing(accountName, key, container):
      print 'Processor started using path: ' + os.getcwd()
      block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
      generator = block_blob_service.list_blobs(container)
      for blob in generator:
          if blob.properties.content_length != 0:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. <span data-ttu-id="c2936-143">Ügyeljen arra, hogy illessze be a megfelelő értékeket a tárfiók nevét és a kulcs hívásában `startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="c2936-143">Be sure to paste the appropriate values for your storage account name and key in the call to `startProcessing`.</span></span>

## <a name="run-the-scripts"></a><span data-ttu-id="c2936-144">A parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="c2936-144">Run the scripts</span></span>
1. <span data-ttu-id="c2936-145">Nyisson meg egy parancssort, a Python a elérési úttal, és futtassa az előfeltételként szükséges Python-csomagok parancsok:</span><span class="sxs-lookup"><span data-stu-id="c2936-145">Open a command prompt that has Python in its path, and then run these commands to install Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="c2936-146">Ha egy korábbi vagy az azure-tároló, vagy az azure, szükség lehet használni a **--frissítése** beállítás</span><span class="sxs-lookup"><span data-stu-id="c2936-146">If you have an earlier version of either azure-storage or azure, you may need to use the **--upgrade** option</span></span>
   
  <span data-ttu-id="c2936-147">Előfordulhat, hogy szükség futtassa a következő (a legtöbb rendszeren nem szükséges):</span><span class="sxs-lookup"><span data-stu-id="c2936-147">You might also need to run the following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="c2936-148">Módosítsa a könyvtárat, ahol csak menteni szeretné sender.py és capturereader.py, és futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="c2936-148">Change your directory to wherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="c2936-149">Ez a parancs futtatásához a küldő új Python folyamat elindul.</span><span class="sxs-lookup"><span data-stu-id="c2936-149">This command starts a new Python process to run the sender.</span></span>
3. <span data-ttu-id="c2936-150">Most Várjon néhány percet a rögzítési futtatásához.</span><span class="sxs-lookup"><span data-stu-id="c2936-150">Now wait a few minutes for the capture to run.</span></span> <span data-ttu-id="c2936-151">Írja be a következő parancsot az eredeti parancs ablakba:</span><span class="sxs-lookup"><span data-stu-id="c2936-151">Then type the following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="c2936-152">A rögzítési processzor a helyi címtárszolgáltatásban töltheti le a fiókot vagy a tároló összes blobjának használja.</span><span class="sxs-lookup"><span data-stu-id="c2936-152">This capture processor uses the local directory to download all the blobs from the storage account/container.</span></span> <span data-ttu-id="c2936-153">Feldolgozza azokat, amelyek nem üres, és kiírja az eredményeket a .csv fájlok a helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="c2936-153">It processes any that are not empty, and writes the results as .csv files into the local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2936-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c2936-154">Next steps</span></span>

<span data-ttu-id="c2936-155">Az alábbi webhelyeken további információt talál az Event Hubsról:</span><span class="sxs-lookup"><span data-stu-id="c2936-155">You can learn more about Event Hubs by visiting the following links:</span></span>

* <span data-ttu-id="c2936-156">[Az Event hubs – áttekintés rögzítése][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="c2936-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="c2936-157">[Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="c2936-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="c2936-158">Az [eseményfeldolgozás horizontális felskálázása az Event Hubs használatával][Scale out Event Processing with Event Hubs] – példa.</span><span class="sxs-lookup"><span data-stu-id="c2936-158">The [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="c2936-159">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="c2936-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
