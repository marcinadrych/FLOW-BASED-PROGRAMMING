#Flow wykorzystujące kamerę IP i bibliotekę Tensor-Flow zbudowane w Node-red

#SmartEye ma na celu powiadomić nas jeżeli ktoś znajdzie się na danym obiekcie w niechcianym przez nas czasie.

#Dodatkowo dzięki temu procesowi, o wiele prościej jest ustalić godzinę, w której działo się coś niepokojącego. 

#Przy budowie flow użyłem biblioteki i aplikacji telegram. Służy ona do powiadomienia nas w aplikacji telegram w trakcie wykrycia (w tym przypadku) człowieka. 
 Otrzymujemy powiadomienie ze zdjęciem z kamery, a tam mamy datę, godzinę i obiekt, który wyzwolił powiadomienie. 
 Dzięki tym danym możemy wejść na monitoring i cofnąć czas do konkretnej godziny, minuty, sekundy i odnaleźć materiał, w którym zaszło do zdarzenia. 

#Nie marnujemy czasu na przeglądanie minuty po minucie nagrań, aż znajdziemy konkretny moment.

#Do budowy tego flow użyłem bibliotek:
 - node-red-contrib-post-object-detection
 - node-red-contrib-telegrambot
 - node-red-contrib-tf-function
 - node-red-contrib-tf-model

#Opis działania flow:
 - Węzeł "inject" automatycznie uruchamia przepływ od razu po uruchomieniu RaspberryPi i Node-red
 - "Http request" wykonuje zapytanie do kamery znajdującej się w sieci lokalnej i wywołuje zrzut. Funkcja "GET" pobiera nam go.
 - "Pre-processing", "COCO SSD LITE" i "post-processing" odpowiadają za przetworzenie zrzutu ekranu i naniesieniu nazw wykrytych obiektów.
 - "json" konwertuje przetworzone dane na dane w postaci json.
 - "switch" odpowiada za zakończenie przepływu w przypadku nie wykrycia danego obiektu(człowieka), lub przechodzi dalej, gdy wykrył obiekt.
 - "bounding-box" nanosi siatkę z zaznaczonym wykrytym obiektem.
 - "function" tu zdefiniowałem przesłanie obrazu i wiadomości tekstowej do aplikacji "Telegram"
 - "Telegram sender" odpowiada za wysłanie powiadomienia do naszej aplikacji znajdującej się na telefonie, czy komputerze.
