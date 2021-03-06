BE AWARE
========
This code is for demonstration purposes. It doesn't work. Read only. 

Base API
============================

### Common
__Content negotiation:__  
By default all data is sent and received as JSON. _"Content-Type"_ header should be set to `application/json`  

__Pagination:__  
Requests that return multiple items will be paginated to 25 items by default. You can control it with `start` and `limit` parameters. If `limit` is more than 100, default value will be set.  

__Errors:__  

- If item is not found, `200 OK` and empty result: `{}` or `[]` will be sent.
- Sending invalid JSON will result in a `400 Bad Request` response.
- Sending the wrong type of JSON values will result in a `400 Bad Request` response.
- Sending invalid fields will result in a `422 Unprocessable Entity` response.

Every 4XX response contains JSON with an error object like

```
{
	"errors": [
		{
			"classification": "DeserializationError",
			"message": "invalid character '\\\\' looking for beginning of value"
		}
	]
}
```

### /garments 
Get garments list. Create new garment.  

__Endpoint:__ `http://v2.local/api/garments`  

__Methods:__ 

- GET, POST

__Parameters:__

- `ids`   Coma separated list of ids.
- `start` Skip "n" records.
- `limit` Limit selection. Default value is 50.

__Result:__ Array of garment objects

e.g.:

```
GET /garments?ids=93e92e72-1bdb-436f-bdb9-52dba6c16176

[
	{ 
		/* Garment Object */ 
	},
]

```

### /garments/:id
__Endpoint:__ `http://v2.local/api/garments/:id`  

__Methods:__ 
Get, update and delete certain garment.

- GET, PUT, DELETE

#####  Garment Model

```javascript
[
	{
		
		"id"        : "43a5dbed-4abb-452a-8f82-9a86e555930b",     // Garment Id as a uuid
		"gid"       : "43a5dbed-4abb-452a-8f82-123123123123",     // Group id
		"dummy_id"  : "0ae99696-0e13-4c54-8ad7-d1488dffbf65",
		"name"      : "Some name",
		"size_name" : "M",         // (String) Size, e.g. "XL", "42", etc...

		"sizes" : [
			{
				"id"        : "43a5dbed-4abb-452a-8f82-9a86e555930b",
				"size_name" : "S"
			},
			{
				"id"        : "33a6dbed-4abb-452a-8f82-1a16e666930a",
				"size_name" : "X"
			}  
		],

		"color" : "", // @todo		
		"slot"  : "some string",
		"layer" : 1.0,

		"url_prefix": "//v2.local/assets",

		"assets" : {
			"geometry" : {
				"id"   : "22a6dbed-1aab-452b-8f81-2a16e994120b"
			},
			"placeholder" : {
				"id"      : "542d25c0000000000000000d"
			}			
		},
		
		"materials" : [
			{
				// material object
			},
			
			{
				// material object
			}
		]
		
	},	
]
```

### /user
__Endpoint:__ `http://v2.local/api/user`  


__Methods:__ 

- GET, POST, PUT

__Parameters:__


__Result:__ Object

##### User Model

```javascript
{
	"dummy": {
		"id": "0ae99696-0e13-4c54-8ad7-d1488dffbf65",
		"default": true,
		"assets": {
			"geometry": {
				"id" : "e748f388-36f8-47a2-b012-61f1083b80e7"
			}
		},
		
		"body": {
			"height"    : 174.0,
			"chest"     : 95.0,
			"underbust" : 72.0,
			"waist"     : 61.5,
			"hips"      : 89.0
		},
	},
	
	"history" : [ 
		{
			"id"       : "e748f388-36f8-47a2-b012-61f1083b80e7", 
			"gid"      : "43a5dbed-4abb-452a-8f82-123123123123",
			"dummy_id" : "0ae99696-0e13-4c54-8ad7-d1488dffbf65",
			"name"     : "Some name",
			
				// ... full garment model ...
		},
	],

	// Personal settings for objects. Control with /user/objects API
	/* Unused
	"objects" : [
		{
			"id" : "e748f388-36f8-47a2-b012-61f1083b80e7",
			"assets" : {
				"placeholder" : {
					"id"  : "53b54559fcb05d3238000002",
				}
			}
		},
		{
			"id"     : "f848f388-41f1-43b3-b012-31f1023b11e1",
			"assets" : {
				"placeholder" : {
					"id"  : "53b54559fcb05d3238000002",
				}
			}
		}		
	]
	*/
}

```

To get morphed mannequin, just add all users body parameters to the dummy link, e.g.:

```
GET v2.local/assets/22a6dbed-1aab-452b-8f81-2a16e994120b?height=174.0&chest=95.0&underbust=72.0&waist=61.5&hips=89.0
```

#### Placeholders and History
@ todo description


### /dummies
Create and Get methods for dummies

__Endpoint:__ `http://v2.local/api/dummies`  


__Methods:__ 

- GET, POST

__Parameters:__


__Result:__ Array of Objects or Object for POST

##### Dummy Model

```json

{
	"id"         : "0ae99696-0e13-4c54-8ad7-d1488dffbf65",
	"name"       : "default dummy",
	"default"    : true,
	"url_prefix" : "//v2.local/assets",  

	"assets": {
		"geometry": {
			"id"  : "e748f388-36f8-47a2-b012-61f1083b80e7"
		}
	},
	
	"body": {
		"chest"     : 91.154,
		"underbust" : 77.13,
		"waist"     : 66.71,
		"hips"      : 88.88,
		"height"    : 170
	}
}

```

### /dummies/:id
Update and Delete methods for dummy object

__Endpoint:__ `http://v2.local/api/dummies/:id`  


__Methods:__ 

- GET, PUT, DELETE


__Result:__ Dummy object  

__Example:__

```sh
curl -XPOST -H "Content-Type:application/json" -d '
{
	"name"    : "default dummy", 
	"default" : true, 
	"assets"  : {
		"geometry" : {
			"id" : "e748f388-36f8-47a2-b012-61f1083b80e7"
		}
	}
}' http://v2.local/api/dummies
```

Result:

```json
{
	"id"         : "0ae99696-0e13-4c54-8ad7-d1488dffbf65",
	"name"       : "default dummy",
	"default"    : true,
	"url_prefix" : "//v2.local/assets",  
	"assets": {
		"geometry": {
			"id"  : "e748f388-36f8-47a2-b012-61f1083b80e7"
		}
	},
	"body": {}
}
```

### /materials
Create and Get materials

__Endpoint:__ `http://v2.local/api/materials`  


__Methods:__ 

- GET, POST

__Result:__ Array of objects  

__Expected POST data__: Array of objects


__Material Object__  

```json
{
    "id"   : "4717c27a-9fb0-4d09-ad70-2ae01f85deeb",
    "name" : "Test Material",
    "ka"   : "0.0435 0.0435 0.0435",
    "kd"   : "0.1086 0.1086 0.1086",
    "ks"   : "0.0000 0.0000 0.0000",
    "illum": 6,
    "d"    : "-halo 0.6600",
    "ns"   : "10.0000",
    "ni"   : "1.19713",
    
    "map_ka" : {
    	"id"       : "54390be70000000000000001",
    	"orig_name": "test_300.jpg",
    	"options"  : "-s 1 1 1 -o 0 0 0 -mm 0 1"
    },
    
    "map_kd" : {
    	"id"         : "54390be70000000000000001",
    	"orig_name"  : "test_300.jpg",
    	"options"    : "-s 1 1 1 -o 0 0 0 -mm 0 1"
    }
}
```

__1__. Create Material  

POST /materials `[ {some material data}, {some material data} ]` 

As a result, you will get an array of generated ids. E.g.

```
[
  "48e1f441-1c50-4f17-9c39-360cf5ecc99c",
  "fab465a2-19f0-4067-8216-b456a7742b8f"
]
```

__2__. Use these ids array as a value for `materials` field of a garment object, e.g.: 

```
PUT /garments/4717c27a-9fb0-4d09-ad70-2ae01f85deeb 
{
	"materials" : [
		"48e1f441-1c50-4f17-9c39-360cf5ecc99c",
		"fab465a2-19f0-4067-8216-b456a7742b8f"
	]
}
```

