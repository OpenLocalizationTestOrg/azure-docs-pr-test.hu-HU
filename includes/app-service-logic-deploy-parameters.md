Az Azure Resource Manager paraméterek megadása értékek keresi toospecify hello sablon telepítésekor. hello sablon tartalmazza az összes hello paraméterértékek nevű paraméterek szakaszban.
Ezeket az értékeket, amelyek eltérőek a hello projektet telepít vagy alapján hello környezet esetében helyez üzembe egy paramétert meg kell határozni. Paraméterek nem adják meg, az értékek, amelyek mindig maradnak hello azonos. Minden paraméter értéke hello sablon toodefine hello erőforrásokat, amelyek telepítéséhez használatos. 

Paraméterek definiálásakor használja hello **Storageaccount_accounttype** mező toospecify, amely a felhasználó értékek biztosíthat a telepítés során. Használjon hello **defaultValue** mező tooassign érték toohello paraméter, ha nincs érték megadva üzembe helyezése során.

Azt ismerteti, minden paraméter hello sablonban.

### <a name="logicappname"></a>logicAppName
hello logic app toocreate hello neve.

    "logicAppName": {
        "type": "string"
    }