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

#Implementacja TensorFlow PostObjectDetection na Raspberrypi4B

 1. Aktualizacja npm:

 - sudo npm i npm@latest -g

 2. Założenie katalogu Node-red i przejście do niego

 - mkdir nodered
 - cd nodered

 3. Instalacja Node-red

 - sudo apt-get install nodered
 lub 
 - bash <(curl -sL https://raw.githubusercontent.com/node-red/linux-installers/master/deb/update-nodejs-and-nodered)

 4. Czyszczenie Node-red’a (jeśli świeżo postawiony - pomiń).

 - sudo npm remove -g node-red node-red-admin
 - sudo rm -R ~/.node-red

 5. Uruchomienie Node-red, w celu stworzenia katalogu .node i zatrzymanie go po pełnym uruchomieniu.

 - node-red
 CTRL + C (zatrzymanie procesu node-red’a)

 5. Instalacja peers.

 - sudo npm install -g npm-install-peers

 6. Instalacja bibliotek dla Dependencies.

 - sudo apt-get install build-essential libcairo2-dev libpango1.0-dev libjpeg-dev libgif-dev librsvg2-dev

 7. Implementacja TensorFlow.

 - cd .node-red
 - sudo npm install @tensorflow/tfjs-node@^1.4.0 --unsafe-perm=true --allow-root

 8. Uruchomienie Eksploratora Plików i włączenie możliwości widzenia ukrytych plików/katalogów.

 - Prawy przycisk myszy -> "Show hidden" 

 #Edycja pliku custom-binary.sample.json

 #W terminalu:
 - cd node_modules/@tensorflow/tfjs-node

 #W eksploratorze plików:
 - Kolejno udajemy się do katalogu .node-red -> node-mudules -> @tensorflow -> tfjs-node -> scripts -> custom-binary.sample.json
 - Zmieniamy nazwę na: custom-binary.json
 - Usuwamy zawartość i wklejamy:
{
  "tf-lib": "https://s3.us.cloud-object-storage.appdomain.cloud/tfjs-cos/libtensorflow-cpu-linux-arm-1.15.0.tar.gz"
}

 #W terminalu (instalacja paczki wraz ze zmianami i zależnościami).

 - sudo npm install
 
 9. Instalacja węzłów z terminala.

 #W katalogu .node-red:
 - sudo npm install node-red-contrib-tf-function
 - sudo npm install node-red-contrib-tf-model
 - sudo npm install node-red-contrib-post-object-detection
 - sudo npm install node-red-contrib-image-tools
 - sudo npm install node-red-contrib-utils
 - sudo npm install node-red-contrib-browser-utils

 10. Uruchomienie Node-red i instalacja brakujących węzłów.

 - Uruchomiony Node-red pod adresem localhost:1880 -> Manage palette -> Install -> node-red-contrib-browser-utils + node-red-contrib-image-tools itd.
