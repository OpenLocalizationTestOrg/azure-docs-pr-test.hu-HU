
### <a name="cacheskuname"></a><span data-ttu-id="27000-101">cacheSKUName</span><span class="sxs-lookup"><span data-stu-id="27000-101">cacheSKUName</span></span>
<span data-ttu-id="27000-102">hello árképzési szintjének hello új Azure Redis Cache-gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="27000-102">hello pricing tier of hello new Azure Redis Cache.</span></span>

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

<span data-ttu-id="27000-103">hello sablon meghatározó engedélyezettek ehhez a paraméterhez (Basic vagy Standard), és hozzárendeli az alapértelmezett értéket (alapszintű), ha nincs érték megadva hello értékek.</span><span class="sxs-lookup"><span data-stu-id="27000-103">hello template defines hello values that are permitted for this parameter (Basic or Standard), and assigns a default value (Basic) if no value is specified.</span></span> <span data-ttu-id="27000-104">Basic biztosít egy egyetlen csomópont több használható fel too53 GB-os méret.</span><span class="sxs-lookup"><span data-stu-id="27000-104">Basic provides a single node with multiple sizes available up too53 GB.</span></span>
<span data-ttu-id="27000-105">Standard biztosít két csomópontos elsődleges vagy replika használható fel too53 GB és 99,9 %-os SLA-t több méret.</span><span class="sxs-lookup"><span data-stu-id="27000-105">Standard provides two-node Primary/Replica with multiple sizes available up too53 GB and 99.9% SLA.</span></span>

### <a name="cacheskufamily"></a><span data-ttu-id="27000-106">cacheSKUFamily</span><span class="sxs-lookup"><span data-stu-id="27000-106">cacheSKUFamily</span></span>
<span data-ttu-id="27000-107">hello termékcsalád hello termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="27000-107">hello family for hello sku.</span></span>

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


### <a name="cacheskucapacity"></a><span data-ttu-id="27000-108">cacheSKUCapacity</span><span class="sxs-lookup"><span data-stu-id="27000-108">cacheSKUCapacity</span></span>
<span data-ttu-id="27000-109">hello új Azure Redis Cache példány hello mérete.</span><span class="sxs-lookup"><span data-stu-id="27000-109">hello size of hello new Azure Redis Cache instance.</span></span> 

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


<span data-ttu-id="27000-110">hello sablon határozza meg, hogy engedélyezve van ez a paraméter (0, 1, 2, 3, 4, 5 vagy 6) a hello értékeket, és hozzárendeli az alapértelmezett értéket (1), ha nincs érték megadva.</span><span class="sxs-lookup"><span data-stu-id="27000-110">hello template defines hello values that are permitted for this parameter (0, 1, 2, 3, 4, 5 or 6), and assigns a default value (1) if no value is specified.</span></span> <span data-ttu-id="27000-111">A számok toofollowing gyorsítótár méretének felel meg: 0 = 250 MB, 1 = 1 GB-os, 2 = 2,5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span><span class="sxs-lookup"><span data-stu-id="27000-111">Those numbers correspond toofollowing cache sizes: 0 = 250 MB, 1 = 1 GB, 2 = 2.5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB</span></span>

