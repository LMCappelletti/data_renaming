Día: 24/04/2025

Versión 1 renaming

INPUTS:

- 'georef_keys.txt': ['country_name_abbreviation', 'pais', 'partner_name_short_en', 'region', 'iso3_desc', 'partner_region_name_short_es', 'region_code', 'partner_region_id', 'pais_o_region', 'pais_o_grupo_de_paises', 'iso3', 'codigo_pais', 'country_code', 'partner_code', 'iso3c', 'countryname', 'nombre_pais', 'region_name', 'iso3_desc_fundar', 'pais_nombre']
- geonomenclador.csv 

--------------------

- Ya detectadoa los .csv por tópico de argendata que tengan columnas que se llamen según 'georef_keys.txt
- Ya detectados los .csv por tópico de argendata que tengan columnas que cumplan: si al menos el 80% de los valores (*) están contenidos en el conjunto de valores de la columna del geonomenclador (eliminando nulos y homogeneizando temas: espacios "NEA "|"NEA", mayúsculas: "pampeana" | "Pampeana", tildes: "AFGANISTÁN" | "AFGANISTAN" )

- Registro de que tiene el geonomenclandor.csv (según coincidencia 80%) pero no el 'georef_keys.txt' [df_GeoNomYes_GeoKeysNo]
        con eso se contribuye a sumar nombres de POSIBLES columnas georreferenciales a 'georef_keys.txt'

- Registro de que tiene 'georef_keys.txt' pero no la condición geonomenclandor.csv (según coincidencia 80%) [df_GeoKeysYes_GeoNomNo]
        - acá me gustaría que haya, hasta el momento, solo NO detección de columnas en idiomas distintos al español:
                pero hay otros problemas (archivosArgendata_problemaRenaming.ods)
        - geonomenclador tiene un tema con los geocódigos, un "mismo" name_lon por tiene distinto geocodigos:
                ejemplo "América del Norte" tiene 3 geocódigos distintos...
        - si algo dice "Europa" solo, y en el geonomenclados hay más de 5 opciones que contienen "Europa"... cuál tomamos?:
                porque podría "jugar" con una detección de que si tal palabra está contenida en name_lon considero esa fila... pero "Europa" está en más de una opción
        - si la columna iso3 aparece vamos bien de todas maneras
        

- tema condicion geonomenclador 80% (detección de .csv con problemas), la bajamos a 60 %? (lo quioero explorar)
- geonomenclador tiene un tema con los geocódigos, por ejemplo "América del Norte" tiene 3 geocódigos distintos
- si algo dice "Europa" solo, y en el geonomenclados hay más de 5 opciones que contienen "Europa"... cuál tomamos?

- tema idioma (detección de .csv con problemas)
- los csv con tema idioma + que muchos no estarán en el geonomenclador los quiero ver en detalle -> a ver si al aplicar tema idioma igual cumple el 
- tema encoding, guardado (proceso renaming)


- multiples codigos para un america del norte, dejo registrado que unico codigo para ese string -s hago uno para uno
- si aparece un codigo en el csv, le mando csv
- si no tengo codigo, le asigno 

PRODUCTO DE ESTO: v1_archivosArgendata_problemaRenaming.ods -> DETALLE DE ARCHIVOS CON GEORREFERENCIA QUE NO SON DETECTADOS Y SU POR QUÉ

--------------------------------------------------------------------------------
05/05/2025

Cómo seguir:

Quedaron las siguientes definiciones para las imputaciones:

   - Si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long
   - Si un dataset tiene un código que NO este incluido en los geocodigos y tiene una string que matchee sin ambigüedad en desc_fundar, entonces se usa el código que está en el geonomenclador (y se guarda la asociación del codigo encontrado con el codigo usado).
   - Si un dataset tiene un código que NO este incluido en los geocodigos y tiene una string que NO matchea sin ambigüedad en desc_fundar, se genera un código nuevo (a revisión para ser desambiguado con posibles matches)
   - Si un dataset tiene una string que matchee sin ambigüedad en desc_fundar entonces se usa el código que está en el geonomenclador para esa string.
   - Si un dataset tiene una string que NO matchee sin ambigüedad en desc_fundar, entonces se genera un código nuevo (a revisión para ser desambiguado con posibles matches)

voy a usar como input de geonomeclador: geonomenclador_LuloTest.csv (viene a ser la pestaña front de https://docs.google.com/spreadsheets/d/1fAQ3rmE3Fr6ztUKXvDYJw0iGH-Vidrx8wYkkHEgBBIQ/edit?gid=1603974855#gid=1603974855)

Tema a resolver:

la pestaña front del excel geonomenclador tiene 4 columnas que son: geocodigo | desc_fundar | name_long | name_short
Pensaba usar esa pestaña para el renaming pero, si no estoy pifiando, no estaría bien porque la columna name_long no está completa. Por ejemplo, el geocodigo GUY se corresponde al desc_fundar Guyana, pero su respectiva fila de la columna name_long  está vacia (adjunto captura). 
Esto lo vi básicamente porque cuando estaba trabajando en el bullet  "si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long", casos como este, el name_long del renaming quedaba vacio.

La pestaña export tiene las columnas: 
geocodigo | name_long | name_short | iso_2
Y en ese caso el geocodigo GUY se corresponde al name_long Guyana

Entonces, cambio el bullet ¿"si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long" por "si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long (si es que existe), y si no es así se nomencla con la string de desc_fundar correspondiente"?

--------------------------------------------------------------------------------
12/05/2025

¿"si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long" por "si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long (si es que existe), y si no es así se nomencla con la string de desc_fundar correspondiente"? me dieron el ok asi que las tareas quedan: 

   - si un dataset tiene un código que esté incluido en los geocodigos del geonomenclador, se nomencla con la string de name_long (si es que existe), y si no es así se nomencla con la string de desc_fundar correspondiente
   - Si un dataset tiene un código que NO este incluido en los geocodigos y tiene una string que matchee sin ambigüedad en desc_fundar, entonces se usa el código que está en el geonomenclador (y se guarda la asociación del codigo encontrado con el codigo usado).
   - Si un dataset tiene un código que NO este incluido en los geocodigos y tiene una string que NO matchea sin ambigüedad en desc_fundar, se genera un código nuevo (a revisión para ser desambiguado con posibles matches)
   - Si un dataset tiene una string que matchee sin ambigüedad en desc_fundar entonces se usa el código que está en el geonomenclador para esa string.
   - Si un dataset tiene una string que NO matchee sin ambigüedad en desc_fundar, entonces se genera un código nuevo (a revisión para ser desambiguado con posibles matches)

14/05/2025 (continúo lo del 12/05/2025)

- pruebo con 60% en lugar de 80%
- de los 27 .csv que tenían "problemas" (no se detectaba columna georreferencia) ahora solo 3 mantienen problema) 


15/05/2025

- queda poner para todos los tópicos en produccion (generacion de carpetas nuevas, los csvs, etc)
- registrar problemas que quedaron dando vueltas (del excel y del procesador de texto)
- categorizar problemas reunion 29, ir categorizando problemas