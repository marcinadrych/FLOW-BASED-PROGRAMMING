#Flow wykorzystujące API, RabbitMQ i Postgresql

#Przetwarzanie danych rozpoczyna się o zapytania do API, które w tym przypadku testowym pobiera randomowe nazwy, smaki, odmiany kawy.
Następnie wszystkie informacje zostają wysłane do RabbitMQ - dane zostają dodane do kolejki.
Kolejna część przepływu odbiera dane z kolejki i zmienia ciąg string na obiekt typu json.
Na końcu funkcja wyciąga nam na potrzeby testu tylko nazwę losowej kawy i dodaje je do lokalnej bazy danych. 