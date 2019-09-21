# Práctico de Aprendizaje no supervisado

## Objetivo
Obtener reglas de asociación entre películas en el dataset movielens
(como si fuera recomendación!) (ah! Pero recomendación es no supervisado?).
Aplicar diferentes métricas de ordenamiento.
Hacer un pequeño informe (entre 200 y 500 palabras): https://rpubs.com/vitidN/203264

## Descripcion
Se realizaron 2 notebooks con el objetivo de ejecutar localmente
y usando el nabucodonosor CCAD.
Primero se procesaron los datos de movies.csv. Basandome en
https://rpubs.com/vitidN/203264 realice el split de generos
en columnas definiendo variables dummies. Hecho esto se sumarizo y se conto
la cantidad de peliculas en la que aparece cada etiqueta.

![alt text](https://github.com/kevinsen/Movielens_Asoc_apriori/edit/master/genre_count.png)
Luego de jugar con el set de movies se leyo el dataset de rankings
cuyo size es de (20000263, 4). Precedi a dropear la columna de timestamp ya
que no parece util para mi asociación.
Este dataset representa las puntuaciones que los usuarios dieron a las
diferentes peliculas. Entonces la asociación que busque es entre usuarios
y ratings positivos (mayores a 4). Por ejemplo {Si a una persona le gusto
la pelicula A entonces le gustara la pelicula B}.

Aqui se dividen los notebooks entre el local y el que se corria en nabu.
Para el local use un sampling del 10% del dataset ranking. Pero trajo problemas
ya que no me devolvia rules en apriori. En el nabu se corrio con el dataset
completo, pero solo los que tenian ratings >= 4

rating
0.5     239125
1.0     680732
1.5     279252
2.0    1430997
2.5     883398
3.0    4291193
3.5    2200156
4.0    5561926
4.5    1534824
5.0    2898660

![alt text](https://github.com/kevinsen/Movielens_Asoc_apriori/edit/master/ratings_count.png)

Se ordeno el dataset por userId y movieId. Eliminando la columna ratings
(ya que no me serviria porque se que estoy evaluando a los mejores puntajes).
Agrupando los datos en peliculas puntuadas por usuarios logro las transactions
obteniendo un total de 131839 trans.
Ya con las transactions ejecute el algoritmo de efficient-apriori con los
siguientes parametros:
min_support = 0.009
min_confidence = 0.4
max_length = 2
min_lift = 5

--costo de procesamiento local
(CPU times: user 15min 53s, sys: 184 ms, total: 15min 53s
Wall time: 15min 53s)

Luego se desarrollo un poco para ordenar los datos y poder tener un output
mas claro aparte de poder guardar en un json los resultados.

Ejemplo de resultados>
- If a person likes Godfather: Part II, The (1974) they will also like Godfather, The (1972) with confidence: 0.8466388344551419 , lift: 5.511827430533378
- If a person likes Back to the Future Part II (1989) they will also like Back to the Future (1985) with confidence: 0.8077935755660874 , lift: 11.882036952923952
- If a person likes Grand Day Out with Wallace and Gromit, A (1989) they will also like Wallace & Gromit: The Wrong Trousers (1993) with confidence: 0.8008849557522124 , lift: 19.377476909784534
- If a person likes Lord of the Rings: The Two Towers, The (2002) they will also like Lord of the Rings: The Fellowship of the Ring, The (2001) with confidence: 0.7863489893565081 , lift: 8.21290219502279
- If a person likes Kill Bill: Vol. 2 (2004) they will also like Kill Bill: Vol. 1 (2003) with confidence: 0.7559465623981753 , lift: 24.859874991272893
- If a person likes Lord of the Rings: The Return of the King, The (2003) they will also like Lord of the Rings: The Fellowship of the Ring, The (2001) with confidence: 0.7542297417631345 , lift: 7.87743760788322
-If a person likes Star Wars: Episode VI - Return of the Jedi (1983) they will also like Star Wars: Episode IV - A New Hope (1977) with confidence: 0.7492728764262808 , lift: 4.466400811826398
- If a person likes Indiana Jones and the Temple of Doom (1984) they will also like Raiders of the Lost Ark (Indiana Jones and the Raiders of the Lost Ark) (1981) with confidence: 0.7214741318214033 , lift: 6.190590827543247
-If a person likes Indiana Jones and the Last Crusade (1989) they will also like Raiders of the Lost Ark (Indiana Jones and the Raiders of the Lost Ark) (1981) with confidence: 0.7178224593078922 , lift: 6.159257742446678
- If a person likes Lord of the Rings: The Return of the King, The (2003) they will also like Lord of the Rings: The Two Towers, The (2002) with confidence: 0.7112892055011378 , lift: 9.156884831956303
