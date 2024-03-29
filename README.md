# BilbaoBizi, el servicio municipal de préstamo de bicicletas de Bilbao.

### TODO ###

Aplicar ML para la prevision de bicis disponibles y detectar cual seria la estacion mas cercana con mas posibilidades de que haya bicis.

### Introduction ###

En este proyecto haremos uso de la API del proveedor del servicio sacada de "North American Bike Share Association" [NABSA](https://github.com/NABSA/gbfs).

En nuestro caso atacaremos a la [APIRest](https://gbfs.nextbike.net/maps/gbfs/v1/nextbike_bo/gbfs.json) de NextBike del servicio Bilbaobizi que nos proporciona informacion en json sobre el servicio de prestamos de bicicletas, a destacar que los datos no nos los proporciona en vivo sino con una hora de delay...

### Requirements ###
- Elasticsearch > v6.0
- Logstash > v6.0
- Kibana > v6.0
- Licencia "Basic" en elastic si quieres hacer zoom en el mapa de más de x10 (es gratis)

### Running ###


#### Import index template.

*Via devtools...*

PUT _template/bilbaobizi 
{ 
"order" : 0, "index_patterns" : [ "bilbaobizi*" ], "settings" : { "index" : { "number_of_shards" : "1" } }, "mappings" : { "properties" : { "location" : { "type" : "geo_point" } } }, "aliases" : { } 
}

OR

*Via terminal...*

curl -XPUT "http://localhost:9200/_template/bilbaobizi" -H 'Content-Type: application/json' -d'{    "order" : 0,    "index_patterns" : [      "bilbaobizi*"    ],    "settings" : {      "index" : {        "number_of_shards" : "1"      }    },    "mappings" : {      "properties" : {        "location" : {          "type" : "geo_point"        }      }    },    "aliases" : { }  }'

#### Running logstash
/usr/share/logstash/bin/logstash -f logstash.conf


### Import Objects ###

Ahora vamos a importar los objetos (busquedas,visualizaciones,dash, etc..) en kibana, para ello:

- Management > Kibana > Index Patterns, añade el patron bilbaobizi-* y el campo de tiempo @timestamp .
- Management > Kibana > Saved Objects, importa kibana.ndjson** .

Te pedira sobreescribir el patron y alguna cosa más, dale a que **si**.

**el archivo kibana_new.ndjson incluye un nuevo mapa totalmente renovado.

### Let´s go! ###
Asi es como deberia de quedarte, ahora ya sabes a que hora tienes bicis al lado de tu casa :)

![imagen_resultado](https://github.com/igorneos/bilbaobizi/blob/master/example_new.gif)
