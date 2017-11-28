---
title: "Event Hubs rögzítése forgatókönyv aaaAzure |} Microsoft Docs"
description: "A minta által használt hello Azure Python SDK toodemonstrate hello Event Hubs rögzítése funkció használata."
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
ms.openlocfilehash: 1737dcca283711d863aa970db0e80ae71814e666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a><span data-ttu-id="5e69a-103">Event Hubs rögzítése forgatókönyv: Python</span><span class="sxs-lookup"><span data-stu-id="5e69a-103">Event Hubs Capture walkthrough: Python</span></span>

<span data-ttu-id="5e69a-104">Event Hubs rögzítése egyik funkciója, amely lehetővé teszi, hogy tooautomatically Event Hubs biztosítanak az event hub tooan az Ön által választott Azure Blob storage-fiók adatainak streaming hello.</span><span class="sxs-lookup"><span data-stu-id="5e69a-104">Event Hubs Capture is a feature of Event Hubs that enables you tooautomatically deliver hello streaming data in your event hub tooan Azure Blob storage account of your choice.</span></span> <span data-ttu-id="5e69a-105">Ez a funkció lehetővé teszi a valós idejű streamelési adatok könnyen tooperform kötegfeldolgozási.</span><span class="sxs-lookup"><span data-stu-id="5e69a-105">This capability makes it easy tooperform batch processing on real-time streaming data.</span></span> <span data-ttu-id="5e69a-106">Ez a cikk ismerteti, hogyan toouse Event Hubs rögzítése a Python.</span><span class="sxs-lookup"><span data-stu-id="5e69a-106">This article describes how toouse Event Hubs Capture with Python.</span></span> <span data-ttu-id="5e69a-107">Event Hubs rögzítése kapcsolatos további információkért lásd: hello [a cikk áttekintése](event-hubs-archive-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5e69a-107">For more information about Event Hubs Capture, see hello [overview article](event-hubs-archive-overview.md).</span></span>

<span data-ttu-id="5e69a-108">Ezt a mintát használ hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello rögzítése funkció.</span><span class="sxs-lookup"><span data-stu-id="5e69a-108">This sample uses hello [Azure Python SDK](https://azure.microsoft.com/develop/python/) toodemonstrate hello Capture feature.</span></span> <span data-ttu-id="5e69a-109">hello sender.py program tooEvent hubok elküldi szimulált környezeti telemetriai adatok JSON formátumban.</span><span class="sxs-lookup"><span data-stu-id="5e69a-109">hello sender.py program sends simulated environmental telemetry tooEvent Hubs in JSON format.</span></span> <span data-ttu-id="5e69a-110">hello eseményközpont beállításai toouse hello rögzítése funkció toowrite az adattárolás tooblob kötegekben.</span><span class="sxs-lookup"><span data-stu-id="5e69a-110">hello event hub is configured toouse hello Capture feature toowrite this data tooblob storage in batches.</span></span> <span data-ttu-id="5e69a-111">hello capturereader.py app majd beolvassa a blobok és eszközönként egy hozzáfűző fájlt hoz létre, majd írja az hello adatokat .csv fájlok.</span><span class="sxs-lookup"><span data-stu-id="5e69a-111">hello capturereader.py app then reads these blobs and creates an append file per device, then writes hello data into .csv files.</span></span>

## <a name="what-will-be-accomplished"></a><span data-ttu-id="5e69a-112">Az oktatóanyag célja</span><span class="sxs-lookup"><span data-stu-id="5e69a-112">What will be accomplished</span></span>

1. <span data-ttu-id="5e69a-113">Hozzon létre egy Azure Blob Storage-fiókot és egy blob tároló belül, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5e69a-113">Create an Azure Blob Storage account and a blob container within it, using hello Azure portal.</span></span>
2. <span data-ttu-id="5e69a-114">Hozzon létre egy Eseményközpontba névtér hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5e69a-114">Create an Event Hub namespace, using hello Azure portal.</span></span>
3. <span data-ttu-id="5e69a-115">Hozzon létre egy eseményközpontba hello rögzítési szolgáltatás engedélyezve van, hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="5e69a-115">Create an event hub with hello Capture feature enabled, using hello Azure portal.</span></span>
4. <span data-ttu-id="5e69a-116">Adatok toohello event hubs egy Python-parancsfájl küldése.</span><span class="sxs-lookup"><span data-stu-id="5e69a-116">Send data toohello event hub with a Python script.</span></span>
5. <span data-ttu-id="5e69a-117">Hello fájlok olvasásának hello rögzítési és dolgozza fel őket másik Python-parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="5e69a-117">Read hello files from hello capture and process them with another Python script.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5e69a-118">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5e69a-118">Prerequisites</span></span>

- <span data-ttu-id="5e69a-119">Python 2.7.x</span><span class="sxs-lookup"><span data-stu-id="5e69a-119">Python 2.7.x</span></span>
- <span data-ttu-id="5e69a-120">Azure-előfizetés</span><span class="sxs-lookup"><span data-stu-id="5e69a-120">An Azure subscription</span></span>
- <span data-ttu-id="5e69a-121">Az aktív [Event Hubs névtér és az event hub.](event-hubs-create.md)</span><span class="sxs-lookup"><span data-stu-id="5e69a-121">An active [Event Hubs namespace and event hub.](event-hubs-create.md)</span></span>

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a><span data-ttu-id="5e69a-122">Azure Storage-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e69a-122">Create an Azure Storage account</span></span>
1. <span data-ttu-id="5e69a-123">Jelentkezzen be toohello [Azure-portálon][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="5e69a-123">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="5e69a-124">Hello portal hello bal oldali navigációs ablaktábláján kattintson **új**, majd kattintson a **tárolási**, és kattintson a **Tárfiók**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-124">In hello left navigation pane of hello portal, click **New**, then click **Storage**, and then click **Storage Account**.</span></span>
3. <span data-ttu-id="5e69a-125">Töltse ki a storage-fiók panelen hello hello mezőket, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-125">Complete hello fields in hello storage account blade and then click **Create**.</span></span>
   
   ![][1]
4. <span data-ttu-id="5e69a-126">Miután meggyőződött arról, hogy hello **központi telepítések sikeres** üzenetet, kattintson az új tárfiók hello és hello hello neve **Essentials** panelen kattintson a **Blobok**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-126">After you see hello **Deployments Succeeded** message, click hello name of hello new storage account and in hello **Essentials** blade, click **Blobs**.</span></span> <span data-ttu-id="5e69a-127">Ha hello **Blob szolgáltatás** panel nyílik meg, kattintson a **+ tároló** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="5e69a-127">When hello **Blob service** blade opens, click **+ Container** at hello top.</span></span> <span data-ttu-id="5e69a-128">Név hello tároló **rögzítése**, majd zárja be hello **Blob szolgáltatás** panelen.</span><span class="sxs-lookup"><span data-stu-id="5e69a-128">Name hello container **capture**, then close hello **Blob service** blade.</span></span>
5. <span data-ttu-id="5e69a-129">Kattintson a **hívóbetűk** a hello bal oldali panelen, és másolja hello hello tárolási fiók nevét és értékét hello **key1**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-129">Click **Access keys** in hello left blade and copy hello name of hello storage account and hello value of **key1**.</span></span> <span data-ttu-id="5e69a-130">Ezen értékek tooNotepad vagy más ideiglenes helyre mentse.</span><span class="sxs-lookup"><span data-stu-id="5e69a-130">Save these values tooNotepad or some other temporary location.</span></span>

## <a name="create-a-python-script-toosend-events-tooyour-event-hub"></a><span data-ttu-id="5e69a-131">A Python parancsfájl toosend események tooyour eseményközpont létrehozása</span><span class="sxs-lookup"><span data-stu-id="5e69a-131">Create a Python script toosend events tooyour event hub</span></span>
1. <span data-ttu-id="5e69a-132">Nyissa meg a kedvenc Python-szerkesztőt, például a [Visual Studio Code][Visual Studio Code].</span><span class="sxs-lookup"><span data-stu-id="5e69a-132">Open your favorite Python editor, such as [Visual Studio Code][Visual Studio Code].</span></span>
2. <span data-ttu-id="5e69a-133">Hozzon létre nevű parancsfájl **sender.py**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-133">Create a script called **sender.py**.</span></span> <span data-ttu-id="5e69a-134">Ez a parancsfájl 200 események tooyour eseményközpont küld.</span><span class="sxs-lookup"><span data-stu-id="5e69a-134">This script sends 200 events tooyour event hub.</span></span> <span data-ttu-id="5e69a-135">Elküldött JSON egyszerű környezeti értékek.</span><span class="sxs-lookup"><span data-stu-id="5e69a-135">They are simple environmental readings sent in JSON.</span></span>
3. <span data-ttu-id="5e69a-136">Illessze be a következő kódot a sender.py hello:</span><span class="sxs-lookup"><span data-stu-id="5e69a-136">Paste hello following code into sender.py:</span></span>
   
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
4. <span data-ttu-id="5e69a-137">Hello megelőző kód toouse, a névtér nevét, a kulcs értékét és a eseményközpont neveként beolvasott hello Event Hubs névtér létrehozása után frissítse.</span><span class="sxs-lookup"><span data-stu-id="5e69a-137">Update hello preceding code toouse your namespace name, key value, and event hub name that you obtained when you created hello Event Hubs namespace.</span></span>

## <a name="create-a-python-script-tooread-your-capture-files"></a><span data-ttu-id="5e69a-138">Hozzon létre egy Python-parancsfájl tooread a rögzítési fájlok</span><span class="sxs-lookup"><span data-stu-id="5e69a-138">Create a Python script tooread your Capture files</span></span>

1. <span data-ttu-id="5e69a-139">Töltse ki a hello panelt, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-139">Fill out hello blade and click **Create**.</span></span>
2. <span data-ttu-id="5e69a-140">Hozzon létre nevű parancsfájl **capturereader.py**.</span><span class="sxs-lookup"><span data-stu-id="5e69a-140">Create a script called **capturereader.py**.</span></span> <span data-ttu-id="5e69a-141">Ez a parancsfájl beolvassa a hello rögzített fájlok, és létrehoz egy fájlt egy eszköz toowrite hello adatok kizárólag az eszköznek.</span><span class="sxs-lookup"><span data-stu-id="5e69a-141">This script reads hello captured files and creates a file per device toowrite hello data only for that device.</span></span>
3. <span data-ttu-id="5e69a-142">Illessze be a következő kódot a capturereader.py hello:</span><span class="sxs-lookup"><span data-stu-id="5e69a-142">Paste hello following code into capturereader.py:</span></span>
   
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
4. <span data-ttu-id="5e69a-143">Lehet, hogy toopaste hello megfelelő értékeket a tárfiók nevét és a kulcsot a hello hívás túl`startProcessing`.</span><span class="sxs-lookup"><span data-stu-id="5e69a-143">Be sure toopaste hello appropriate values for your storage account name and key in hello call too`startProcessing`.</span></span>

## <a name="run-hello-scripts"></a><span data-ttu-id="5e69a-144">Hello parancsfájlok futtatása</span><span class="sxs-lookup"><span data-stu-id="5e69a-144">Run hello scripts</span></span>
1. <span data-ttu-id="5e69a-145">Nyisson meg egy parancssort, a Python a elérési útvonalát a, és futtassa a parancsokat tooinstall Python-csomagokat:</span><span class="sxs-lookup"><span data-stu-id="5e69a-145">Open a command prompt that has Python in its path, and then run these commands tooinstall Python prerequisite packages:</span></span>
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  <span data-ttu-id="5e69a-146">Ha egy korábbi vagy az azure-tároló, vagy az azure, szükség lehet a toouse hello **--frissítése** beállítás</span><span class="sxs-lookup"><span data-stu-id="5e69a-146">If you have an earlier version of either azure-storage or azure, you may need toouse hello **--upgrade** option</span></span>
   
  <span data-ttu-id="5e69a-147">Szükség esetén toorun hello követve (a legtöbb rendszeren nem szükséges):</span><span class="sxs-lookup"><span data-stu-id="5e69a-147">You might also need toorun hello following (not necessary on most systems):</span></span>
   
  ```
  pip install cryptography
  ```
2. <span data-ttu-id="5e69a-148">Módosítsa a directory toowherever sender.py és capturereader.py mentette, majd futtassa ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="5e69a-148">Change your directory toowherever you saved sender.py and capturereader.py, and run this command:</span></span>
   
  ```
  start python sender.py
  ```
   
  <span data-ttu-id="5e69a-149">Egy új Python folyamat toorun hello küldő indítja el.</span><span class="sxs-lookup"><span data-stu-id="5e69a-149">This command starts a new Python process toorun hello sender.</span></span>
3. <span data-ttu-id="5e69a-150">Most Várjon néhány percet a hello rögzítési toorun.</span><span class="sxs-lookup"><span data-stu-id="5e69a-150">Now wait a few minutes for hello capture toorun.</span></span> <span data-ttu-id="5e69a-151">Írja be a következő parancsot az eredeti parancs ablakba hello:</span><span class="sxs-lookup"><span data-stu-id="5e69a-151">Then type hello following command into your original command window:</span></span>
   
   ```
   python capturereader.py
   ```

   <span data-ttu-id="5e69a-152">A rögzítési processzor használ hello helyi könyvtár toodownload hello fiók vagy a tároló az összes hello BLOB.</span><span class="sxs-lookup"><span data-stu-id="5e69a-152">This capture processor uses hello local directory toodownload all hello blobs from hello storage account/container.</span></span> <span data-ttu-id="5e69a-153">Feldolgozza azokat, amelyek nem üres, és kiírja a hello eredményeket csv-fájlként hello helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="5e69a-153">It processes any that are not empty, and writes hello results as .csv files into hello local directory.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5e69a-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5e69a-154">Next steps</span></span>

<span data-ttu-id="5e69a-155">További információ az Event Hubs érhetők el a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="5e69a-155">You can learn more about Event Hubs by visiting hello following links:</span></span>

* <span data-ttu-id="5e69a-156">[Az Event hubs – áttekintés rögzítése][Overview of Event Hubs Capture]</span><span class="sxs-lookup"><span data-stu-id="5e69a-156">[Overview of Event Hubs Capture][Overview of Event Hubs Capture]</span></span>
* <span data-ttu-id="5e69a-157">[Az Event Hubsot használó teljes mintaalkalmazás][sample application that uses Event Hubs].</span><span class="sxs-lookup"><span data-stu-id="5e69a-157">A complete [sample application that uses Event Hubs][sample application that uses Event Hubs].</span></span>
* <span data-ttu-id="5e69a-158">Hello [esemény feldolgozása az Event Hubs kibővítési] [ Scale out Event Processing with Event Hubs] minta.</span><span class="sxs-lookup"><span data-stu-id="5e69a-158">hello [Scale out Event Processing with Event Hubs][Scale out Event Processing with Event Hubs] sample.</span></span>
* <span data-ttu-id="5e69a-159">[Event Hubs – áttekintés][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="5e69a-159">[Event Hubs overview][Event Hubs overview]</span></span>

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-archive-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-overview.md
[sample application that uses Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Scale out Event Processing with Event Hubs]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-45f43fc3
