# ESP-Microtroleur
# 📡 TP ESP32 – Envoi de données en WiFi

![ESP32](https://upload.wikimedia.org/wikipedia/commons/3/3b/ESP32_DevKitC_v4.jpg)

---

## 🎯 Objectif

Ce TP permet de comprendre comment un **ESP32** peut envoyer des données vers un serveur web en WiFi, puis comment ces données peuvent être stockées et affichées sur une page web.

---

## ⚙️ Fonctionnement global

```txt
ESP32 → HTTP POST → Serveur Ubuntu (PHP) → fichier data.txt → page web
```

---

## 📚 Étapes du projet

1. L’ESP32 lit une valeur (capteur ou valeur aléatoire)
2. Connexion au WiFi
3. Envoi des données via requête HTTP (POST)
4. Le serveur PHP reçoit la donnée
5. Stockage dans un fichier `.txt`
6. Affichage sur une page web

---

## 📶 Connexion WiFi

### 🔧 Configuration

```txt
SSID : TPSN035
Mot de passe : BTSSN2022
```

---

## 🧰 Matériel

- ESP32
- Câble USB
- Ordinateur avec Arduino IDE
- Serveur Ubuntu (Apache + PHP)

---

## 💻 Code ESP32 (Arduino)

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

const char* ssid = "TPSN035";
const char* password = "BTSSN2022";
const char* serverName = "http://<IP_UBUNTU>/btsciel/data.php";

void setup() {
  Serial.begin(115200);
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("\nConnecté au WiFi !");
  Serial.println(WiFi.localIP());
}

void loop() {
  if (WiFi.status() == WL_CONNECTED) {

    int valeur = random(0, 100);

    HTTPClient http;
    http.begin(serverName);
    http.addHeader("Content-Type", "application/x-www-form-urlencoded");

    String data = "valeur=" + String(valeur);

    int code = http.POST(data);

    Serial.print("Code HTTP : ");
    Serial.println(code);

    http.end();
  }

  delay(5000);
}
```

---

## 🖥️ Code serveur PHP

📁 `/var/www/html/btsciel/data.php`

```php
<?php

if(isset($_POST['valeur'])) {
    $valeur = $_POST['valeur'];

    file_put_contents("data.txt", $valeur);

    echo "Valeur reçue : " . $valeur;
} else {
    echo "Aucune donnée reçue";
}

?>
```

---

## 🌍 Accès serveur

- 📡 Envoi des données :
```
http://IP_UBUNTU/btsciel/data.php
```

- 📊 Affichage (optionnel) :
```
http://IP_UBUNTU/btsciel/index.php
```

---

## 📖 Rappels HTTP

| Méthode | Rôle |
|----------|------|
| GET | Lire des données |
| POST | Envoyer des données |
| PUT | Modifier des données |

---

## 📄 Page statique vs dynamique

- 🟦 **HTML** : page fixe  
- 🟩 **PHP** : page dynamique (réagit aux données)

---

## ✅ Résultat attendu

✔ L’ESP32 envoie des données  
✔ Le serveur les stocke dans `data.txt`  
✔ Une page web peut les afficher  

---

## 🚀 Améliorations possibles

- 🌡️ Ajouter un vrai capteur (température, humidité…)
- 📊 Créer un graphique (Chart.js)
- 🗄️ Utiliser une base de données MySQL
- 🔒 Sécuriser avec HTTPS
- ⏱️ Envoyer les données automatiquement (timer)

---

## 👨‍💻 Auteur

Projet TP – BTS CIEL
