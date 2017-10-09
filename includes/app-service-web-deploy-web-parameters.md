Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmazza az összes hello paraméterértékek nevű paraméterek szakaszban.
Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos. Minden egyes paraméterérték hello sablon toodefine hello telepített erőforrások esetén használatos. 

Paraméterek definiálásakor használja hello **Storageaccount_accounttype** mező toospecify, amely a felhasználó értékek biztosíthat a telepítés során. Használjon hello **defaultValue** mező tooassign érték toohello paraméter, ha nincs érték megadva üzembe helyezése során.

Azt ismerteti, minden paraméter hello sablonban.

### <a name="sitename"></a>SiteName
hello web app, hogy kívánja-e toocreate hello neve.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
App Service hello hello neve megtervezése toouse hello a webalkalmazás üzemeltetéséhez.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>Termékváltozat
hello üzemeltetési terv hello tarifacsomagját.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "hello pricing tier for hello hosting plan."
      }
    }

hello sablon hello értékek, amelyeknél engedélyezve van ez a paraméter határozza meg, és hozzárendeli az alapértelmezett értéket (S1), ha nincs érték megadva.

### <a name="workersize"></a>workerSize
hello példányméretének hello üzemeltetési terv (kis, közepes vagy nagy).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

hello sablon határozza meg, amelyeknél engedélyezve van ez a paraméter (0, 1 vagy 2) a hello értékeket, és hozzárendeli az alapértelmezett értéket (0), ha nincs érték megadva. hello értékek toosmall, közepes és nagy felelnek meg.

