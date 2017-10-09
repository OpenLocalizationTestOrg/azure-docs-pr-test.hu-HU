
### <a name="cacheskuname"></a>cacheSKUName
hello árképzési szintjének hello új Azure Redis Cache-gyorsítótár.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "hello pricing tier of hello new Azure Redis Cache."
      }
    },

hello sablon meghatározó engedélyezettek ehhez a paraméterhez (Basic vagy Standard), és hozzárendeli az alapértelmezett értéket (alapszintű), ha nincs érték megadva hello értékek. Basic biztosít egy egyetlen csomópont több használható fel too53 GB-os méret.
Standard biztosít két csomópontos elsődleges vagy replika használható fel too53 GB és 99,9 %-os SLA-t több méret.

### <a name="cacheskufamily"></a>cacheSKUFamily
hello termékcsalád hello termékváltozat.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "hello family for hello sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
hello új Azure Redis Cache példány hello mérete. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "hello size of hello new Azure Redis Cache instance. "
      }
    }


hello sablon határozza meg, hogy engedélyezve van ez a paraméter (0, 1, 2, 3, 4, 5 vagy 6) a hello értékeket, és hozzárendeli az alapértelmezett értéket (1), ha nincs érték megadva. A számok toofollowing gyorsítótár méretének felel meg: 0 = 250 MB, 1 = 1 GB-os, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

